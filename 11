pip install flask python-telegram-bot
import logging
from telegram import Update
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, ConversationHandler, CallbackContext

# 1. 로깅 설정
logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
                    level=logging.INFO)
logger = logging.getLogger(__name__)

# 2. 사용자 상태 정의
ACTIVITY, ENVIRONMENT, EXPERIENCE, PRIORITY = range(4)

# 3. 사용자가 입력한 성향을 저장할 딕셔너리
user_data = {}

# 4. 각 단계별 질문
def start(update: Update, context: CallbackContext) -> int:
    update.message.reply_text(
        "안녕하세요! 여행지 추천을 도와드리겠습니다. 😊\n"
        "먼저, 어떤 종류의 활동을 즐기시나요?\n"
        "1. 문화/역사 탐방\n2. 자연 탐험\n3. 쇼핑\n4. 액티비티\n5. 예술"
    )
    return ACTIVITY

def activity(update: Update, context: CallbackContext) -> int:
    user_choice = update.message.text
    user_data[update.message.chat_id] = {"activity": user_choice}
    
    update.message.reply_text(
        "어떤 환경에서 여행을 즐기고 싶으신가요?\n"
        "1. 도심\n2. 자연\n3. 바다\n4. 유적지"
    )
    return ENVIRONMENT

def environment(update: Update, context: CallbackContext) -> int:
    user_choice = update.message.text
    user_data[update.message.chat_id]["environment"] = user_choice
    
    update.message.reply_text(
        "여행 중 어떤 경험을 가장 중시하시나요?\n"
        "1. 사진 명소\n2. 문화 체험\n3. 힐링\n4. 도전적인 활동\n5. 새로운 음식 시도"
    )
    return EXPERIENCE

def experience(update: Update, context: CallbackContext) -> int:
    user_choice = update.message.text
    user_data[update.message.chat_id]["experience"] = user_choice
    
    update.message.reply_text(
        "여행 중 어떤 것을 가장 중요하게 생각하시나요?\n"
        "1. 좋은 접근성\n2. 독특한 장소\n3. 저렴한 가격\n4. 안전하고 편안한 환경"
    )
    return PRIORITY

def priority(update: Update, context: CallbackContext) -> int:
    user_choice = update.message.text
    user_data[update.message.chat_id]["priority"] = user_choice

    # 성향을 바탕으로 추천을 제공
    update.message.reply_text(generate_recommendation(update.message.chat_id))

    return ConversationHandler.END

def generate_recommendation(chat_id: int) -> str:
    # 사용자의 성향을 바탕으로 관광지 추천
    data = user_data.get(chat_id, {})
    
    activity = data.get("activity", "기타")
    environment = data.get("environment", "기타")
    experience = data.get("experience", "기타")
    priority = data.get("priority", "기타")

    # 추천 문구 작성
    recommendations = []

    if activity == "1":
        recommendations.append("문화/역사 탐방을 좋아하신다면, 경복궁, 창덕궁, 국립박물관을 추천합니다.")
    elif activity == "2":
        recommendations.append("자연 탐험을 좋아하신다면, 북한산, 설악산, 제주도 한라산을 추천합니다.")
    elif activity == "3":
        recommendations.append("쇼핑을 즐기시나요? 서울 명동, 동대문, 강남의 쇼핑몰을 추천합니다.")
    elif activity == "4":
        recommendations.append("액티비티를 좋아하신다면, 에버랜드, 롯데월드, 서핑을 즐길 수 있는 제주도를 추천합니다.")
    elif activity == "5":
        recommendations.append("예술을 좋아하신다면, 서울의 미술관과 전시회를 추천합니다.")
    
    if environment == "1":
        recommendations.append("도심 속에서는 서울 시내 명소들(남산타워, 청계천 등)을 추천해요.")
    elif environment == "2":
        recommendations.append("자연을 좋아하시면, 제주도의 오름, 설악산 등을 추천드립니다.")
    elif environment == "3":
        recommendations.append("바다를 좋아하신다면, 부산 해운대, 강릉 경포대 등을 추천합니다.")
    elif environment == "4":
        recommendations.append("유적지 여행을 원하시면 경주, 전주 한옥마을을 추천드립니다.")

    if experience == "1":
        recommendations.append("사진 찍기 좋은 명소로는 경복궁, 제주 오름 등이 있습니다.")
    elif experience == "2":
        recommendations.append("문화 체험을 원하시면 전통시장 방문, 한복 체험을 추천해요.")
    elif experience == "3":
        recommendations.append("힐링을 원하시면 온천이나 스파 리조트를 추천합니다.")
    elif experience == "4":
        recommendations.append("도전적인 활동을 원하시면 하이킹, 트래킹, 서핑 등을 추천해요.")
    elif experience == "5":
        recommendations.append("새로운 음식을 시도해 보고 싶으시면, 전주 비빔밥, 부산의 밀면을 추천합니다.")

    if priority == "1":
        recommendations.append("좋은 접근성을 원하시면 서울 도심에서 가까운 명소를 추천합니다.")
    elif priority == "2":
        recommendations.append("독특한 장소를 원하시면, 벽화마을이나 이색적인 테마파크를 추천해요.")
    elif priority == "3":
        recommendations.append("저렴한 가격대의 관광지로는 국립박물관, 무료 공원 등을 추천해요.")
    elif priority == "4":
        recommendations.append("안전하고 편안한 환경을 원하시면 가족 단위 여행지나 리조트를 추천드립니다.")

    return "\n".join(recommendations)

def cancel(update: Update, context: CallbackContext) -> int:
    update.message.reply_text("여행지 추천을 종료합니다. 다음에 또 찾아주세요!")
    return ConversationHandler.END

def main():
    # 5. Updater와 Dispatcher 설정
    updater = Updater("YOUR_BOT_API_KEY", use_context=True)
    dp = updater.dispatcher

    # 6. 대화 흐름 설정
    conv_handler = ConversationHandler(
        entry_points=[CommandHandler('start', start)],
        states={
            ACTIVITY: [MessageHandler(Filters.text & ~Filters.command, activity)],
            ENVIRONMENT: [MessageHandler(Filters.text & ~Filters.command, environment)],
            EXPERIENCE: [MessageHandler(Filters.text & ~Filters.command, experience)],
            PRIORITY: [MessageHandler(Filters.text & ~Filters.command, priority)],
        },
        fallbacks=[CommandHandler('cancel', cancel)],
    )

    dp.add_handler(conv_handler)

    # 7. 봇 시작
    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()
