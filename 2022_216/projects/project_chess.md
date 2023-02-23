# Игра в шахматы!

---

Проект свободен: нет

Участники: Афанасьев Олег, Иванов Павел, Судаков Илья

Предложен студентами: да

Ссылка на репозиторий: https://github.com/anomaliyamai/caos_seminar_project

---

**Задача**

Жесткие ботари очень любят расслабиться по-ботарьски. Самое лучшее времяпровождение - это размять свои мозги в игре шахматы с другими ботарями!

**Функционал**

Сто процентов будет графический интерфейс (меню и все остальное). Пользователь будет взаимодействовать с ним с помощью компьютерной мышки (то есть устройство ввода - мышь, устройство вывода - монитор). Будет возможность играть по сети и еще несколько фич по вдохновению авторов (некоторые уже описаны в интерфейсе для пользователя). Сама игра будет представлять из себя классические шахматы по всем канонам.

**Стек технологий**
1) [C++.](https://ru.wikipedia.org/wiki/C%2B%2B)
2) [Simple and Fast Multimedia Library C++ Edition.](https://www.sfml-dev.org/)
3) Возможно другие!!!!!!!

**Крутые способности:**
1) В игру можно будет играть по сети (sfml network).
2) Во время игры пользователи смогут писать текстовые сообщения друг другу и отправлять реакции.
3) Будут так же звуки ходов, важных событий, возможна фоновая музыка.

**Интерфейс для пользователя:**
1) Игроки должны выбрать себе nickname.
2) Игрокам будет предложен цвет, за который они будет играть (возможно будет что-то кроме черного и белого), и в зависимости от выбраных цветов, определяется порядок в котором ходят игроки.
3) Время каждого игрока на обдумывание будет засекаться как в классических шахматах.
4) Чтобы игрок походил определенной фигурой, он должен нажать на нее, после этого будут подсвечены поля, куда она может сходить, на одно из подсвеченных полей должен будет нажать игрок. Если игрок нажмет на неподсвеченную клетку, то игра подскажет, что ход некорректный. Если игрок еще не сходил данной фигурой и передумал ей ходить, то он должен нажать повторно на выбранную фигуру.
5) Игра будет проходить по правилам классических шахмат, в случае достижения пешкой границы, будет предложен обмен пешки на другую фигуру, будут показаны доступные фигуры (картинки), на которую нужно будет нажать.
6) В случае победы/поражения игрока, на экране будет появляться соответствующее сообщение (картинка).
7) Будут присутствовать примитивные звуки при взаимодействии пользователя с интерфейсом.
8) Также будут доступны некоторые дополнительные режимы (Шашки Mode, Random расположение фигур Mode, Игра с самим собой - и другие по мере вдохновения авторов).
