import os
import requests
import telebot

@bot.message_handler(commands=['start'])
def start_message(message):
    welcome_text = (
        "سڵاڤ! 👋\n\n"
        "  ئەڤ بۆیە بۆ ژێبرنا باگراوندێ رسما هاتیە چێکردن.\n\n"
        "🔹 **چاوانیا بکارئینانێ  :**\n"
        "1️⃣ رسمی بنێرە بۆ بۆتی .\n"
        "هەر کات پێویستت بە یارمەتی هەبێت، /help بنووسە."
        "ئەڤ بۆتە هاتیە دروست کردن ژلایێ @hamzayassiir"
    )
    bot.reply_to(message, welcome_text)

# وەرگرتنی Environment Variables
TOKEN = os.environ.get("8782044993:AAHUeWBnLirpGA1P6e4QiiSNn1SUekmhSEM")
API_KEY = os.environ.get("7rCFBH1mgexW4x2uiYKKXk6q")

bot = telebot.TeleBot(TOKEN)

@bot.message_handler(commands=['start'])
def start_message(message):
    bot.reply_to(message, "سڵاو! بۆتەکەت حاضره. وێنە بنێر بۆ لابردنی باگراوند.")

@bot.message_handler(content_types=['photo'])
def remove_bg(message):
    file_info = bot.get_file(message.photo[-1].file_id)
    downloaded_file = bot.download_file(file_info.file_path)

    response = requests.post(
        'https://api.remove.bg/v1.0/removebg',
        files={'image_file': downloaded_file},
        data={'size': 'auto'},
        headers={'X-Api-Key': API_KEY},
    )

    if response.status_code == requests.codes.ok:
        bot.send_photo(message.chat.id, response.content)
    else:
        bot.reply_to(message, "هەڵە ڕوویدا، تکایە دوبارە هەوڵ بدە!")

bot.infinity_polling()