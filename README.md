# Osnova cyberimun system


Приложение "SecureVault"

Описание:
SecureVault - это веб-приложение, предназначенное для безопасного хранения и управления личными данными пользователей, такими как пароли, номера кредитных карт и личные заметки. Пользователи могут создавать учетные записи, добавлять свои данные в защищенные хранилища и получать к ним доступ с любого устройства.

Архитектура:

![1](https://github.com/user-attachments/assets/ed9b8ea7-5906-4dca-b985-7d7dd1f509df)

Компоненты:

* Клиент: Веб-интерфейс, написанный на HTML, CSS и JavaScript. Использует REST API для взаимодействия с сервером.
* Сервер: 
    * Приложение:  Backend написан на Node.js с использованием фреймворка Express.js. Обрабатывает запросы от клиента, взаимодействует с базой данных и выполняет бизнес-логику.
    * База данных: Хранит данные пользователей, включая учетные записи, данные хранилищ и сессии. Используется MongoDB.
* Балансировщик нагрузки: Распределяет трафик между несколькими экземплярами сервера для обеспечения масштабируемости.
* CDN: Кэширует статические ресурсы (HTML, CSS, JS) для улучшения производительности.

Уязвимости:
1. SQL-инъекция в запросах к базе данных: 
    * Описание: Приложение использует небезопасные методы формирования SQL-запросов, что позволяет злоумышленнику внедрить вредоносный код в запрос.
    * Влияние: Злоумышленник может получить доступ к конфиденциальным данным пользователей, таким как пароли и номера кредитных карт.
2. Межсайтовый скриптинг (XSS) в веб-интерфейсе:
    * Описание: Приложение не экранирует пользовательский ввод, что позволяет злоумышленнику внедрить вредоносный JavaScript-код на страницы сайта.
    * Влияние: Злоумышленник может украсть сессионные куки пользователей, получить доступ к их учетным записям и выполнить действия от их имени.
3. Отсутствие шифрования данных при хранении:
    * Описание: Данные пользователей хранятся в базе данных в незашифрованном виде.
    * Влияние: В случае компрометации базы данных злоумышленник получит неограниченный доступ к конфиденциальным данным всех пользователей.

Сценарий атаки:
1. Злоумышленник обнаруживает уязвимость XSS на странице входа в приложение.
2. Он внедряет вредоносный JavaScript-код в поле ввода имени пользователя.
3. Пользователь, просматривая страницу входа, активирует вредоносный код.
4. Вредоносный код крадет сессионные куки пользователя и отправляет их злоумышленнику.
5. Злоумышленник использует украденные куки для доступа к учетной записи пользователя.
6. Злоумышленник получает доступ к данным хранилища пользователя, включая пароли и номера кредитных карт.

Исправление уязвимостей:
1. SQL-инъекция:
    * Использовать параметризованные запросы или ORM для формирования SQL-запросов.
    * Валидировать и экранировать пользовательский ввод на стороне сервера.
2. XSS:
    * Экранировать пользовательский ввод на стороне сервера перед его отображением в веб-интерфейсе.
    * Использовать Content Security Policy (CSP) для ограничения выполнения JavaScript-кода.
3. Отсутствие шифрования данных:
    * Шифровать конфиденциальные данные пользователей перед их сохранением в базе данных.
    * Использовать надежные алгоритмы шифрования и безопасные методы управления ключами.

Новая архитектура:

[SecureVault Architecture]

Изменения:

* Добавлен модуль шифрования данных: Шифрует конфиденциальные данные пользователей перед их сохранением в базе данных и расшифровывает их при запросе.
* Добавлен модуль валидации и экранирования ввода: Валидирует и экранирует пользовательский ввод на стороне сервера для предотвращения SQL-инъекций и XSS-атак.
* Добавлен Content Security Policy (CSP): Ограничивает выполнение JavaScript-кода в веб-интерфейсе.
