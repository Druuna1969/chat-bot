# chat-bot
Код чат-бота с интеграцией в Telegram и веб-интерфейс
python
Copy
import nltk
from nltk.tokenize import word_tokenize
from flask import Flask, request
import telebot

# Установка токена для Telegram
TOKEN = "ВАШ_ТОКЕН_ТЕЛЕГРАМ"
bot = telebot.TeleBot(TOKEN)
app = Flask(__name__)

# Функция для обработки запросов и генерации ответов
def chatbot_response(user_input):
    responses = {
        "привет": "Здравствуйте! Чем могу помочь?",
        "офис": "Наши офисы находятся в Барнауле (пр-т Красноармейский, д. 77, офис 301) и Москве (ул. Мещанская, дом 9/14, стр. 1).",
        "услуги": "Мы занимаемся экспертной деятельностью. Подробнее можно узнать на нашем сайте или по телефону.",
        "директор": "Исполнительный директор Муравьев Александр Александрович. Вы можете направить официальный запрос на нашу почту."
    }
    
    # Токенизация ввода пользователя
    tokens = word_tokenize(user_input.lower())
    for word in tokens:
        if word in responses:
            return responses[word]
    
    # Ответ по умолчанию
    return "Извините, я пока не знаю ответа на этот вопрос. Пожалуйста, свяжитесь с нашим специалистом."

# Обработчик сообщений Telegram
@bot.message_handler(func=lambda message: True)
def handle_message(message):
    response = chatbot_response(message.text)
    bot.send_message(message.chat.id, response)

# Веб-интерфейс (Flask)
@app.route("/", methods=["GET", "POST"])
def chat():
    if request.method == "POST":
        user_input = request.form["message"]
        return chatbot_response(user_input)
    return "Чат-бот для ООО 'ЭКСПЕРТИЗА' работает!"

# Запуск бота
if __name__ == "__main__":
    bot.polling(none_stop=True)
    app.run(host="0.0.0.0", port=5000)
Как запустить бота
Для Telegram:
Получите токен у @BotFather в Telegram.

Вставьте токен в переменную TOKEN в коде.

Запустите скрипт, и бот начнет отвечать в чате.

Для веб-интерфейса:
Установите Flask:

bash
Copy
pip install flask
Запустите приложение командой:

bash
Copy
python script.py
Доступ к веб-чату можно получить по адресу: http://localhost:5000/.
