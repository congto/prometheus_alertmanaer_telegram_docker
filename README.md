**Alertmanager + Telegram Integration**
Dockerized solution for sending Prometheus alerts to Telegram

🚀 Quick Start
============================================================

```
git clone https://github.com/congto/prom_alert_tele_docker.git

cd prom_alert_tele_docker
```



🔧Configuration
===========================================================

Sửa file  .env  với thông tin của bạn: 

```
BOT_TOKEN=your_bot_token  
CHAT_ID=your_chat_id  
```

Hoặc chỉ cần sửa trong file python chứa code của webhook 

Sửa webhook ở dòng `api_url` của slack ở file `alertmanager.yml` 

Build image 

```
docker compose up -d --build
```



"  # Путь к файлу с алертами
      
Auto-start (Linux)
============================================================

sudo cp alertmanager-telegramm.service /etc/systemd/system/

sudo systemctl enable --now alertmanager-telegramm.service

🛠️ Verification
============================================================
Send test alert:

Cách 1: Sử dụng lệnh để tắt bật rabbitmq 

Stop rabbitmq 
```
docker compose scale rabbitmq=0
```


Start rabbitmq 
```
docker compose scale rabbitmq=1
```


Cách 2: tets bằng curl 

curl -X POST http://localhost:9093/api/v2/alerts  -H 'Content-Type: application/json' -d '[{"labels":{"alertname":"TEST"}}]'


Check Telegram for notification

⚠️ Troubleshooting
============================================================

No Telegram alerts        -        **!!! Verify bot token/chat_id !!!** = .env it may work incorrectly

Alertmanager unreachable        -        Check Docker network connectivity

Service won't start        -        Inspect logs: 

```
journalctl -u alertmanager-telegramm.service 

#OR 

docker logs telegram-webhook 

#OR

docker logs alertmanager
```