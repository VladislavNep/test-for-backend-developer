# Email Прослушиватель
Создать сервис на aiohttp для получения сообщений с yandex почты.


## Описание
У нас есть yandex почта куда поступают письмо от пользователя. Далее специалист обрабатывает это письмо 
и делает ответ, между пользователем и со специалистом создается "диалог".
Нужно создать сервис, который будет обрабатывать все поступающие email сообщения и записывать в базу данных mongoDB.

Логика действий:

1) Пользователь написал нам сообщение на почту, сервис **Email Listener** его получает и начинает обрабатывать.
2) Если письмо от пользователя новое и не является ответом на письмо от специалиста, то мы создаем новую запись(диалог) в 
mongodb(*смотри структуру в пункте MongoDB*).
3) Если письмо является ответом на письмо специалиста, то сервис должен это понять, найти соответствующий диалог в mongo
и обновить запись, добавив объект с соответствующими полями в поле **messages**
   
### Примечание
Один и тот же пользователь может вести несколько диалогов с нашим специалистами одновременно(например по разным темам),
сервис должен понимать какое письмо к какому диалогу относится, если ни к кому, то создать новый диалог. Каким образом
сервис будет это понимать, решать вам. 


## Задание
1) Создать сервис Email Listener на фреймворке aiohttp.
2) Завернуть в docker и описать в docker-compose.yml, environment для приложения и подключения описать там же.
3) Выложить на свой github и прислать ссылку на репозиторий.

## Схема
![Alt text](docs/scheme.png)


## MongoDB
При инсталляции подымется докер контейнер с mongodb. Так же создастся пустая база **dialog**.
Необходимо будет создать коллекцию с полями:

```
_id: primary_key,
email_from: email,
messages: [
    {
        subject: str,
        body: str,
        created: datetime
    },
    {
        subject: str,
        body: str,
        created: datetime
    },
]
```
**_id** будет являться уникальным идентификатором диалога 
### Инсталляция
    $ cd mongo
    $ docker-compose up --build

### Конфиги для подключения к базе mongodb
```dotenv
Host: 127.0.0.1
Port: 27017
Database: dialog
```
