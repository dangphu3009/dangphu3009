import telebot
import requests

API_TOKEN = '8018488537:AAFvtTKvYGcfmiluaFVxi-Yh4E5huLafSbA'
bot = telebot.TeleBot(API_TOKEN)

@bot.message_handler(commands=['start'])
def start(message):
    bot.reply_to(message,
        "Chào bạn! TÔI LÀ BOT [ DANGPHU.SITE ]\n\n"
        "Lệnh:\n"
        "/buff <id tiktok của bạn> để buff tài khoản\n"
        "Ví dụ: /buff just.30th9 (id của bạn)\n\n"
        "Liên hệ ADMIN: [Facebook Admin](https://www.facebook.com/just.30th9)",
        parse_mode="Markdown"
    )

@bot.message_handler(commands=['buff'])
def buff(message):
    parts = message.text.split()
    if len(parts) < 2:
        bot.reply_to(message, "❗ Vui lòng nhập username sau lệnh. Ví dụ: /buff id tiktok của bạn")
        return

    username = parts[1]
    bot.reply_to(message, "⏳ ĐANG XỬ LÝ...")

    try:
        url = f"https://guanghai.x10.mx/fl.php?key=lequanghai291008&username={username}"
        res = requests.get(url)
        data = res.json()
        stats = data.get("data", {}).get("stats", {})

        if stats.get("success"):
            reply = f"""🔷 BUFF THÀNH CÔNG
👾 Username: {stats['username']}
🕹️ Nickname: {stats['nickname'] or 'Không có'}
👤 Followers: {stats['followers_before']} ➡ {stats['followers_after']}
🎮 Tăng: +{stats['increase']}
🧩 Likes: {stats['likes'] or 0}
"""
            bot.reply_to(message, reply)
        else:
            bot.reply_to(message, "❗ VUI LÒNG CHỜ THÊM THỜI GIAN")
    except Exception as e:
        bot.reply_to(message, f"⚠️ Lỗi: {e}")

bot.infinity_polling()
