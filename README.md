# nodejs-hw-mongodb

[text](<../Application file structure/src>)

Задание

Вам нужно создать приложение для работы с коллекцией контактов, в котором можно будет посредством HTTP запросов получать данные всех контактов или одного по id.

Файл с данными для загрузки:

contacts.json//https://drive.google.com/file/d/13baA584rRyDY8L_Axqi2Ii_qD9lUG-Hw/view

Критерии приема
Создано репозиторийnodejs-hw-mongodb
Задание выполнено на ветке hw2-mongodb
При сдаче домашней работы есть ссылки на исходные файлы Githubи ссылки на задеплоенный проект этой ДЗ (ветка hw2-mongodb) на render.com // https://render.com
При выполнении кода задачи нет ошибок

Пошаговое выполнение задания

Шаг 1
Инициализируйте проект командой npm init -y

Добавьте в зависимости от проекта eslint и подкорректируйте его конфигурационный файл согласно приведенному примеру в материалах 1го модуля в блоке “Файлы настроек”.

Добавьте в корень проекта файлы .gitignore и .prettierrc соответствующее содержимое.

Установите nodemon в качестве зависимости для разработки. Добавьте скрипт "dev" в файл package.json для запуска сервера с помощью nodemon. Для этого измените раздел scripts.
"lint": "eslint src//*.js"**:

ESLint — это инструмент для анализа кода, который помогает находить и исправлять проблемы в коде JavaScript.
src//*.js** указывает, что нужно проверить все файлы с расширением .js в папке src и во всех её подпапках.
Команда npm run lint выполнит ESLint для всех указанных файлов, что поможет обеспечить качество и консистентность кода, а также избежать ошибок.
"start": "node ./src/index.js":

node ./src/index.js запускает основной файл вашего приложения с помощью Node.js.
Команда npm start используется для запуска вашего приложения в продакшен-режиме.
"dev": "nodemon src/index.js":

Nodemon — это инструмент, который автоматически перезапускает приложение Node.js при изменении файлов в проекте.
src/index.js — это файл, который будет запущен и за которым будет следить nodemon.
Команда npm run dev запустит ваш проект в режиме разработки и будет перезапускать его каждый раз, когда вы вносите изменения в файлы, что значительно ускоряет процесс разработки.

Шаг 2

Создайте в корне проекта папку src.

В папке src создайте файл с именем server.js. В нем будет находиться логика работы вашего express-сервера.

В файле src/server.js создайте функцию setupServer, в которой будет создаваться express сервер. Эта функция должна включать в себя:

1.Создание сервера с помощью вызова express()
2.Настройка cors//https://www.npmjs.com/package/cors:
Пример настройки CORS для разрешения запросов только с определенного домена:
const corsOptions = {
  origin: 'http://example.com', // Разрешает запросы только с указанного домена
  methods: ['GET', 'POST'], // Разрешает только указанные методы
  allowedHeaders: ['Content-Type', 'Authorization'], // Разрешает только указанные заголовки
};

app.use(cors(corsOptions));
 и логгера pino//https://github.com/pinojs/pino-http .
 Использовать pino-pretty
Команда разработки Pino создала аналогичный jq инструмент — pino-pretty — для преобразования строк JSON в удобочитаемый человеком обычный текст.//https://habr.com/ru/articles/737982/ or //https://github.com/pinojs/pino-pretty
npm install express cors pino-http pino-pretty dotenv
3.Обработку несуществующих роутов (возвращает статус 404 и ответное сообщение)
{
   message: 'Not found', ,
}

Запуск сервера на порте, указанном через переменную окружения PORT или 3000, если такая переменная не указана
При удачном запуске сервера выводить в консоль строку “Server is running on port {PORT}”, где {PORT}это номер вашего порта
Не забудьте указать переменную окружения в файле.env.example

Создайте файл src/index.js. Импортируйте и вызовите в нем функцию setupServer.

Шаг 3

Создайте свой кластер в mongodb и функцию initMongoConnection для установления соединения с ней в отдельном файле src/db/initMongoConnection.js.

При создании кластера в MongoDB Atlas, не забудьте настроить доступ к сети, чтобы разрешить подключение с любого IP-адреса. Для этого выполните следующие шаги:

Войдите в свой аккаунт MongoDB Atlas и перейдите к своему проекту.
Выберите кластер или создайте новый.
Выделите раздел "Network Access".
Добавьте новую запись в список IP-адресов, нажав кнопку "+ ADD IP ADDRESS".
В появившемся окне выберите "ALLOW ACCESS FROM ANYWHERE" или введите 0.0.0.0/0в поле "Access List Entry". Это позволит подключение к вашему кластеру с любого IP-адреса.
При необходимости добавьте комментарий, затем нажмите "Confirm".

При удачном подключении к вашей базе mongodbвыводите в консоль строку "Mongo connection successfully established!".

Данные для подключения к базе должны быть вынесены в следующие переменные оцепления:
MONGODB_USER
MONGODB_PASSWORD
MONGODB_URL
MONGODB_DB
Укажите эти переменные окружения в файле.env.example
Для работы с mongodbиспользуйте пакет mongoose .

В файле src/index.js вызовите функции initMongoConnection. Убедитесь, что соединение с базой устанавливается до запуска сервера.

Mongoose — это библиотека для работы сMongoDB в среде Node.js. Она предоставляет простой способ моделирования и валидации данных MongoDB, а также удобный интерфейс выполнения запросов в базу данных. Установим ее в наше приложение соответствующей командой:npm install mongoose


Шаг 4

имя - строка, обязательная
phoneNumber — строка, обязательная
электронная почта - электронная почта, необязательно
isFavourite — логическое значение, по умолчанию false
contactType — строка, перечисление («работа», «дом», «личный»), обязательно, по умолчанию «личный»


Для автоматического создания полей createdAtи updatedAtможно использовать параметр timestamps: trueпри создании модели. Это добавляет к объекту два поля: createdAt(дата создания) и updatedAt(дата обновления), и их не нужно добавлять вручную.



контакты.json



Импортируйте базовый набор контактов из файла contacts.json в вашу базу, используя любой UI интерфейс (в браузере, Mongo Compass и т.п.). Убедитесь, что название коллекции в коде модели и визуальном интерфейсе совпадает.



Шаг 5



Создайте роут GET /contacts, который будет возвращать массив всех контактов. Обработка этого роута должна включать:

Регистрация роута в файлеsrc/server.js
Описание контроллера для этого роута
Создание сервиса в папке src/services в файле с соответствующим существом именем (в данном случае contacts.js)
Ответ сервера должен содержать объект со следующими свойствами:
status- статус ответа
message— уведомление о результате выполнения операции"Successfully found contacts!"
data- данные, полученные в результате обработки запроса


Шаг 6



Создайте роут GET /contacts/:contactId, который будет возвращать данные контакта по переданному ID или возвращать ошибку 404, если контакт не найден. Обработка этого роута должна включать:

Регистрация роута в файлеsrc/server.js
Описание контроллера для этого роута
Создание сервиса в папке src/servicesв файле с соответствующим именем сущности (в данном случае contacts.js)
Ответ сервера, если контакт найден, должен быть со статусом 200 и содержать объект со следующими свойствами:


{
	status:  200 ,
 	сообщение:  «Успешно найден контакт с идентификатором {**contactId**}!» ,
 	данные:
		// об'єкт контакта
}



Добавьте проверку или контакт по переданному идентификатору был найден. Если контакт не найден, верните ответ с сатусом 404 и следующим объектом:



{
	сообщение: «Контакт не найден» ,
}



На данном этапе не нужно проверять невалидный MongoDB ID в этом модуле. Предполагаем, что ID всегда валидный




Шаг 7



Задеплойте ваше приложение из ветки hw2-mongodbна render.com . Пошаговая инструкция как это сделать есть в этом видео:





Очень важно перед сдачей дзв на проверку ментору проверять работу вашего задеплоенного приложения на render.com !

Если, например, при деплое вы забыли добавить переменные оцепления (env), то задеплоенный бэкенд не будет работать. Также проверьте, что все созданные вами маршруты бэкенда работают, как ожидается в соответствии с заданием.