import logging
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import ApplicationBuilder, ContextTypes, CommandHandler, CallbackQueryHandler

# --- 1. CONFIGURATION ---
# Your verified credentials
TOKEN = '8730669228:AAFFYjbIus2vncwdGaDwSv-Ub2g6IkkoLkA'
MAIN_GROUP_ID = -1003679055484 

# Simple logging to see errors in Render
logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', level=logging.INFO)

# --- 2. THE WELCOME INTERFACE ---
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    # Professional buttons like CoinXpert
    keyboard = [
        [InlineKeyboardButton("🟩 CREATE ESCROW GROUP", callback_data='create_deal')],
        [InlineKeyboardButton("📜 RULES", callback_data='rules'), 
         InlineKeyboardButton("📖 INSTRUCTIONS", callback_data='instr')],
        [InlineKeyboardButton("❓ WHAT IS ESCROW?", callback_data='about')]
    ]
    reply_markup = InlineKeyboardMarkup(keyboard)
    
    welcome_text = (
        "💰 **Welcome to 24/7 Escrow Service**\n\n"
        "💳 **SERVICE FEES:**\n• $5.00 flat for deals under $100\n• 5.0% for deals over $100\n\n"
        "I support secure transactions for:\n"
        "**BTC • LTC • ETH • XMR • USDT**\n\n"
        "👇 Tap the button below to start a new secure transaction."
    )
    
    if update.effective_chat.type == "private":
        await update.message.reply_text(welcome_text, reply_markup=reply_markup, parse_mode='Markdown')
    else:
        await update.message.reply_text("Please message me in private to start a deal!")

# --- 3. THE BUTTON LOGIC ---
async def handle_buttons(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()

    if query.data == 'create_deal':
        try:
            # Generates a unique invite link for the buyer and seller
            invite = await context.bot.create_chat_invite_link(
                chat_id=MAIN_GROUP_ID,
                member_limit=2,
                name=f"Deal_User_{query.from_user.id}"
            )
            await query.edit_message_text(
                f"✅ **Secure Escrow Group Created!**\n\n"
                f"Forward this link to the other person involved in the deal:\n"
                f"🔗 {invite.invite_link}\n\n"
                "Once you are both inside, state your roles (Buyer/Seller) to begin."
            )
        except Exception as e:
            await query.edit_message_text(f"❌ Error: Bot must be an Admin in the group with 'Invite Users' permission.")

    elif query.data == 'rules':
        await query.edit_message_text("📜 **Service Rules:**\n\n1. No prohibited items.\n2. Both parties must confirm before funds are released.\n3. Fees are non-refundable once the transaction is locked.")

# --- 4. START THE BOT ---
if __name__ == '__main__':
    application = ApplicationBuilder().token(TOKEN).build()
    
    application.add_handler(CommandHandler('start', start))
    application.add_handler(CallbackQueryHandler(handle_buttons))
    
    print("Escrow Bot is running...")
    application.run_polling()
