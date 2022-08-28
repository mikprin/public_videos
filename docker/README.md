# Ультимативный гайд по Docker



# Установка

## Linux

1. Установить ядро докер для вашей платформы. Подробно тут в [официальной документации на английском](https://docs.docker.com/engine/install/ubuntu/). И вот тут [на русском](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-ru)

### Ububtu как пример:

Установить пакеты

* `sudo apt-get update`
* `sudo apt-get install ca-certificates curl gnupg lsb-release`

Добавить репозиторий

* `sudo mkdir -p /etc/apt/keyrings`
* `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`

Установить движок

* `sudo apt-get update` - важный шаг
* `sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin`


Еще один важный шаг, добавить сервисы docker (если установщик не сделал это сам):

* `sudo systemctl enable --now docker.service`
* `sudo systemctl enable --now containerd.service`

Проверяем установку

* `sudo docker run hello-world`

Важный пост установочный шаг для удобства. Добавить себя в группу докер чтобы не использовать sudo:

* `sudo groupadd docker`
* `sudo usermod -aG docker $USER`

Теперь можно запустить контейнер без sudo:

* `docker run hello-world`

### Manjaro/Arch

* `sudo pacman -Syu`
* `sudo pacman -S docker`

Systemd сервис:

`sudo systemctl enable --now docker.service`

Важный пост установочный шаг для удобства. Добавить себя в группу докер чтобы не использовать sudo:

* `sudo groupadd docker`
* `sudo usermod -aG docker $USER`

## Windows

Скачать [установочный файл](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe).

Запустить в CMD:

`start /w "" "Docker Desktop Installer.exe" install --accept-license`

Добавить пользователя в группу

`net localgroup docker-users <user> /add`

## Используем утилиту `docker`

Одной из самых простых и удачных утилит для нативного управления контейнерами является встроенная утилита `docker`.
Чтобы вызвать ее, нужно установить движок`docker-engine` и получаете ее в подарок. 

Добавляя к слову `docker` другие команды можно управлять контейнерами.

Чтобы перечислить используемые и установленные контейнеры используйте `docker ps`. Эта утилита, как и многие последующие вдохновлена UNIX утилитой `ps` которая перечисляет запущенные процессы.

Для обращения к контейнеру можно использовать его имя, которое дано было ему при создании, либо уникальный ID. Тут важно не путать имя контейнера и его образа. Ибо у вас может быть несколько контейнеров с одинаковым образом. Например два контейнера с базами данных Postgres

* `docker ps` - перечислить работающие контейнеры
* `docker ps -a` - перечислить все установленные контейнеры (в том числе остановленные)
* `docker stop container_name/id` - остановить запущенный контейнер
* `docker start container_name/id` - запустить остановленный контейнер
* `docker rm container_name/id` - удалить остановленный контейнер. (Используя флаг `--force` можно удалить и работающий, но лучше все же остановить его)


## Работа с образами

* `docker images` - перечислить загруженные образы
* `docker rmi hello_world` - удалить образ с именем `hello_world`
* `docker pull hello-world` - скачать образ с именем `hello world` из docker hub
* `docker search hello_world` - найти образ hello_world в docker hub

## Запуск контейнеров

* `docker run hello-world` - скачать и (или) запустить образ контейнера `hello-world`. Делает все нужные шаги
* `docker run hello-world -d` - добавьте `-d` чтобы не прикреплять терминал к контейнеру (контейнер будет существовать на фоне)
* `docker run hello-world --name my_hello_world` - скачать и (или) запустить образ контейнера `hello-world`. Дать контейнеру имя `my_hello_world`, иначе у контейнера будет случайное имя. Имя используется для обращения к контейнеру.
* `docker-compose up -d` - использовать файл `docker-compose.yaml` в текущей папке и запустить (аналогично `docker run`) все контейнеры (сервисы) оттуда.
* `docker-compose stop` - использовать файл `docker-compose.yaml` в текущей папке и остановить все контейнеры (сервисы) оттуда.

## Работа с портами

* `docker run --port 8888:80 hello_world` - скачать и (или) запустить контейнер `hello-world`, но соединить порт 8888 системы с портом 80 контейнера.

## Работа с общими папками

* `docker run --volume /docker/:/os_folder hello_world` - скачать и (или) запустить контейнер `hello-world`, но соединить `/docker` системы с директорией `/os_folder` контейнера.

## Docker compose

Конманды `rm`,`run`,`stop` работают в связке аналогичной `docker run` но для контейнеров перечсисленных в `docker-compose.yaml`.

Пример `docker-compose.yaml` с комментариями:

```
  hello_world:
    container_name: postgresql_database # Имя контейнера (для обращения к нему)
    image: hello_world # Название образа
    restart: always #
    ports:
      - 1111:1111" # Прокидывание порта 1111 контейнера в систему
    environment:
      MY_VAR=1 # Передача в окружение контейнера переменной MY_VAR равной 1

```