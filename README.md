from pyrogram import Client
import schedule
import time

api_id = 'Ваш айди'
api_hash = 'Ваш хеш'
message = ("Ваш текст")

def send_messages():
    print("Starting send_messages")
    with Client('my_new_account', api_id, api_hash) as app:
        try:
            with open('chats.txt', 'r') as f:
                links = [line.strip() for line in f]
        except Exception as e:
            print(f"Failed to read chat links. Error: {e}")
            return
        for link in links:
            username = link.split('/')[-1]  # извлекаем username из ссылки
            try:
                app.send_message(username, message)
                print(f"Message sent to chat {username}")
            except Exception as e:
                print(f"Failed to send message to chat {link}. Error: {e}")
    print("Finished send_messages")

send_messages() # Call the function immediately
schedule.every(10).minutes.do(send_messages) # Then schedule it every 10 minutes

while True:
    schedule.run_pending()
    time.sleep(60)
