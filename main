import yt_dlp
import os
from telegram import Update # type: ignore
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes, MessageHandler, filters, Application 
import asyncio
TOKEN = "8527471359:AAHmEm8UxP7-I0_7RIhqzXTosyz9WRMHG3Y"

fname = "audio.mp3"
settings = {
    'ffmpeg_location': r'C:\ffmpeg\bin',
    'format': 'bestaudio/best',
    'outtmpl': 'audio.%(ext)s',
    'postprocessors': [{
        'key': 'FFmpegExtractAudio',
        'preferredcodec': 'mp3',
        'preferredquality': '320',
    }],
}

async def error_handler(update, context):
    print(f"Ошибка: {context.error}")

async def music(update):
    url = update.message.text
    try:
        with yt_dlp.YoutubeDL(settings) as dwn:
            info = dwn.extract_info(url, download=False)
            if int(info.get("duration")) < 1800:
                dwn.download([url])
                await update.message.reply_audio(
                    audio = open(fname, 'rb')
                )
            else:
                await update.message.reply_text("слишком длинное")
    finally:
        if os.path.exists(fname):
            os.remove(fname)


app = ApplicationBuilder().token(TOKEN).build()
#app.add_handler(CommandHandler("start", start))
#app.add_handler(CommandHandler("roll", roll))
#app.add_handler(CommandHandler("credits", credits))
app.add_error_handler(error_handler)
app.add_handler(MessageHandler(filters.TEXT, music))
print("started")
app.run_polling()
