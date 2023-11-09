import tkinter as tk
import pygame
from random import choice
import time
from pygame import mixer  # Импортируем mixer из pygame

# Инициализация pygame для воспроизведения звука
pygame.mixer.init()

# Инициализация звукового модуля pygame
mixer.init()

# Загрузка фоновой музыки
mixer.music.load("/storage/emulated/0/Download/Game2023/music.mp3")
mixer.music.play(-1)  # -1 означает воспроизведение в бесконечном цикле

# Глобальные переменные
selected_color = None  # Глобальная переменная для выбранного цвета
start_time = None  # Глобальная переменная для начального времени

# Функция, которая будет вызываться при нажатии на кнопки
def button_click(button):
    # Генерируем случайный цвет
    new_color = choice(["red", "green", "blue", "yellow", "orange", "purple"])
    button.config(bg=new_color)  # Устанавливаем цвет кнопки
    global selected_color
    selected_color = new_color

    # Воспроизводим звук
    sound = pygame.mixer.Sound("/storage/emulated/0/Download/Game2023/but.mp3")  # Укажите путь к аудиофайлу
    sound.play()
    
    # Проверяем, все ли кнопки имеют одинаковый цвет
    if all(btn['bg'] == selected_color for btn in buttons):
        end_time = time.time()
        show_end_screen(end_time - start_time)  # Передаем потраченное время

# Функция для отображения экрана "Конец"
def show_end_screen(elapsed_time):
    end_screen = tk.Toplevel(root)
    end_screen.title("Конец")
    end_screen.geometry("1050x600")  # Укажите желаемый размер окна
    end_label = tk.Label(end_screen, text=f"Поздравляю!\nВремя: {elapsed_time:.2f} секунд", font=("Helvetica", 12))
    end_label.pack(fill="both", expand=True)
    ok_button = tk.Button(end_screen, text="Ок", command=root.destroy)  # Кнопка "Ок" для выхода из приложения
    ok_button.pack()
    end_screen.transient(root)
    end_screen.grab_set()
    root.wait_window(end_screen)

# Функция для проверки наличия слова "конец"
def check_for_end():
    for btn in buttons:
        if btn['bg'] != selected_color:
            return
    end_label.config(text="Конец игры!")
    end_sound.play()

# Функция для отображения условий игры в начале
def show_game_conditions():
    conditions_screen = tk.Toplevel(root)
    conditions_screen.title("Условия игры")
    conditions_screen.geometry("1050x400")  # Укажите желаемый размер окна
    conditions_label = tk.Label(conditions_screen, text="Условия игры:\n\n1. Нажмите на кнопки, чтобы изменить их цвет.\n2. Ваша цель - сделать все кнопки одного цвета.", font=("Helvetica", 8))
    conditions_label.pack(fill="both", expand=True)
    
    start_button = tk.Button(conditions_screen, text="Старт", command=conditions_screen.destroy)
    start_button.pack()

    conditions_screen.transient(root)
    conditions_screen.grab_set()
    root.wait_window(conditions_screen)

# Создаем основное окно
root = tk.Tk()
root.title("Пример с 16 кнопками")

# Показываем условия игры в начале
show_game_conditions()

# Создаем и размещаем 16 кнопок в 4 столбца
buttons = []
for i in range(1, 17):
    button = tk.Button(root, text=f"")
    button.grid(row=(i-1)//4, column=(i-1)%4, sticky='nsew')
    button.config(command=lambda btn=button: button_click(btn))  # Привязываем функцию к кнопке
    buttons.append(button)

# Создаем строки и столбцы, чтобы кнопки растягивались на весь экран
for i in range(4):
    root.grid_rowconfigure(i, weight=1)
    root.grid_columnconfigure(i, weight=1)

# Звук для слова "конец"
end_sound = pygame.mixer.Sound("/storage/emulated/0/Download/3.mp3")  # Укажите путь к аудиофайлу

# Запускаем главный цикл приложения
root.after(1000, check_for_end)  # Запускаем функцию check_for_end через 1 секунду
start_time = time.time()  # Запоминаем начальное время
root.mainloop()