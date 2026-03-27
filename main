import os
import vk_api
from vk_api.bot_longpoll import VkBotLongPoll, VkBotEventType
import google.generativeai as genai

# --- ПРОКСИ ДЛЯ БЕСПЛАТНОГО ТАРИФА ---
os.environ['http_proxy'] = "http://proxy.server:3128"
os.environ['https_proxy'] = "http://proxy.server:3128"

# --- ТВОИ НАСТРОЙКИ ---
VK_TOKEN = os.environ.get('VK_TOKEN')
GROUP_ID = 236440291
GEMINI_KEY = os.environ.get('GEMINI_KEY')
# ----------------------

# Настройка Gemini
genai.configure(api_key=GEMINI_KEY)
model = genai.GenerativeModel('gemini-1.5-flash')

# Настройка ВК
vk_session = vk_api.VkApi(token=VK_TOKEN)
vk = vk_session.get_api()
longpoll = VkBotLongPoll(vk_session, GROUP_ID)

print("ОлегоТест пройден. Мозги Gemini пришиты!")

for event in longpoll.listen():
    if event.type == VkBotEventType.MESSAGE_NEW and event.from_user:
        user_id = event.obj.message['from_id']
        text = event.obj.message['text']

        # Инструкция для бота (Промпт)
        prompt = f"Ты — эксперт из паблика JustieOff. Ты шаришь в железе, ненавидишь майнеры, шутишь про Александра с его 121 Вт на Ryzen 5600 и Матвея с его овсянкой. Отвечай кратко, с юмором и используй слово ОлегоТест. Вопрос пользователя: {text}"

        try:
            response = model.generate_content(prompt)
            final_text = response.text
        except:
            final_text = "Мозги перегрелись, видать Матвей кабель перегрыз."

        vk.messages.send(user_id=user_id, message=final_text, random_id=0)
