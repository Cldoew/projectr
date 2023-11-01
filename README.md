> julianka ♡︎:
import sqlite3
import tkinter as tk


# создание БД
class DataBase():
    def init(self):
        self.connection = sqlite3.connect('employees.db')
        self.cursor = self.connection.cursor()
        self.cursor.execute('''CREATE TABLE IF NOT EXISTS employees
                        (id INTEGER PRIMARY KEY AUTOINCREMENT,
                        full_name TEXT NOT NULL,
                        phone_number TEXT NOT NULL,
                        email_address TEXT NOT NULL,
                        salary REAL NOT NULL)''')
        self.connection.commit()
    def insert_data(self, full_name, phone_number, email_address, salary):
    #ввод в базу данных
        self.cursor.execute('''
                        INSERT INTO Users (full_name, phone_number, email_address, salary)
                        VALUES (?, ?, ?, ?)''', (full_name, phone_number, email_address, salary))
        self.connection.commit() #сохранение
        



class Second(tk.Frame):
    def init(self):  #конструктор класса
        super().init(root) #родитель класса
        self.db = db #привязка переменной
        self.add_employee()
        self.update_employee()
        self.delete_employee()
        self.search_employee()

# добавляем сотрудника
    def add_employee(full_name, phone_number, email_address, salary):
        connection = sqlite3.connect('employees.db')
        cursor = connection.cursor()
        cursor.execute("INSERT INTO employees (full_name, phone_number, email_address, salary) VALUES (?, ?, ?, ?)",
                    (full_name, phone_number, email_address, salary))
        connection.commit()

    # обновляем данные сотрудника
    def update_employee(employee_id, full_name, phone_number, email_address, salary):
        connection = sqlite3.connect('employees.db')
        cursor = connection.cursor()
        cursor.execute("UPDATE employees SET full_name=?, phone_number=?, email_address=?, salary=? WHERE id=?",
                    (full_name, phone_number, email_address, salary, employee_id))
        connection.commit()

    # удаляем сотрудника
    def delete_employee(employee_id):
        connection = sqlite3.connect('employees.db')
        cursor = connection.cursor()
        cursor.execute("DELETE FROM employees WHERE id=?", (employee_id,))
        connection.commit()

    # поиск сотрудника
    def search_employee(full_name):
        connection = sqlite3.connect('employees.db')
        cursor = connection.cursor()
        cursor.execute("SELECT * FROM employees WHERE full_name LIKE ?", ('%'+full_name+'%',))
        results = cursor.fetchall()
        return results

# основной код 
class Main(tk.Toplevel):
    def init(self, root):  #конструктор класса
        super().init(root) #родитель класса
        self.main() #привязка функции
        self.db = db #привязка переменной


    def main(self):
        self.title('Выбор действия') #заголовок
        self.geometry('450x200') #размеры окна 
        self.configure(bg='#dcf5f9') #цвет
        self.resizable(False, False) #для отсутствия поломки пользователем
        self.grab_set() #все события
        self.focus_set() #фокус

        # вывод кода команды и её описания
        while True:
            print('1. Добавить сотрудника')
            print('2. Изменить сотрудника')
            print('3. Удалить сотрудника')
            print('4. Поиск по ФИО')
            print('0. Выход')
            choice = input('Выберите действие: ')

            # ввод данных нового сотрудника
            if choice == '1':
                full_name = input('Введите ФИО: ')
                phone_number = input('Введите номер телефона: ')
                email_address = input('Введите адрес электронной почты: ')
                salary = int(input('Введите заработную плату: '))
                Second.add_employee(full_name, phone_number, email_address, salary)

> julianka ♡︎:
# ввод обновлённых данных сотрудника
            elif choice == '2':
                employee_id = int(input('Введите ID сотрудника: '))
                full_name = input('Введите новое ФИО: ')
                phone_number = input('Введите новый номер телефона: ')
                email_address = input('Введите новый адрес электронной почты: ')
                salary = int(input('Введите новую заработную плату: '))
                Second.update_employee(employee_id, full_name, phone_number, email_address, salary)

            # удаление сотрудника
            elif choice == '3':
                employee_id = int(input('Введите ID сотрудника: '))
                Second.delete_employee(employee_id)

            # поиск сотрудника
            elif choice == '4':
                full_name = input('Введите ФИО для поиска: ')
                results = Second.search_employee(full_name)
                if len(results) == 0:
                    print('Сотрудников не найдено')
                else:
                    for result in results:
                        print(f"ФИО: {result[1]}, Номер телефона: {result[2]}, Адрес электронной почты: {result[3]}, Заработная плата: {result[4]}")

            # завершение кода (типа крестика на окнах програм, крч выход из програмы...)
            elif choice == '0':
                connection = sqlite3.connect('employees.db')
                connection.close()
                break
    def open_second(self):
        Second()
# точка входа
if name == 'main':
    root = tk.Tk() #создание окна
    db = DataBase() #переменная для подключения к БД
    app = Main(root) #присоединяем класс окна к переменной 
    root.title('Сотрудники компании') #заголовok
    root.configure(bg='#edf6f8') #цвет фона
    root.geometry('745x450') #размеры окна 
    root.resizable(False, False) #для отсутствия поломки пользователем
    root.mainloop() #показ окна
