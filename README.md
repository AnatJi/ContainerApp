Practical work [https://github.com/AnatJi/ContainerApp.git/] Целью проекта является создание контейнера и запуск приложения в нем.

# Структура проекта:

## Dockerfile:

Докерфайл необходим для автоматизации запуска програмы, подключения библиотек, скачивания и установки необходимых элементов необходимых для функционирования запусксемого скрипта.

Докерфайл состоит из следующих элементов:

```
FROM python:3.10

# Задаём переменную окружения, чтобы пользователь не участвовал в установке (выбор клавиатуры и прочее)
ENV DEBIAN_FRONTEND=noninteractive

# Установите рабочую директорию в контейнере
WORKDIR /app

# Копируем файл скрипта в рабочую директорию контейнера
COPY . /app

# Укажите команду для запуска скрипта
CMD ["python", "/app/GreatingDateSript.py"]

```

#RRGGBB `Важно чтобы исполняемый скрипт находился в той же директории что и докерфайл`

##Запуск программы

Для сборки образа используем команду: 
`docker build -t [вводим имя для нового образа] .`

В нашем случае я назвал образ `container5_7`, он запустил пайтон скрипт который вывел дату и время:

![image 0f build dokerfile](https://github.com/AnatJi/ContainerApp/blob/docker-directory/ImagesForReport/image_for_doc)

## Docker-compose:

Докер композ позволяет запускать сразу несколько контейнеров и настраивать их параметры.

создадим две директории для bash-скрипта и python-скрипта:

```
mkdir bashapp
mkdir pythonapp
```

в эти директории помещаем наши скрипты через команду `cp`

После этого в основной директории создадим YAML-файл, который будет создавать контейнеры из `python:3.9-slim` и `bash:latest` образов.

```

    version: '3.8'   #указывает версию формата файла docker-compose.yml

    services: # services - основной раздел с описанием всех контейнеров
      pythonapp:
       image: python:3.9-slim  # image - указывает на Docker-образ для создания контейнера
       volumes:
         - ./pythonapp:/app  # volumes - монтирует директорию с хост-компьютера в директорию контейнера 
       working_dir: /app   # working_dir - устанавливает рабочую директорию
       command: python GreatingDateSript.py    # command - указывает какую команду нужно выполнить 

      bash_app:
        image: bash:latest
        volumes:
          - ./bashapp:/app
        working_dir: /app
        
        command: ["bash", "./DateGreetingScript.sh"]
```

После создания YAML-файла его останется только запустить.

Для запуска используем команду:
`docker-compose up`

Таким образом оба скрипта выполняются в отдельных контейнерах.

![image of start docker-compose file](https://github.com/AnatJi/ContainerApp/blob/docker-directory/ImagesForReport/image_for_compose)

Для завершения работы можно воспользоваться командой:

`docker-compose down`


