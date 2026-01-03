import telebot
import time
from telebot.types import ReplyKeyboardMarkup, KeyboardButton

bot = telebot.TeleBot("8507174903:AAHlwMz07uQh_aJy_YGSZXEcLqWBF2-ISIc")

markup_main = ReplyKeyboardMarkup(resize_keyboard=True, row_width=2)
markup_main.add(
    KeyboardButton("💰 Купить товар"),
    KeyboardButton("📢 Канал"),
    KeyboardButton("🏪 Магазин"),
    KeyboardButton("👨‍💼 Поддержка"),
    KeyboardButton("⭐ Отзывы")
)

@bot.message_handler(commands=['start'])
def send_welcome(message):
    bot.reply_to(message, 
                 "👋 Привет! Добро пожаловать в наш магазин!\n\n"
                 "Я помогу тебе быстро найти нужную информацию и совершить покупку.\n\n"
                 "Выбери действие на клавиатуре ниже или используй команды:",
                 reply_markup=markup_main)

@bot.message_handler(commands=['help'])
def send_help(message):
    help_text = (
        "📋 Список доступных команд:\n\n"
        "/start — запустить бота и увидеть главное меню\n"
        "/help — показать это сообщение\n"
        "/buy — информация о покупке товара\n"
        "/admins — контакты поддержки\n"
        "/reviews — отзывы о маркете\n\n"
        "Или просто используй кнопки ниже 👇"
    )
    bot.reply_to(message, help_text, reply_markup=markup_main)

@bot.message_handler(commands=['admins'])
def send_admins(message):
    bot.reply_to(message, 
                 "👨‍💼 Поддержка:\n"
                 "По всем вопросам пиши напрямую — @tqauk (главный администратор)\n"
                 "Также можешь написать @spamm0\n\n"
                 "Отвечаем максимально быстро!", 
                 reply_markup=markup_main)

@bot.message_handler(commands=['buy'])
def send_buy(message):
    bot.reply_to(message, 
                 "💰 Чтобы купить товар:\n\n"
                 "1. Перейди в @CryptoBot\n"
                 "2. Нажми START и вставь эту команду:\n"
                 "/start IVF3sy5nVFJG\n\n"
                 "После оплаты товар придёт автоматически 🚀",
                 parse_mode='Markdown',
                 reply_markup=markup_main)

@bot.message_handler(commands=['reviews'])
def send_reviews(message):
    bot.reply_to(message, 
                 "⭐ Отзывы о нашем маркете можно посмотреть в канале:\n"
                 "https://t.me/pricegranatov\n\n"
                 "Там реальные скрины сделок и отзывы покупателей 😉",
                 reply_markup=markup_main,
                 disable_web_page_preview=False)

@bot.message_handler(content_types=['text'])
def handle_text(message):
    if message.text == "💰 Купить товар":
        send_buy(message)
    elif message.text == "📢 Канал":
        bot.reply_to(message, 
                     "📢 Наш канал с прайсом и новостями:\n"
                     "https://t.me/pricegranatov",
                     reply_markup=markup_main)
    elif message.text == "🏪 Магазин":
        bot.reply_to(message, 
                     "🏪 Информация о магазине и правила:\n"
                     "https://t.me/cnannelinfo",
                     reply_markup=markup_main)
    elif message.text == "👨‍💼 Поддержка":
        send_admins(message)
    elif message.text == "⭐ Отзывы":
        send_reviews(message)


bot.remove_webhook()
time.sleep(1)
bot.infinity_polling()
