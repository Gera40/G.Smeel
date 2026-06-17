# G.Smeel
Смайлик 
import logging
from aiogram import Bot, Dispatcher, F
from aiogram.types import Message, InlineKeyboardMarkup, InlineKeyboardButton, WebAppInfo
from aiogram.filters import Command

# Твой токен от BotFather
TOKEN = "ТВОЙ_ТОКЕН"
bot = Bot(8930259988:AAGvXW-jtH5wfCunG38ZeAG1zdxIT5QvxDw)
dp = Dispatcher()

# Логика настроений (универсальная)
def get_mood_emoji(text):
    text = text.lower()
    if any(w in text for w in ["счаст", "рад"]): return "😊 ✨ ☀️ 🌈 🥳"
    if any(w in text for w in ["груст", "печал"]): return "😢 😔 💧 🥀 🖤"
    if any(w in text for w in ["зл", "гнев", "бесит"]): return "😡 🔥 💢 👊 👿"
    if any(w in text for w in ["люб", "нежн"]): return "❤️ 🥰 😍 💌 💖"
    return "🤔 Не понял, напиши еще раз!"

@dp.message(Command("start"))
async def start(message: Message):
    # Кнопка, которая открывает мини-апп прямо внутри Telegram
    # Вместо URL вставь ссылку на свой index.html, залитый на GitHub Pages
    web_app = WebAppInfo(url="https://ТВОЯ_ССЫЛКА_НА_GITHUB_PAGES.html")
    
    markup = InlineKeyboardMarkup(inline_keyboard=[
        [InlineKeyboardButton(text="Открыть выбор настроения", web_app=web_app)]
    ])
    await message.answer("Привет! Нажми кнопку, чтобы выбрать свое настроение в приложении:", reply_markup=markup)

@dp.message(F.web_app_data)
async def web_app_handler(message: Message):
    # Бот принимает данные из мини-аппа и шлет эмодзи
    data = message.web_app_data.data
    await message.answer(f"Твой результат: {get_mood_emoji(data)}")

if __name__ == "__main__":
    dp.run_polling(bot)
    
