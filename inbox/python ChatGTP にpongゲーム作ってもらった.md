---
type: note
---

#python 

---
2023-03-16  10:12

# python ChatGTP にpongゲーム作ってもらった

![[Pasted image 20230316101309.png]]

[[python 仮想環境の作り方]]

```shell
$ mkdir ~/Desktop/python
$ cd python
$ python -m venv .venv
$ source .venv/bin/activate
```

pong.py
```python
import pygame
import random

# ゲーム画面のサイズ
WIDTH = 800
HEIGHT = 600

# パドルのサイズ
PADDLE_WIDTH = 20
PADDLE_HEIGHT = 100

# ボールのサイズと速度
BALL_SIZE = 20
BALL_SPEED = 5

# 色
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)

# Pygameの初期化
pygame.init()

# ゲーム画面の作成
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Pong")

# パドルの初期位置
player_pos = [50, HEIGHT//2 - PADDLE_HEIGHT//2]
ai_pos = [WIDTH - 50 - PADDLE_WIDTH, HEIGHT//2 - PADDLE_HEIGHT//2]

# ボールの初期位置と速度
ball_pos = [WIDTH//2 - BALL_SIZE//2, HEIGHT//2 - BALL_SIZE//2]
ball_vel = [BALL_SPEED, BALL_SPEED]

# スコア
player_score = 0
ai_score = 0

# パドルを描画する関数
def draw_paddle(paddle):
    pygame.draw.rect(screen, WHITE, (paddle[0], paddle[1], PADDLE_WIDTH, PADDLE_HEIGHT))

# ボールを描画する関数
def draw_ball(ball):
    pygame.draw.rect(screen, WHITE, (ball[0], ball[1], BALL_SIZE, BALL_SIZE))

# ゲーム画面を更新する関数
def update_screen():
    screen.fill(BLACK)
    draw_paddle(player_pos)
    draw_paddle(ai_pos)
    draw_ball(ball_pos)
    pygame.display.update()

# ボールが壁に当たったときの処理
def ball_wall_collision():
    if ball_pos[1] <= 0 or ball_pos[1] >= HEIGHT - BALL_SIZE:
        ball_vel[1] = -ball_vel[1]

# ボールがパドルに当たったときの処理
def ball_paddle_collision():
    global ball_vel
    if ball_pos[0] <= player_pos[0] + PADDLE_WIDTH and ball_pos[1] >= player_pos[1] and ball_pos[1] <= player_pos[1] + PADDLE_HEIGHT:
        ball_vel[0] = -ball_vel[0]
    elif ball_pos[0] + BALL_SIZE >= ai_pos[0] and ball_pos[1] >= ai_pos[1] and ball_pos[1] <= ai_pos[1] + PADDLE_HEIGHT:
        ball_vel[0] = -ball_vel[0]

# ボールがゴールに入ったときの処理
def ball_goal():
    global player_score, ai_score, ball_pos, ball_vel
    if ball_pos[0] <= 0:
        ai_score += 1
        ball_pos = [WIDTH//2 - BALL_SIZE//2, HEIGHT//2 - BALL_SIZE//2]
        ball_vel = [random.choice([-BALL_SPEED, BALL_SPEED]), random.choice([-BALL_SPEED, BALL_SPEED])]
    elif ball_pos[0] + BALL_SIZE >= WIDTH:
        player_score += 1
        ball_pos = [WIDTH//2 - BALL_SIZE//2, HEIGHT//2 - BALL_SIZE//2]
        ball_vel = [random.choice([-BALL_SPEED, BALL_SPEED]), random.choice([-BALL_SPEED, BALL_SPEED])]

# AIの動きを決定する関数
def ai_move():
    if ball_vel[0] > 0:
        if ai_pos[1] + PADDLE_HEIGHT // 2 < ball_pos[1]:
            ai_pos[1] += BALL_SPEED
        elif ai_pos[1] + PADDLE_HEIGHT // 2 > ball_pos[1]:
            ai_pos[1] -= BALL_SPEED

# ゲームループ
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # パドルの移動
    keys = pygame.key.get_pressed()
    if keys[pygame.K_UP] and player_pos[1] > 0:
        player_pos[1] -= BALL_SPEED
    if keys[pygame.K_DOWN] and player_pos[1] < HEIGHT - PADDLE_HEIGHT:
        player_pos[1] += BALL_SPEED

    # ボールの移動
    ball_pos[0] += ball_vel[0]
    ball_pos[1] += ball_vel[1]

    # ボールの当たり判定
    ball_wall_collision()
    ball_paddle_collision()
    ball_goal()

    # AIの動き
    ai_move()

    # ゲーム画面の更新
    update_screen()

# Pygameの終了処理
pygame.quit()

```

```shell
> pip install pygame
> python pong.py
```

![[Pasted image 20230316101643.png]]

やべー打ち返してくる!!
ノー修正。

```shell
$ deactivate
```

python仮想環境はプロジェクトの中に作られるイメージ?
そのプロジェクトに入ったら、アクティベートでそのプロジェクトの仮想環境になってそこだけのPython環境世界にシフトするって感じか。
そこだけの実行環境。環境展開の仕掛けが .env に用意してあるということだね。


