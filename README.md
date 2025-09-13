# pin-pong-tf2-edition
гра була створена на мемах онлайн гри від розробників valve team fortrest 2

<img width="301" height="169" alt="image" src="https://github.com/user-attachments/assets/3bef487c-8f73-48f2-9dcb-0d5529fec7f9" />

# 🎮 Онлайн Пінг-Понг (Python + Pygame)

- 🧍 Гравці — це Heavy з TF2
- 🔊 Усі звуки — це справжні (і дуже смішні) voice lines з гри
- 🧠 Пауза активується легендарним: "Think fast, chucklenuts!"
- 💀 Поразка? Під сміх Heavy!
- 🎵 Фонова музика — з TF2, щоб був справжній vibe апгрейд станції

Цей проєкт — **мемна адаптація класичного пінг-понгу**, створена для фану і ностальгії по TF2.


<img width="450" height="250" alt="image" src="https://github.com/user-attachments/assets/a95c2607-ac98-4865-820d-3649becfd83d" />


Це клієнтська частина багатокористувацької гри **Пінг-Понг**, створеної з використанням **Pygame** та **socket**. Ви граєте за персонажів у стилі TF2, керуючи ракетними важковаговиками на арені — з фоновою музикою та фановими звуками!

---

## 📦 Вміст

- ## 🔧 Налаштування

Ініціалізується Pygame, мікшер для звуків і вікно гри з розміром 800x600:

```python
from pygame import *
import socket
import json
from threading import Thread

WIDTH, HEIGHT = 800, 600
init()
mixer.init()
screen = display.set_mode((WIDTH, HEIGHT))
clock = time.Clock()
display.set_caption("Пінг-Понг")
- [🖼️ Графіка](#-графіка)
- paddle_img_1 = image.load("red_huavy.png").convert_alpha()
paddle_img_2 = image.load("blu_heavy.png").convert_alpha()
ball_img = image.load("dispancerpng.png").convert_alpha()
pause_overlay = image.load("thinkfast.png").convert_alpha()
background = image.load("bg.png").convert()

# Масштабування під розміри гри
paddle_img_1 = transform.scale(paddle_img_1, (20, 100))
paddle_img_2 = transform.scale(paddle_img_2, (20, 100))
ball_img = transform.scale(ball_img, (20, 20))
pause_overlay = transform.scale(pause_overlay, (WIDTH, HEIGHT))
background = transform.scale(background, (WIDTH, HEIGHT))
- [🔊 Звуки](#-звуки)
try:
    background_sound = mixer.Sound("Voicy_Upgrade Station.mp3")
    score_sound = mixer.Sound("Voicy_Sniper says _No!_.mp3")
    effect_sound = mixer.Sound("Voicy_think fast chuckelnuts.mp3")
    hit_sound = mixer.Sound("Voicy_Pootis.mp3")
    lose_sound = mixer.Sound("Voicy_laugh.mp3")
except Exception as e:
    print(f"Помилка при завантаженні звуків: {e}")

# Налаштування гучності
background_sound.set_volume(1.0)
score_sound.set_volume(1.0)
effect_sound.set_volume(1.0)
hit_sound.set_volume(1.0)
lose_sound.set_volume(1.0)

background_sound.play(-1)  # Безкінечне відтворення фонової мелодії
- [🌐 Мережа (Socket)](#-мережа-socket)
def connect_to_server():
    while True:
        try:
            client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            client.connect(('localhost', 8080))
            buffer = ""
            game_state = {}
            my_id = int(client.recv(24).decode())
            return my_id, game_state, buffer, client
        except:
            pass

def receive():
    global buffer, game_state, game_over
    while not game_over:
        try:
            data = client.recv(1024).decode()
            buffer += data
            while "\n" in buffer:
                packet, buffer = buffer.split("\n", 1)
                if packet.strip():
                    game_state = json.loads(packet)
        except:
            game_state["winner"] = -1
            break
- [🎮 Ігровий цикл](#-ігровий-цикл)
while True:
    for e in event.get():
        if e.type == QUIT:
            exit()
        if e.type == KEYDOWN:
            if e.key == K_y and not paused:
                effect_sound.play()
                effect_start_time = time.get_ticks()
                paused = True
                mixer.music.pause()

    if paused:
        now = time.get_ticks()
        elapsed = now - effect_start_time if effect_start_time else 0
        if 2000 <= elapsed <= 5000:
            screen.fill((255, 255, 255))
        else:
            screen.blit(pause_overlay, (0, 0))
        display.update()
        if not mixer.get_busy():
            paused = False
            mixer.music.unpause()
        continue
- [🎯 Управління](#-управління)
Клавіша	Дія
W	Рух ракетки вгору
S	Рух ракетки вниз
Y	"Пауза" з мемним ефектом
K	Рестарт після виграшу
- [📁 Структура проекту](#-структура-проекту)
pong-game/
├── client.py                # Основний клієнтський код гри
├── red_huavy.png            # Paddle для Player 1
├── blu_heavy.png            # Paddle для Player 2
├── dispancerpng.png         # М'яч
├── thinkfast.png            # Зображення для паузи
├── bg.png                   # Фон
├── Voicy_*.mp3              # Звуки з TF2
└── README.md                # Цей файл
- [🚀 Запуск](#-запуск)
Встанови залежності:

pip install pygame

Переконайся, що сервер запущено (server.py або інший).

Запусти клієнтську гру:

python client.py

Скріншоти з гри:

<img width="803" height="598" alt="image" src="https://github.com/user-attachments/assets/b6bd17d1-563e-4159-acf8-67340d3c6276" />


<img width="802" height="596" alt="image" src="https://github.com/user-attachments/assets/b2a77124-8afe-4519-91c1-af6be2417174" />



---
