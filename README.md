# Генератор заключений в экспертизе

## Подготовка перед установкой:

- Открыть файл expertiza-1.0-SNAPSHOT.jar архиватором, далее в BOOT-INF/classes/ открыть файл application-production.properties в блокноте и отредакатировать сткроку подключения к БД (spring.datasource.url=jdbc:mariadb://127.0.0.1:3306/expertiza?createDatabaseIfNotExist=true&allowPublicKeyRetrieval=true)
  Вместо адреса 127.0.0.1 указать IP сервера, на котором установлена MySql. Если MySql установлена на том же сервере, указать текущий IP адрес. Здесь же, при необходимости, можно указать логин\пароль для доступа к бд.
- Запаковать отредактированный файл обратно.

## Варианты установки

### Docker:

Системные требования:

- ОС: Windows 10+, Linux
- Docker
- MySql 8+

Перед созданием контейнера положить в папку docker последнюю версию expertiza-1.0-SNAPSHOT.jar и license.xml

Создание контейнера (выполнять в папке docker):

`docker build -t expertiza .`

Запуск контейнера:

`docker run -d -p 80:80 expertiza`

Если по адресу http://127.0.0.1/ ничего не открылось, посмотреть логи контейнера expertiza на наличие ошибок.

### Windows сервис:

Системные требования:

- ОС: Windows 7+
- MySql 8+

Установка (предполагается, что все файлы лежат в одной папке):

- Скачать последнюю стабильную версию winsw на https://github.com/winsw/winsw/releases
- Переименовать winsw в conclusionService.exe

- Создать файл conclusionService.xml с содержимым:
  ```xml
  <service>
  <id>conclusionService</id>
  <name>Conclusion Service</name>
  <description>Conclusion Service</description>
  <env name="MYAPP_HOME" value="%BASE%"/>
  <executable>java</executable>
  <arguments>-Xmx512m -jar -Dspring.profiles.active=production "%BASE%\expertiza-1.0-SNAPSHOT.jar"</arguments>
  <logmode>rotate</logmode>
  </service>
  ```
- Поместить в эту же папку файлы expertiza-1.0-SNAPSHOT.jar, license.xml
- Для установки сервиса выполнить: 

`conclusionService.exe install`

- Зайти в список сервисов Windows, найти службу conclusionService, в свойствах указать тип запуска "автоматический(отложенный)". Запустить службу.

- Если по адресу http://127.0.0.1/ ничего не открылось, посмотреть логи в той же папке.
