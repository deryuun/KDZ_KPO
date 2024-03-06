
# Restourant

Restourant — это система управления и пользования рестораном.

## Описание проекта

Этот проект представляет собой систему управления заказами для ресторана, которая позволяет пользователям регистрироваться как администраторы или посетители. Администраторы имеют возможность управления меню ресторана, в то время как посетители могут взаимодействовать с системой для размещения и управления своими заказами.

### Эндпоинты

#### Администратор (`Admin` класс)

- **addDish()**: Добавляет новое блюдо в меню.
- **editDish()**: Редактирует информацию о существующем блюде.
- **deleteDish()**: Удаляет блюдо из меню.
- **revenue()**: Считает сумму выручки.
- **showFeedbacks()**: Показывает отзывы.
- **usersInSystem()**: Показывает всех пользователей в системе.

#### Посетитель (`Visitor` класс)

- **makeOrder()**: Создает новый заказ.
- **addDish()**: Добавляет блюдо к существующему заказу.
- **cancelOrder()**: Отменяет существующий заказ.
- **payOrder()**: Оплачивает заказ.
- **feedback()**: Оставляет отзыв о заказе.

#### Общие действия для всех пользователей (`Funcs` класс)

- **logIn()**: Вход в систему.
- **logOut()**: Выход из системы.
- **visitorFuncs()**: Доступные действия для посетителей.
- **adminFuncs()**: Доступные действия для администраторов.

### Система регистрации и авторизации

Пользователи могут регистрироваться в системе как администраторы или посетители и использовать свои учетные данные для входа в систему.

### Управление меню

Администраторы могут добавлять, редактировать и удалять блюда в меню, просматривать сумму выручки, отзывы и пользователей в системе, что позволяет динамически управлять предложением ресторана.

### Управление заказами

Посетители могут размещать заказы, добавлять к ним блюда, отменять неоплаченные заказы и оплачивать их. После оплаты заказа посетители могут оставить отзыв.

### Паттерны

#### Шаблонный метод

Метод addOrderToDB() представляет собой пример шаблонного метода. В нем определен основной алгоритм добавления заказа в базу данных, но конкретные шаги этого алгоритма могут быть изменены или расширены в подклассах.

Методы addDish(), payOrder(), feedback(), makeOrder(), cancelOrder(), showOrderStatus() представляют собой примеры шаблонного метода.
В этих методах определен общий алгоритм выполнения операции (добавление блюда в заказ, оплата заказа, оставление отзыва и т.д.), но некоторые шаги этого алгоритма могут быть изменены или дополнены в подклассах (в данном случае подклассом является класс Visitor).
Такой подход позволяет избежать дублирования кода и обеспечивает единообразное выполнение операций в различных контекстах.

#### Фасад

Класс Visitor является фасадом для взаимодействия пользователя с рестораном. Он предоставляет удобный интерфейс для работы пользователя с различными операциями (добавление блюда, оплата заказа и т.д.).
Фасад скрывает сложность внутренней реализации и предоставляет простой интерфейс взаимодействия с системой, что делает код более понятным и удобным для использования.

Класс User выступает в роли фасада для взаимодействия с базой данных и аутентификации пользователя.
Методы registration() и authentication() предоставляют удобный интерфейс для регистрации нового пользователя и аутентификации существующего.

#### Стратегия

В классе Funcs методы visitorFuncs() и adminFuncs() представляют собой различные стратегии обработки действий в зависимости от статуса пользователя.
В зависимости от статуса пользователя (посетитель или администратор), вызывается соответствующий набор функций для обработки его запросов.
Этот подход позволяет изолировать и разделить различные наборы функций для разных типов пользователей, делая код более модульным и легко расширяемым.

### Использование программы

#### Установка JDBC драйвера:

Сначала вам нужно загрузить JDBC драйвер (в файле с проектом уже есть папка libs, в которой хранится JDBC драйвер) для базы данных (PostgreSQL). Вы можете найти JDBC драйвер для PostgreSQL на официальном сайте PostgreSQL. Скачайте соответствующий JDBC драйвер JAR файл.

#### Добавление JDBC драйвера в проект:

После загрузки JDBC драйвера вам нужно добавить его в ваш проект. 

Если вы используете IntelliJ IDEA:
- поместить его в отдельную папку
- в Intellij IDEA перейти в "File" -> "Project Structure" ->  "+ (New project library)" -> "Java" -> выбираем нашу папку -> "Apply"

Если вы используете Maven, добавьте зависимость JDBC в файл pom.xml.

#### Создание базы данных:

Запустите базу данных (PostgreSQL) и создайте новую базу данных. Вы можете использовать инструмент управления базами данных, такой как pgAdmin, или выполнить SQL запрос через командную строку.

#### Создание таблиц:

Для моей программы нужно создать пять таблиц в базе данных. Вот SQL запросы, которые надо вставить: 

CREATE TABLE IF NOT EXISTS users (
                       id SERIAL PRIMARY KEY,
                       username VARCHAR(255) NOT NULL UNIQUE,
                       password VARCHAR(255) NOT NULL,
                       status VARCHAR(50)
);

CREATE TABLE IF NOT EXISTS dishes (
                        id SERIAL PRIMARY KEY,
                        title TEXT NOT NULL,
                        amount INTEGER NOT NULL,
                        price INTEGER NOT NULL,
                        cook_time INTEGER NOT NULL

);

CREATE TABLE IF NOT EXISTS orders (
                        id SERIAL PRIMARY KEY,
                        user_id INTEGER REFERENCES users(id),
                        total_price INTEGER NOT NULL,
                        status VARCHAR(50) NOT NULL
);

CREATE TABLE IF NOT EXISTS order_items (
                             id SERIAL PRIMARY KEY,
                             order_id INT,
                             dish_name VARCHAR(255),
                             quantity INT,
                             FOREIGN KEY (order_id) REFERENCES orders(id)
);

CREATE TABLE IF NOT EXISTS feedback (
                             id SERIAL PRIMARY KEY,
                             order_id INT NOT NULL,
                             rating INT NOT NULL,
                             comment TEXT,
                             created_at TIMESTAMP NOT NULL
);

#### Просмотр данных в таблицах

Для того, чтобы псмотреть какие данные хранятся в таблицах, введите следующий SQL запрос: 

SELECT * FROM table_title; (вместо table_title название вашей таблицы)