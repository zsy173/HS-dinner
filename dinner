import os
import random
import requests
from flask import Flask, request, jsonify

app = Flask(__name__)

# 从环境变量读取飞书机器人 Webhook
WEBHOOK_URL = os.getenv("WEBHOOK_URL")

# 自定义20家店铺（可修改）
RESTAURANTS = [
    "麦当劳", "肯德基", "必胜客", "海底捞", "兰州拉面",
    "沙县小吃", "重庆小面", "桂林米粉", "黄焖鸡米饭", "麻辣香锅",
    "日式料理", "韩式烤肉", "意大利餐厅", "港式茶餐厅", "海南鸡饭",
    "披萨店", "汉堡王", "寿司店", "烧烤摊", "火锅店"
]

def send_reply(message):
    data = {
        "msg_type": "text",
        "content": {"text": message}
    }
    requests.post(WEBHOOK_URL, json=data)

@app.route("/", methods=["POST"])
def handle_request():
    data = request.json
    # 飞书验证请求
    if data.get("type") == "url_verification":
        return jsonify({"challenge": data.get("challenge")})
    
    # 如果消息包含"晚饭吃什么"
    if data.get("event", {}).get("text", "").find("晚饭吃什么") != -1:
        choice = random.choice(RESTAURANTS)
        send_reply(f"🍚 今日推荐：{choice}\n\n备选列表：{', '.join(RESTAURANTS)}")
    
    return jsonify({"success": True})

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=int(os.getenv("PORT", 5000)))
