# Decorators Logger
## Описание проекта

Этот проект демонстрирует использование декораторов в Python для логирования вызовов функций. Декораторы logger и logger(path) записывают информацию о вызове функций, включая дату и время, имя функции, переданные аргументы и возвращаемое значение, в файл журнала.
## Содержание

    Описание проекта
    Структура проекта
    Как использовать
        Непараметризованный декоратор logger
        Параметризованный декоратор logger(path)
    Тестирование
    Пример работы

## Структура проекта


``` yaml
decorators_logger/
│
├── deco_1.py        # Реализация непараметризованного декоратора logger
├── deco_2.py        # Реализация параметризованного декоратора logger(path)
├── main.log         # Файл журнала для непараметризованного декоратора
└── README.md        # Этот файл
```

## Как использовать

## Непараметризованный декоратор logger

Этот декоратор записывает информацию о вызове функции в файл main.log.

Пример использования:

``` python

import os
from datetime import datetime

def logger(old_function):
    def new_function(*args, **kwargs):
        now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        result = old_function(*args, **kwargs)
        log_entry = (f"{now} - Функция '{old_function.__name__}' вызвана с "
                     f"args: {args}, kwargs: {kwargs}. Возвращено: {result}\n")
        with open('main.log', 'a', encoding='utf-8') as f:
            f.write(log_entry)
        return result
    return new_function

@logger
def hello_world():
    return 'Hello World'

@logger
def summator(a, b=0):
    return a + b

@logger
def div(a, b):
    return a / b
```

## Параметризованный декоратор logger(path)

Этот декоратор принимает путь к файлу и записывает информацию о вызове функции в указанный файл.

Пример использования:

``` python

import os
from datetime import datetime

def logger(path):
    def __logger(old_function):
        def new_function(*args, **kwargs):
            now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
            result = old_function(*args, **kwargs)
            log_entry = (f"{now} - Функция '{old_function.__name__}' вызвана с "
                         f"args: {args}, kwargs: {kwargs}. Возвращено: {result}\n")
            with open(path, 'a', encoding='utf-8') as f:
                f.write(log_entry)
            return result
        return new_function
    return __logger

@logger('log_1.log')
def hello_world():
    return 'Hello World'

@logger('log_1.log')
def summator(a, b=0):
    return a + b

@logger('log_1.log')
def div(a, b):
    return a / b
```

## Описание:

    Удаляет файл main.log, если он существует.
    Декорирует три функции: hello_world, summator и div.
    Вызывает эти функции и проверяет их возвращаемые значения.
    Проверяет наличие файла main.log и содержание записей.


``` bash

python deco_1.py
```
Вывод:
Содержимое main.log:

``` yaml

2024-04-27 10:15:30 - Функция 'hello_world' вызвана с args: (), kwargs: {}. Возвращено: Hello World
2024-04-27 10:15:31 - Функция 'summator' вызвана с args: (2, 2), kwargs: {}. Возвращено: 4
2024-04-27 10:15:32 - Функция 'div' вызвана с args: (6, 2), kwargs: {}. Возвращено: 3.0
2024-04-27 10:15:33 - Функция 'summator' вызвана с args: (4.3, 2.2), kwargs: {}. Возвращено: 6.5
2024-04-27 10:15:34 - Функция 'summator' вызвана с args: (), kwargs: {'a': 0, 'b': 0}. Возвращено: 0
```
