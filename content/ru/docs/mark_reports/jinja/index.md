---
title: Учимся Jinja
description:  Использование библиотеки Jinja2
linkedTitle: Отчёты Марка
author: Пашковский Марк Анатольевич
categories: [Сайт]
resources:
  - src: "**.{png,jpg}"
    title: "Image #:counter"
    params:
      byline: ""
---

###### **Использование библиотеки Jinja2**

### **План:**

- 1.  Цели и задачи
- 2.  Что такое Jinja ?
- 3.  Как установить ?
- 4.  Как пользоваться в Web-разработке ?

### **Цели и задачи**
Изучить базовый функционал Jinja и протестировать полученные знания
с фреймворком Flask


### **Что такое Jinja ?**

**Jinja --** это библиотека шаблонизатор, способная обрабатывать сложные
шаблоны с поддержкой разных конструкций. Используется для динамического
создания страниц на основе данных, полученных из баз данных (backend)

### **Установка:**

Установка данной библиотеки производиться командой:

pip3 install Jinja2

**Базовый синтаксис шаблонов:**

{{ переменная }}

```c
{% if %}

{% else %}

{% endif %}

{% block name_of_a_block %}

{% endblock name_of_a_block %}

{% for iter in array %}

{% endfor %}
```

Все значения переменных для парсинга передаются в виде словаря (или
наборов словарей):

**Пример с оценками:**

> **templates/template.txt**

```c
{# templates/message.txt #}
{% for student in students %}

{{ student.name }}

{% if student.score > 50 %}
    Very good. You passed the test !
{% else %}
    You didn't pass the test
{% endif %}
{% endfor %}
```

> **main.py**

```py
import jinja2

students = [

    {"name": "Alex",

     "score": 95},

    {"name": "John",

     "score": 60}

]

en =
jinja2.Environment(loader=jinja2.FileSystemLoader(searchpath="templates/"))

template = en.get_template("message.txt")

print(template.render(students=students))
```
**Вывод:**

```
Alex

Very good. You passed the test !

John

Very good. You passed the test !
```
### **Использование Jinja в Web-разработке**

Попробуем использовать Jinja2 совместно с Flask'ом:

Установка Flask:
```bash
python -m pip install flask
```

**Создание базового приложения на Flask:**
> **app.py**

```py

from flask import Flask

app = Flask(__name__)

@app.route("/")

def home():

    return "<h1>Basic app</h1>"

if __name__ == "__main__":

    app.run(debug=True)
```


{{< imgproc image1 Fill "600x300" >}}
Результат работы
{{< /imgproc >}}


**Теперь создадим несколько файлов:**

- base.html

- database.py (для эмуляции работы базы данных)

- results.txt (в папке data)

> **Значения results.txt:**
```
John: 84

Andrew: 20

Fin: 45

Stacy: 60

```
> **database.py:**
```py
_database_str = ""

def getDatabase() -> list[dict]:

    out = []

    if _database_str != "":

        return _database_str

   

    with open ("../data/results.txt") as file:

        for line in file.readlines():

            name, score = line.strip().split(":")

            out.append({

                "name": name.strip(),

                "score": int(score.strip())

            })

    return out
```

> **app.py**
```py
from flask import Flask

from jinja2 import Environment, FileSystemLoader

from database import getDatabase

app = Flask(__name__)

template_env = Environment(loader=FileSystemLoader("../templates/"))

results_template = template_env.get_template("base.html")

max_score = 100

title = "Results for grammar test"

@app.route("/students/")

def students():

    students_with_scores = getDatabase()

    if not students_with_scores:

        return "<h1>ERROR</h1>"

   

    return results_template.render({

    "max_score":max_score, "title":title,
"students":students_with_scores})

@app.route("/")

def home():

    return "<h1>Basic app</h1>"

if __name__ == "__main__":

    app.run(debug=True)
```

> **base.html:**
```html
{# templates/base.html #}

<!DOCTYPE html>

<html lang="en">

<head>

  <meta charset="utf-8">

  <title>{{ title }}</title>

</head>

<body>

  <h1>{{ title }}</h1>

  <ul>

  {% for student in students %}

    <li>

      {% if student.score > 30 %}✅{% else %}❌{% endif %}

      <em>{{ student.name }}:</em> {{ student.score }}/{{ max_score
}}

    </li>

  {% endfor %}

  </ul>



</body>

</html>
```
<!-- ![](./jinja/media/image2.png){width="6.496527777777778in"
height="3.170138888888889in"} -->

{{< imgproc image2 Fill "600x300" >}}
Результат работы нашего приложения Flask
{{< /imgproc >}}


### Вывод:

Jinja2 позволяет динамично изменять и генерировать статические страницы,
что мы и проделали в данной работе
