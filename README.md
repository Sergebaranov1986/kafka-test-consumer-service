1) Создаем проект в initializir, в dependecy добавляем kafka
   
2) Создаем 2 отдельных сервиса producer и consumer
   
3) Поднимаем kafka через Docker, в файле docker-compose указываем image: 'bitnamilegacy/kafka:latest' , поскольку все имейджи bitnami перенесены в легаси репозиторий и больше не поддерживаются
   
4) Создаем конфигурационный класс, для того, чтобы consumer мог понимать, как ему работать с Kafka(аннотируем @EnableKafka, так как используем KafkaListener):

-  в первый бин в properties снова кладем где находится Kafka(localhost) и дополнительно определяем consumer group(но наш сервис запущен в единственном экземпляре). Десериализация через JsonDeserializer(depricated!)
-  второй бин ConcurrentKafkaListenerContainerFactory настройка в интернете
5) Пишем сам consumer service. в нем аннотируем метод @Kafkalistener(указываем топик, который этот метод слушает)
   
6) Проводим тесты через единичные post запросы, смотрим логи. Добавлены ключи для разбития по партициям.
   
![6](https://github.com/user-attachments/assets/bf361467-2db4-4e81-b2a0-c75a52abeea4)

7) Используем Apache Bench для нагрузочного тестирования HTTP-эндпоинта(1000 запросов, параллельно по 10 запросов, тело запроса в producer'е)
![7](https://github.com/user-attachments/assets/65c44ae1-053e-4f2b-89e7-a395bd814497)

   


   
