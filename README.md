import telebot
import sqlite3
import random
from datetime import datetime
from telebot.types import InlineKeyboardMarkup, InlineKeyboardButton, ReplyKeyboardMarkup, KeyboardButton, WebAppInfo

bot = telebot.TeleBot("7479961446:AAH418Vo8WwBY8yV8xV9hmj0wlEYUO3LFkI")
user_data = {}

HELLO_ANSWERS = [
    "Здравствуйте, {name}. Чем могу быть полезна?",
    "Добрый день, {name}. Я готова помочь.",
    "Приветствую, {name}. Если потребуется помощь — обращайтесь.",
    "Здравствуйте, {name}. Я на связи.",
    "Привет! Чем могу помочь, {name}?"
]
HOW_ARE_YOU_ANSWERS = [
    "У меня всё стабильно. Спасибо за вопрос.",
    "Всё в порядке. Готова к работе.",
    "Спасибо, что спросили. Всё хорошо.",
    "Я функционирую в штатном режиме.",
    "Всё отлично, {name}! А у вас?"
]
WHAT_DOING_ANSWERS = [
    "Ожидаю ваших команд.",
    "Готова помочь, если потребуется.",
    "В данный момент свободна для ваших запросов.",
    "Следую своим задачам. Если что-то нужно — обращайтесь.",
    "Читаю ваши сообщения и думаю, чем помочь."
]
THANKS_ANSWERS = [
    "Пожалуйста, {name}. Всегда рада помочь!",
    "Обращайтесь, {name}. Для этого я и создана.",
    "Рада быть полезной.",
    "Не за что. Если что — я рядом.",
    "Спасибо вам за обращение!"
]
JOKE_ANSWERS = [
    "Почему программисты путают Хэллоуин и Рождество? Потому что 31 OCT = 25 DEC.",
    "Бывают два типа людей: те, кто понимает двоичный код, и те, кто нет.",
    "Знаете, почему компьютер не может держать секреты? Потому что у него всегда есть утечка памяти.",
    "Ошибка — это не баг, а фича! 😉",
    "Самая короткая программа: 'return 0;'"
]
WEATHER_ANSWERS = [
    "Я не синоптик, но могу предположить, что за окном лучше, чем в коде.",
    "Погода всегда разная, а я всегда на связи.",
    "Погода не влияет на мою работоспособность. А у вас как?",
    "Проверьте прогноз в интернете — я больше по цифровым облакам.",
    "Погода отличная для общения со мной!"
]
COMPLIMENT_ANSWERS = [
    "Спасибо, {name}. Вы тоже ничего!",
    "Приятно слышать. Но не забывайте — я всего лишь программа.",
    "Спасибо за комплимент! Я стараюсь быть полезной.",
    "Ваша доброта засчитана, {name}.",
    "Спасибо, мне приятно!"
]
BYE_ANSWERS = [
    "До свидания, {name}. Если что — пишите.",
    "Буду ждать новых задач. Хорошего дня!",
    "Пока! Не забывайте возвращаться.",
    "До встречи, {name}.",
    "До скорого!"
]
UNKNOWN_ANSWERS = [
    "Извините, я не совсем поняла ваш запрос.",
    "Пожалуйста, уточните, что вы имели в виду.",
    "Не уверена, что понимаю. Можете переформулировать?",
    "Ваше сообщение не распознано. Попробуйте иначе.",
    "Я пока не знаю, как ответить на это. Попробуйте спросить по-другому."
]
TIME_ANSWERS = [
    "Сейчас {time}.",
    "Текущее время: {time}.",
    "На часах {time}.",
    "Время на сервере: {time}."
]
DATE_ANSWERS = [
    "Сегодня {date}.",
    "Текущая дата: {date}.",
    "Сегодня на календаре {date}.",
    "Дата сегодня: {date}."
]
WHO_ARE_YOU_ANSWERS = [
    "Я — ваш виртуальный ассистент-бот на Python.",
    "Я бот, созданный для помощи и общения.",
    "Я Telegram-бот, могу отвечать на вопросы, шутить и помогать.",
    "Я — цифровой помощник. Могу рассказать анекдот, найти сайт, помочь с регистрацией."
]
WHAT_CAN_YOU_DO_ANSWERS = [
    "Я могу отвечать на вопросы, шутить, показывать сайты, помогать с регистрацией и многое другое!",
    "Я умею отправлять ссылки, рассказывать анекдоты, помогать с регистрацией и просто общаться.",
    "Могу открыть веб-приложение, рассказать шутку, ответить на ваши вопросы и многое другое.",
    "Я создана для помощи, общения и хорошего настроения!"
]
EMOJIS = [
    "🙂", "📝", "👩‍💻", "⚡", "🤖", "💼", "😉", "🕒", "⏳", "🧐", "🗂️",
    "📋", "🤔", "🙄", "🕵️", "🌐", "⚠️", "🤓", "⏰", "🛠️", "🔒", "📲", "📷"
]

def maybe_add_emoji(text: str) -> str:
    """Добавляет эмодзи к тексту с вероятностью 50%."""
    return f"{text} {random.choice(EMOJIS)}" if random.random() < 0.5 else text

@bot.message_handler(commands=['webapp'])
def webapp(message):
    markup = ReplyKeyboardMarkup(resize_keyboard=True)
    markup.add(
        KeyboardButton(
            text="Открыть WebApp",
            web_app=WebAppInfo(url="https://kinoogo.zone")
        )
    )
    bot.send_message(
        message.chat.id,
        "Откройте наше веб-приложение:",
        reply_markup=markup
    )

@bot.message_handler(commands=['site'])
def site(message):
    markup = InlineKeyboardMarkup()
    markup.add(InlineKeyboardButton("Открыть сайт", url="https://kinoogo.zone/"))
    bot.send_message(
        message.chat.id,
        maybe_add_emoji("Сайт для просмотра фильмов:"),
        reply_markup=markup
    )

@bot.message_handler(commands=['youtube', 'yt'])
def youtube(message):
    markup = InlineKeyboardMarkup()
    markup.add(InlineKeyboardButton("Открыть YouTube", url="https://www.youtube.com/"))
    bot.send_message(
        message.chat.id,
        maybe_add_emoji("YouTube:"),
        reply_markup=markup
    )

@bot.message_handler(commands=['Info'])
def info_command(message):
    bot.send_message(
        message.chat.id,
        maybe_add_emoji("<b>Я — ваш виртуальный ассистент. Готова помочь с вопросами и задачами.</b>"),
        parse_mode="html"
    )

@bot.message_handler(commands=['start'])
def start(message):
    name = f"{message.from_user.first_name or ''} {getattr(message.from_user, 'last_name', '')}".strip()
    markup = ReplyKeyboardMarkup(resize_keyboard=True)
    markup.add(
        KeyboardButton(
            text="Открыть WebApp",
            web_app=WebAppInfo(url="https://kinoogo.zone")
        )
    )
    bot.send_message(
        message.chat.id,
        maybe_add_emoji(f"Здравствуйте, {name}. Для регистрации используйте команду /register."),
        reply_markup=markup
    )

@bot.message_handler(commands=['register'])
def register(message):
    try:
        with sqlite3.connect('assi.sql') as conn:
            conn.execute("CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, pass TEXT)")
    except Exception as e:
        bot.send_message(message.chat.id, maybe_add_emoji(f"Ошибка базы данных: {e}"))
        return
    bot.send_message(message.chat.id, maybe_add_emoji("Пожалуйста, укажите ваше имя."))
    bot.register_next_step_handler(message, user_name)

def user_name(message):
    user_data[message.chat.id] = {'name': message.text}
    bot.send_message(message.chat.id, maybe_add_emoji("Теперь придумайте пароль."))
    bot.register_next_step_handler(message, user_pass)

def user_pass(message):
    name = user_data[message.chat.id]['name']
    password = message.text
    try:
        with sqlite3.connect('assi.sql') as conn:
            conn.execute("INSERT INTO users (name, pass) VALUES (?, ?)", (name, password))
    except Exception as e:
        bot.send_message(message.chat.id, maybe_add_emoji(f"Ошибка базы данных: {e}"))
        return
    bot.send_message(message.chat.id, maybe_add_emoji("Регистрация завершена. Если потребуется помощь — обращайтесь."))

@bot.message_handler(commands=['Help'])
def help_command(message):
    bot.send_message(message.chat.id, maybe_add_emoji(
        "Я могу:\n"
        "- Открыть сайт (/site)\n"
        "- Открыть WebApp (/webapp)\n"
        "- Открыть YouTube (/youtube)\n"
        "- Зарегистрировать вас (/register)\n"
        "- Рассказать анекдот, ответить на вопросы, показать время и дату.\n"
        "Просто напишите свой вопрос!"
    ))

@bot.message_handler(content_types=['photo'])
def get_photo(message):
    bot.reply_to(message, maybe_add_emoji("Фотография получена. Спасибо."))

@bot.message_handler(func=lambda message: True)
def text_handler(message):
    if not hasattr(message, 'text') or message.text is None:
        return
    text = message.text.lower()
    name = f"{message.from_user.first_name or ''} {getattr(message.from_user, 'last_name', '')}".strip()

    # Приветствия
    if text in ["что делаешь?", "как ты?", "чем занимаешься?", "чем занят?", "чем занимаешься"]:
        answer = random.choice(WHAT_DOING_ANSWERS).format(name=name)
    elif text in ['как дела?', "как у тебя дела?", "как настроение?", "как жизнь?"]:
        answer = random.choice(HOW_ARE_YOU_ANSWERS).format(name=name)
    elif text in ["привет", "здравствуй", "добрый день", "доброе утро", "добрый вечер", "хай", "hello", "hi"]:
        answer = random.choice(HELLO_ANSWERS).format(name=name)
    # Благодарности
    elif any(word in text for word in ["спасибо", "благодарю", "спс", "благодарствую", "thanks", "thank you"]):
        answer = random.choice(THANKS_ANSWERS).format(name=name)
    # Анекдоты
    elif any(word in text for word in ["анекдот", "шутка", "рассмеши", "юмор", "шутку"]):
        answer = random.choice(JOKE_ANSWERS)
    # Погода
    elif any(word in text for word in ["погода", "на улице", "за окном", "какая погода"]):
        answer = random.choice(WEATHER_ANSWERS)
    # Комплименты
    elif any(word in text for word in ["ты классная", "ты крутая", "молодец", "умница", "лучшая", "крутая", "умная"]):
        answer = random.choice(COMPLIMENT_ANSWERS).format(name=name)
    # Прощания
    elif any(word in text for word in ["пока", "до свидания", "до встречи", "bye", "goodbye", "прощай"]):
        answer = random.choice(BYE_ANSWERS).format(name=name)
    # Время
    elif any(word in text for word in ["время", "сколько времени", "который час", "time"]):
        now = datetime.now().strftime("%H:%M")
        answer = random.choice(TIME_ANSWERS).format(time=now)
    # Дата
    elif any(word in text for word in ["дата", "какое сегодня число", "какой сегодня день", "date"]):
        today = datetime.now().strftime("%d.%m.%Y")
        answer = random.choice(DATE_ANSWERS).format(date=today)
    # Кто ты
    elif any(word in text for word in ["кто ты", "что ты за бот", "ты кто", "кто ты такая"]):
        answer = random.choice(WHO_ARE_YOU_ANSWERS)
    # Что умеешь
    elif any(word in text for word in ["что ты умеешь", "твои возможности", "что можешь", "умеешь"]):
        answer = random.choice(WHAT_CAN_YOU_DO_ANSWERS)
    # Помощь
    elif any(word in text for word in ["помоги", "help", "нужна помощь", "подскажи", "подскажи что делать"]):
        answer = "Для списка возможностей напишите /help или задайте свой вопрос!"
    else:
        answer = random.choice(UNKNOWN_ANSWERS).format(name=name)
    bot.send_message(message.chat.id, maybe_add_emoji(answer))

if __name__ == "__main__":
    try:
        bot.polling(none_stop=True)
    except Exception as e:
        print(f"Ошибка запуска бота: {e}")
