[vvs.py](https://github.com/user-attachments/files/27054547/vvs.py)
quotes = [
    {"text": "Будьте собой; все остальные уже заняты.", "author": "Оскар Уайльд", "topic": "Мотивация"},
    {"text": "Жизнь — это то, что происходит, пока вы строите планы.", "author": "Джон Леннон", "topic": "Жизнь"},
    {"text": "Успех — это умение идти от неудачи к неудаче, не теряя энтузиазма.", "author": "Уинстон Черчилль", "topic": "Мотивация"},
    # добавьте больше цитат по желанию
]
import tkinter as tk
from tkinter import ttk, messagebox
import random
import json
import os

# Глобальные переменные
history = []

# Загрузка истории из файла
def load_history():
    global history
    if os.path.exists("history.json"):
        with open("history.json", "r", encoding="utf-8") as f:
            history = json.load(f)
    else:
        history = []

# Сохранение истории
def save_history():
    with open("history.json", "w", encoding="utf-8") as f:
        json.dump(history, f, ensure_ascii=False, indent=4)

# Выбор случайной цитаты
def generate_quote():
    quote = random.choice(quotes)
    history.append(quote)
    save_history()
    update_history_list()
    display_quote(quote)

# Обновление отображения истории
def update_history_list(filtered_quotes=None):
    listbox.delete(0, tk.END)
    display_list = filtered_quotes if filtered_quotes is not None else history
    for q in display_list:
        listbox.insert(tk.END, f'"{q["text"]}" - {q["author"]} [{q["topic"]}]')

# Отобразить цитату
def display_quote(quote):
    quote_text.delete(1.0, tk.END)
    quote_text.insert(tk.END, f'"{quote["text"]}"\n\n- {quote["author"]}\n[{quote["topic"]}]')

# Фильтрация
def filter_quotes():
    author_filter = author_entry.get().strip().lower()
    topic_filter = topic_entry.get().strip().lower()
    filtered = []
    for q in history:
        if author_filter and author_filter not in q["author"].lower():
            continue
        if topic_filter and topic_filter not in q["topic"].lower():
            continue
        filtered.append(q)
    update_history_list(filtered)

# GUI
root = tk.Tk()
root.title("Random Quote Generator")

load_history()

# Кнопка генерации
generate_button = ttk.Button(root, text="Сгенерировать цитату", command=generate_quote)
generate_button.pack(pady=10)

# Поле для отображения цитаты
quote_text = tk.Text(root, height=5, width=60, wrap=tk.WORD)
quote_text.pack(pady=10)

# Фильтры
filter_frame = ttk.Frame(root)
filter_frame.pack(pady=10)

ttk.Label(filter_frame, text="Автор:").grid(row=0, column=0, padx=5)
author_entry = ttk.Entry(filter_frame)
author_entry.grid(row=0, column=1, padx=5)

ttk.Label(filter_frame, text="Тема:").grid(row=0, column=2, padx=5)
topic_entry = ttk.Entry(filter_frame)
topic_entry.grid(row=0, column=3, padx=5)

filter_button = ttk.Button(filter_frame, text="Применить фильтр", command=filter_quotes)
filter_button.grid(row=0, column=4, padx=5)

# История
ttk.Label(root, text="История цитат:").pack()
listbox = tk.Listbox(root, width=80, height=10)
listbox.pack()

# Инициализация
update_history_list()

root.mainloop()
git init
git add .
git commit -m "Initial commit: Random Quote Generator"
