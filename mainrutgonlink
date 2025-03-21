import logging
import requests
from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes

logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', level=logging.INFO)
logger = logging.getLogger(__name__)

def yeumoney_link(token, long_url):
    api_url = 'https://yeumoney.site/shortlink'
    params = {
        'token': token,
        'url': long_url
    }
    try:
        response = requests.get(api_url, params=params)
        response.raise_for_status()
        json_data = response.json()
        if 'shortenedUrl' in json_data:
            shortened_link = json_data['shortenedUrl']
            return shortened_link
        else:
            logger.error(json_data)
            return None
    except requests.RequestException as e:
        logger.error(f'Error during Yeumoney API call: {e}')
        return None

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text('Nhập URL để rút gọn link: /rutgonlink <URL>.')

async def shorten_url(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    args = context.args
    if len(args) == 1:
        long_url = args[0]
        if long_url.startswith(('http://', 'https://')):
            shortened_link = yeumoney_link('api yeumoney o day', long_url) 
            if shortened_link:
                await update.message.reply_text(f'Liên kết đã rút gọn: {shortened_link}')
            else:
                await update.message.reply_text('Không thể rút gọn, vui lòng thử lại hoặc kiểm tra lại URL.')
        else:
            await update.message.reply_text('Vui lòng đảm bảo link bắt đầu bằng http:// hoặc https://')
    else:
        await update.message.reply_text('Lệnh không đúng. Sử dụng /rutgonlink <URL>.')

def main() -> None:
    application = Application.builder().token('token bot').build()
    application.add_handler(CommandHandler('start', start))
    application.add_handler(CommandHandler('rutgonlink', shorten_url))

    application.run_polling()

if __name__ == '__main__':
    print("Bot chạy thành công!")
    main()
