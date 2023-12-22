# Туториал по работе с Docker

Здесь можно подглядывать регулярные команды и паттерны сборки проектов

## Структура репозитория
```
├── DockerFiles
      ├── example1
            ├── poetry
                  ├── DockerFile
            ├── requirements
                  ├── DockerFile
            └── README.md
            
      ├── example2
            ├── poetry
                  ├── DockerFile
            ├── requirements
                  ├── DockerFile
            └── README.md
            
            
      ├── example3
            ├── poetry
                  ├── DockerFile
            ├── requirements
                  ├── DockerFile
            └── README.md
      
├── .dockerignore
├── docker-compose.yml
└── README.md
```

## Описание репозитория

- `/Dockerfiles` - пример реализации разных приложений в двух видах через `requiremets.txt` || `poetry`
- `docker-compose.yml` - пример реализации cборки компоуз файла с множественными компонентами
- `README.md` - документация по репозиторию

## Установка докер на linux
- Команды необходимо выполнять по очереди

1) `sudo apt update`
2) `sudo apt install apt-transport-https ca-certificates curl software-properties-common`
3) `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
4) `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"`
5) `sudo apt update`
6) `apt-cache policy docker-ce`
7) `sudo apt install docker-ce`
8) `sudo systemctl status docker` - проверка статуса

## Основная структура Docker сборки в проекте

1) `DockerFile`  - должен находится в родительской части проекта, необходим для сборки, конкретно текущего проекта


2) `docker-compose.yml`  - должен находится в родительской части проекта, необходим для многоступенчатной сборки
   проекта, если проект может включать в себя не только основные зависимости, а например базу данных, брокеры,
   планировщик и тд.


3) `.dockerignore` - файл игнорирования сборки проекта, необязательный в использовании, но может быть полезен, чтобы не
   тащить не нужные файлы

## Основные команды Docker
 - `docker images` - список образов
 - `docker ps` - список работающих контейнеров
 - `docker ps -a` - список всех контейнеров 
 - `docker logs name_conatainer\id_container` - логи контейнера
 - `docker network prune` - удаление неиспользуемых сетей
 - `docker system prune -a`  - удаление не используемых объектов (выполнять всегда перед тем как пересобираете образы)
 - `docker volume prune -a`  - удаление не используемых volume

#### [Подробная шпаргалка по командам](https://habr.com/ru/companies/flant/articles/336654/)

### Вот несколько вариаций команды `docker build` с пояснениями:

1. `docker build -t <имя_образа> <путь_к_Dockerfile>`
   - -t или --tag указывает имя и тег для создаваемого образа.
   - <имя_образа> - имя, которое вы хотите присвоить образу.
   - <путь_к_Dockerfile> - путь к Dockerfile. Это может быть относительный или абсолютный путь к файлу.

   Пример: `docker build -t myimage:latest .`
   В этом примере мы создаем образ с именем myimage и тегом latest из Dockerfile, находящегося в текущей директории (.).


2. `docker build -f <путь_к_Dockerfile> -t <имя_образа>`
   - -f или --file позволяет указать путь к Dockerfile.
   - <путь_к_Dockerfile> - путь к Dockerfile.

   Пример: `docker build -f /path/to/Dockerfile -t myimage`
   В этом примере мы создаем образ с именем myimage из Dockerfile, находящегося по указанному пути /path/to/Dockerfile.


3. `docker build --build-arg <переменная>=<значение> -t <имя_образа> <путь_к_Dockerfile>`
   - --build-arg позволяет передавать аргументы сборки в Dockerfile.
   - <переменная>=<значение> - имя и значение аргумента сборки.
   

   Пример: `docker build --build-arg ENVIRONMENT=production -t myimage .`
   В этом примере мы создаем образ с именем myimage и передаем аргумент сборки ENVIRONMENT со значением production в Dockerfile.


4. `docker build --no-cache -t <имя_образа> <путь_к_Dockerfile>`
   - --no-cache указывает Docker не использовать кэш при сборке образа.

   Пример: `docker build --no-cache -t myimage .`
   В этом примере мы создаем образ с именем myimage, принудительно пересобирая его без использования кэша.


### Вот несколько вариаций команды `docker run` с пояснениями:

1. `docker run <имя_образа>`
   - `<имя_образа>` - имя Docker-образа, на основе которого будет создан и запущен контейнер.

   Пример: `docker run myimage`
   В этом примере мы создаем и запускаем контейнер на основе образа с именем `myimage`.


2. `docker run -d <имя_образа>`
   - `-d` или `--detach` позволяет контейнеру работать в фоновом режиме (детач).

   Пример: `docker run -d myimage`
   В этом примере мы создаем и запускаем контейнер на основе образа `myimage` в фоновом режиме.


3. `docker run -p <хост_порт>:<контейнер_порт> <имя_образа>`
   - `-p` или `--publish` позволяет пробросить порт контейнера на хост-машину.
   - `<хост_порт>` - порт на хост-машине, который будет связан с портом контейнера.
   - `<контейнер_порт>` - порт в контейнере, который будет доступен через хост-порт.

   Пример: `docker run -p 8080:80 myimage`
   В этом примере мы создаем и запускаем контейнер на основе образа `myimage`, пробрасывая порт 80 (контейнера) на порт 8080 (хоста).


4. `docker run -e <переменная>=<значение> <имя_образа>`
   - `-e` или `--env` позволяет задавать переменные окружения в контейнере.
   - `<переменная>=<значение>` - имя и значение переменной окружения.

   Пример: `docker run -e ENVIRONMENT=production myimage`
   В этом примере мы создаем и запускаем контейнер на основе образа `myimage`, устанавливая значение переменной окружения `ENVIRONMENT` в `production`.


5. `docker run --name <имя_контейнера> <имя_образа>`
   - `--name` указывает имя для контейнера.
   - `<имя_контейнера>` - имя, которое вы хотите присвоить контейнеру.

   Пример: `docker run --name mycontainer myimage`
   В этом примере мы создаем и запускаем контейнер на основе образа `myimage` с именем `mycontainer`.