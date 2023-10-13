# Для будущих моповцев

DL это вычисление с матрицами, например перемножение. А перемножение можно распараллеливать. Пусть у клиента нет своих ресурсов чтобы обучить модель поэтому он просто хочет описать граф (DAG) вычислений и отдать это сервису. Обновить веса он сможет сам (или как бонус).

## Формат графа

Каждая вершина графа - либо перемножение матриц, которое можно распараллеливать, либо какая-то нелинейная функция, которая может быть задана опять же путем до бинарника, который пользователь может изначально загрузить в систему. Так же есть стандартные функции например max(по столбцам, строкам), sum(всех элементов таблицы).

Ребра - связи между задачами. Результаты всех родителей просто конкатенируем по столбцам (то есть увеличивается количество столбцов). Если у задачи-вершины несколько потомков, то данные копируются в каждого ребенка.

Граф может быть описан в форматах dot, json, yaml, а как именно - задайте сами.

## Сервер

Нужно запускать вычисления в вершине графа в любом порядке обхода (пытаться это оптимизировать не обязательно), сохранять результат, объединять его с другими, передавать на вход следующему вычислению.

Перемножение матрицы должно быть распараллелено, сделайте это тремя способами и сравните время работы (следующий пункт):
1. только через потоки
2. только через SSE
3. в каждом потоке используйте SSE

Пользователю отправлять результат.

## Обязательная работа чтобы показать ценность своего решения

Сначала написать решение без параллельности - все в один поток для одного пользователя. Нужно будет нагенерировать парочку графов (генерировать можно руками или я могу скинуть репу где генерируется рандомная структура графа, но наполнить его нужно самостоятельно) и замерить время работы на них. После - замерить время с распараллеливанием. Нарисовать графики зависимости от размера графа + процент матриц в графе.

Также провести зависимость работы от количества пользователей на сервера для лучшего из решений. Каждому пользователю можно давать один и тот же граф для вычислений.

Также можно проводить любые оптимизации, но нужно повторить сравнение на тестовых графах.

~~Опубликовать статью~~

Графики должны быть красивыми, то есть иметь название, подписанные оси, легенду если нужно, желательно сетку. Если у данных разный порядок, то шкала должна быть логарифмированной. Диапазон значений должен быть разумный. Рисовать графики можно не на с, но только рисовать.

## Клиент

Пользователь может просто описать граф. Нужно поддерживать форматы dot, json, yaml, но форматы графов задайте самостоятельно. Так как у нас клиент делает всю работу по обучению, то запросы на сервер должны быть достаточно интенсивными, поэтому закрывать соединение с клиентом нужно по timeout/запросу от клиента.

Также клиент будет в основном только обновлять веса, поэтому должна быть возможность по информации об изменении весов (в любом виде, все те же разрешения файлов или даже внутри запроса), не создавая новых структур, просто поменять текущие и запустить еще раз.

## Соединение с клиентом

Если клиентов слишком много, то клиенты должны ставится в очередь, и в ответе пользователю указывать, сколько людей до него.

Если первый необработанный клиент _A_ ждет слишком долго, то нужно поставить на паузу кого-то из активных - _B_, сохранить его поток (стек и текущую инструкцию), и на его место на некоторое время назначить нового клиента _A_. Затем _A_ поставить в середину очереди, и продолжить _B_. Если соединение c _A_ не должно быть закрыто сразу, то просто сохранить данные этого клиента.

Соединение с клиентом закрывается автоматически, если он не отправлял новых запросов какой-то таймаут.

Для общения клиентской и серверной частей все данные должны сохраняться в структуры. Они должны сериализироваться в последовательность байт и расшифровывались из нее.

## Что-то из высокого искусства

Один из принципов проектирования usability продукта - пользователь должен понимать, что происходит сейчас с его запросом, что может отменить его, и понимать что ему пишет сервер. Мы не будем проводить исследования, но если есть несколько вариантов что-то сделать, выбирайте тот, который по вашему ощущению как пользователя будет удобнее использовать. А это означает, что будет также проверять

- на любой запрос клиенту (с любым действием не только начальной отправкой) нужно написать ответ от сервера - получилось ли что-то сделать или ошибку
- пользователь может захотеть узнать прогресс - нужно ответить короткой информацией (если вдруг граф выполняется слишком долго)
- пользователь может отменить свой запрос, закрыть соединение
- пользователь может захотеть восстановить свой запрос, восстановить соединение
- пользователь может спросить, что вообще умеет делать сервер
- пользователь может спросить, что происходит в целом на сервере, его нагрузка

## Мониторинг сервера

При этом у сервера есть админ, которому нужно собирать статистики по клиентам, чтобы если что добавлять ресурсы, менять железо, или добавить премиум подписку на какие-то тяжелые графы, если нагрузка на его сервис будет слишком большой (сейчас этого нет).

Админ также может остановить сервер через отправку сигнала (сервер должен сохранить все промежуточные данные), или убивать сервер (мгновенная смерть), восстанавливать его, а также перезагружать (просто чтобы была одной командой). Можете сделать отдельный модуль прямо для этой работы. Вам нужно будет самостоятельно написать инструкцию для админа (на какие сигналы что будет происходить).

# По вопросам что здесь происходит

Если хотите, спрашивайте в общей группе, уточнить написанное условие - @KseniaPetrenko.