# Adding-a-book

import tkinter as tk
from tkinter import ttk
from tkinter import messagebox
import psycopg2

def connect_db():
    try:
        connection = psycopg2.connect(user="postgres",
                                      # пароль, который указали при установке PostgreSQL
                                      password="22222",
                                      host="localhost",
                                      port="5432")
        connection.set_isolation_level(ISOLATION_LEVEL_AUTOCOMMIT)
        # Курсор для выполнения операций с базой данных
        cursor = connection.cursor()
        sql_create_database = 'create database postgres_db'
        cursor.execute(sql_create_database)
    except (Exception, Error) as error:
        print("Ошибка при работе с PostgreSQL", error)
    finally:
        if connection:
            cursor.close()
            connection.close()
            print("Соединение с PostgreSQL закрыто")

def create_table(connection):
    with connection.cursor() as cursor:
        cursor.execute(
            """CREATE TABLE IF NOT EXISTS user (
            id SERIAL PRIMARY KEY,
            username VARCHAR(50) UNIQUE NOT NULL
            password VARCHAR(255) UNIQUE NOT NULL);
            """)
        connection.commit()

class BookAdderApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Добавление Книги")
        self.root.geometry("400x250")

        # Название книги
        ttk.Label(root, text="Название:").grid(row=0, column=0, padx=5, pady=5, sticky="e")
        self.title_entry = ttk.Entry(root, width=30)
        self.title_entry.grid(row=0, column=1, padx=5, pady=5)

        # Автор книги
        ttk.Label(root, text="Автор:").grid(row=1, column=0, padx=5, pady=5, sticky="e")
        self.author_entry = ttk.Entry(root, width=30)
        self.author_entry.grid(row=1, column=1, padx=5, pady=5)

        # Жанр
        ttk.Label(root, text="Жанр:").grid(row=2, column=0, padx=5, pady=5, sticky="e")
        self.year_entry = ttk.Entry(root, width=10)
        self.year_entry.grid(row=2, column=1, padx=5, pady=5, sticky="w")

        # Кнопка добавления
        add_button = ttk.Button(root, text="Добавить Книгу", command=self.add_book)
        add_button.grid(row=3, column=0, columnspan=2, pady=10)

    def add_book(self):
        title = self.title_entry.get().strip()
        author = self.author_entry.get().strip()
        genre = self.year_entry.get().strip()

        if not title or not author or not  genre:
             messagebox.showerror("Ошибка", "Пожалуйста, заполните все поля.")
             return


        print(f"Добавлена книга: {title}, {author}, {genre}")
        messagebox.showinfo("Успех", "Книга успешно добавлена!")
        self.clear_entries()

    def clear_entries(self):
        self.title_entry.delete(0, tk.END)
        self.author_entry.delete(0, tk.END)
        self.year_entry.delete(0, tk.END)

if __name__ == "__main__":
    root = tk.Tk()
    app = BookAdderApp(root)
    root.mainloop()

    cursor.close()  # закрываем курсор
    conn.close()  # закрываем подключение
