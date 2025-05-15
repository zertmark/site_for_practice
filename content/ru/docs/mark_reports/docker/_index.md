---
title: Знакомство с Docker Compose
description:  Знакомимся с технологией для одновременного управления нескольких контейнеров 
linkedTitle: Отчёты Марка
author: Пашковский Марк Анатольевич 
categories: [Сайт]
resources:
  - src: "**.{png,jpg}"
    title: "Image #:counter"
    params: 
      byline: ""
---

###### **Знакомство с Docker Compose**

### **1. Что такое Docker Compose ?**

Docker Compose --- это мощный инструмент, позволяющий очень быстро
развёртывать приложения, отличающиеся сложной архитектурой. Если Docker
оперирует отдельными контейнерами, то Docker Compose оперирует уже
набором контейнеров, т.е. есть сервисов.

### **2. Создание проекта для Docker Compose**

##### **2.1 Создадим базовый проект, для демонстрации функций docker compose**

{{< imgproc image1 Crop "600x300" >}}
Рисунок 1 - строение базового проекта
{{< /imgproc >}}

Содержимое файла Dockerfile в папках client и server будут отличаться
только названием python файлов, которые мы добавляем и, в случае с
Dockerfile'а в папке server -- мы также добавляем файл `index.html`:
> DockerFile client

```docker
FROM python:3

WORKDIR /client/

ADD client.py .
```
> *DockerFile server*
```docker

FROM python:3

WORKDIR /server/

ADD server.py .

ADD index.html .
```
Создаём файл `docker-compose.yaml` и указываем в качестве сервисов наши
`server` и `client` и соответствующие им папки:

```docker
version: "3"

services:

  server:

    build: server/

    command: python ./server.py

    ports:

      - 4444:4444

  client:

    build: client/

    command: python ./client.py

    network_mode: host

    depends_on:

      - server
```
##### **2.2 Соберём наш проект**
Вводим команду: `docker-compose build`

{{< imgproc image2 Fit "1920x1080" >}}
Рисунок 2 - Сборка всего проекта одной командой docker-compose build
{{< /imgproc >}}

Проверим наши собранные образы командой `docker images`:

{{< imgproc image3 Fit "600x300" >}}
Рисунок 3 - проверка собранных образов Docker
{{< /imgproc >}}


Как видно на рисунке 3 - оба наших образа успешно собрались. Теперь
давайте их запустим, проверим их работоспособность.

Сделать это можно также при помощи Docker Compose, а именно при помощи
команды `docker-compose up`:

{{< imgproc image4 Fit "1920x1080" >}}
Рисунок 4 - запуск всех контейнеров
{{< /imgproc >}}

Результат запуска и работы контейнеров успешен (клиент успешно выводит
содержимое index.html, полученный с сервера):

{{< imgproc image5 Fit "1920x800" >}}
Рисунок 5 - вывод «клиента» полученных данных от «сервера»
{{< /imgproc >}}


Можно также заметить, что «клиент» отключается (возвращает ноль) в
выводе команды docker-compose, что означает, что контейнер завершил свою
работу. Проверим это:

{{< imgproc image6 Fit "1920x300" >}}
Рисунок 6 - работа только одного контейнера «сервер»
{{< /imgproc >}}


Также сверим результат вывода «клиента» контейнера с оригинальным
файлом:

{{< imgproc image7 Fit "1920x1080" >}}
Рисунок 7 - проверка правильности чтения файла index.html
{{< /imgproc >}}

### **3. Применим наши новые навыки для сборки новой версий Cyberpolygon**
##### **3.1 В новой версии Cyberpolygon был добавлен Docker Compose (рисунки 8-10):**

{{< imgproc image8 Fill "1920x1080" >}}
Рисунок 8 - начало сборки
{{< /imgproc >}}


{{< imgproc image9 Fill "1920x1080" >}}
Рисунок 9 - сборка успешно завершена
{{< /imgproc >}}


Запустим контейнеры командой docker-compose up и зайдём на
localhost:8000

{{< imgproc image10 Fit "1920x1080" >}}
Рисунок 10 - успешный запуск контейнеров для сайта Cyberpolygon'а
{{< /imgproc >}}


### **Вывод**

Мы узнали: что такое docker-composer, его базовые настройки и способы
применения и успешно запустили тестовый проект и проект Cyberpolygon
