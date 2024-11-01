
1. Введение в базы данных и SQL

Теория:

	•	Что такое база данных: Это организованная структура, предназначенная для хранения и управления данными. В большинстве случаев данные хранятся в виде таблиц, напоминающих электронные таблицы.
	•	СУБД (система управления базами данных): СУБД помогает работать с базой данных, обеспечивая хранение, манипуляции, защиту данных. Примеры СУБД: SQLite, MySQL, PostgreSQL.
	•	SQL (Structured Query Language): Это язык запросов, позволяющий работать с базами данных, включая операции создания таблиц, добавления, удаления и изменения данных, а также выполнения запросов для выборки информации.

Пример кода:

Создадим первую таблицу students с полями id, name, age, grade:

import sqlite3

# Создаем подключение и курсор
connection = sqlite3.connect('example.db')
cursor = connection.cursor()

# Создаем таблицу
cursor.execute('''CREATE TABLE IF NOT EXISTS students (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    name TEXT NOT NULL,
                    age INTEGER,
                    grade TEXT
                )''')
connection.commit()
connection.close()

Задания:

	1.	Создать таблицу employees с полями id, name, position, salary.
	2.	Создать таблицу products для хранения данных о товарах с полями: product_id, product_name, price, quantity.

2. Подключение к базе данных и создание таблиц

Теория:

	•	Подключение к базе данных: В Python для работы с SQLite можно использовать встроенную библиотеку sqlite3. Метод connect() создает подключение к базе данных.
	•	Курсор: cursor() — объект, с помощью которого выполняются SQL-запросы.
	•	Типы данных SQL:
	•	INTEGER: целое число.
	•	TEXT: строка текста.
	•	REAL: число с плавающей точкой.
	•	BLOB: для хранения бинарных данных, таких как изображения или файлы.

Пример кода для добавления записей:

Добавим записи о студентах:

connection = sqlite3.connect('example.db')
cursor = connection.cursor()

# Добавление данных
cursor.execute("INSERT INTO students (name, age, grade) VALUES ('Alice', 20, 'A')")
cursor.execute("INSERT INTO students (name, age, grade) VALUES ('Bob', 19, 'B')")

connection.commit()
connection.close()

Задания:

	1.	Добавить три записи о сотрудниках в таблицу employees.
	2.	Создать таблицу courses с полями course_id, course_name, credits. Вставить данные о трёх курсах.

3. CRUD-операции (Create, Read, Update, Delete)

Теория:

	•	Создание (Create): добавление новых записей в таблицу с помощью команды INSERT.
	•	Чтение (Read): получение данных из таблицы, часто с фильтрацией (WHERE) и сортировкой (ORDER BY).
	•	Обновление (Update): изменение данных в существующих записях с помощью UPDATE.
	•	Удаление (Delete): удаление записей, соответствующих условию, из таблицы.

Примеры кода:

	•	Добавление данных:

cursor.execute("INSERT INTO students (name, age, grade) VALUES ('Charlie', 21, 'C')")


	•	Чтение данных:

cursor.execute("SELECT * FROM students WHERE grade = 'A'")
rows = cursor.fetchall()
for row in rows:
    print(row)


	•	Обновление данных:

cursor.execute("UPDATE students SET grade = 'A+' WHERE name = 'Alice'")


	•	Удаление данных:

cursor.execute("DELETE FROM students WHERE name = 'Bob'")



Задания:

	1.	Прочитать все записи из таблицы employees, отобрать сотрудников с зарплатой выше 50000.
	2.	Обновить данные в таблице products, увеличив цену товара с product_id = 1 на 10%.
	3.	Удалить запись в таблице students для студента с именем Charlie.

4. Запросы и связи между таблицами

Теория:

	•	Связи между таблицами:
	•	Один ко многим: Например, один отдел может включать множество сотрудников.
	•	Многие ко многим: Например, студенты могут посещать несколько курсов, и курсы могут включать несколько студентов.
	•	JOIN: Оператор для объединения данных из нескольких таблиц.
	•	INNER JOIN: Выбирает только совпадающие записи из обеих таблиц.
	•	LEFT JOIN: Выбирает все записи из левой таблицы и совпадающие из правой.

Пример кода с JOIN:

Создадим таблицу courses и enrollment, чтобы показать курсы, на которые записаны студенты:

# Создаем таблицы
cursor.execute('''CREATE TABLE IF NOT EXISTS courses (
                    course_id INTEGER PRIMARY KEY,
                    course_name TEXT
                )''')
cursor.execute('''CREATE TABLE IF NOT EXISTS enrollment (
                    student_id INTEGER,
                    course_id INTEGER,
                    FOREIGN KEY(student_id) REFERENCES students(id),
                    FOREIGN KEY(course_id) REFERENCES courses(course_id)
                )''')
connection.commit()

# Объединение данных
cursor.execute('''SELECT students.name, courses.course_name 
                  FROM students
                  JOIN enrollment ON students.id = enrollment.student_id
                  JOIN courses ON courses.course_id = enrollment.course_id''')
rows = cursor.fetchall()
for row in rows:
    print(row)

Задания:

	1.	Создать таблицы departments и employees, где один отдел включает несколько сотрудников.
	2.	Связать таблицы books и authors так, чтобы можно было выводить названия книг и их авторов.
	3.	Выполнить запрос для отображения всех сотрудников с названием отдела, в котором они работают.

5. Проект: Учёт книг

Описание задания:

Создать базу данных для учёта книг с таблицами books и authors. Каждая книга должна иметь название, год выпуска, жанр и автора. Связать таблицы так, чтобы для каждой книги можно было получить её автора.

Шаги для выполнения проекта:

	1.	Создание базы данных и таблиц:

connection = sqlite3.connect('library.db')
cursor = connection.cursor()

cursor.execute('''CREATE TABLE IF NOT EXISTS authors (
                    author_id INTEGER PRIMARY KEY,
                    name TEXT NOT NULL
                )''')

cursor.execute('''CREATE TABLE IF NOT EXISTS books (
                    book_id INTEGER PRIMARY KEY,
                    title TEXT,
                    year INTEGER,
                    genre TEXT,
                    author_id INTEGER,
                    FOREIGN KEY(author_id) REFERENCES authors(author_id)
                )''')
connection.commit()


	2.	Добавление данных:

cursor.execute("INSERT INTO authors (name) VALUES ('Leo Tolstoy')")
cursor.execute("INSERT INTO books (title, year, genre, author_id) VALUES ('War and Peace', 1869, 'Novel', 1)")
connection.commit()


	3.	Запросы для выборки данных:

# Получение всех книг с именами их авторов
cursor.execute('''SELECT books.title, authors.name 
                  FROM books
                  JOIN authors ON books.author_id = authors.author_id''')
rows = cursor.fetchall()
for row in rows:
    print(row)



Задания:

	•	Добавить ещё несколько авторов и книг.
	•	Написать функции для добавления, обновления, удаления и отображения книг и авторов.
	•	Написать запрос, который покажет все книги, опубликованные после 1900 года, отсортированные по году выпуска.

Этот материал обеспечит полное погружение в основы работы с базами данных на Python и позволит ученику приобрести навыки, необходимые для создания и управления базами данных, написания запросов и построения структурированных проектов.
