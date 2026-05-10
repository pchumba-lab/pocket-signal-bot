import logging
import random
from datetime import datetime
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, ContextTypes

# ================== YOUR BOT TOKEN ==================
TOKEN = "8669374261:AAGziM5yMczIsluQyJ5c5q4Lw_65ed8rdA0"   # ← CHANGE THIS!

PAIRS = ["EURUSD", "GBPUSD", "USDJPY", "AUDUSD", "USDCAD", 
         "EURJPY", "GBPJPY", "NZDUSD", "EURGBP", "USDCHF"]

def generate_signal(asset: str):
    direction = random.choice(["CALL", "PUT"])
    expiry = random.choice([1, 3, 5])
    confidence = random.randint(68, 94)
    now = datetime.now().strftime("%H:%M:%S")
    emoji = "🟢" if direction == "CALL" else "🔴"
    
    return f"""
{emoji} NEW POCKET OPTION SIGNAL

Pair: {asset}
Direction: {direction} {'⬆️' if direction == "CALL" else '⬇️'}
Expiry: {expiry} minutes
Confidence: {confidence}%
Time: {now}

💰 Good luck on Pocket Option!
"""

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = []
    row = []
    for pair in PAIRS:
        row.append(InlineKeyboardButton(pair, callback_data=pair))
        if len(row) == 3:
            keyboard.append(row)
            row = []
    if row:
        keyboard.append(row)

    await update.message.reply_text(
        "🚀 Pocket Option Signal Bot\n\nClick any pair to get instant signal:",
        reply_markup=InlineKeyboardMarkup(keyboard)
    )

async def button(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    asset = query.data
    await query.answer()
    signal_text = generate_signal(asset)
    await query.edit_message_text(signal_text, parse_mode='Markdown')

def main():
    app = Application.builder().token(TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(CallbackQueryHandler(button))
    
    print("✅ Bot is running on Railway...")
    app.run_polling()

if name == "main":
    logging.basicConfig(level=logging.INFO)
    main()
