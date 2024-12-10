import pygame
import time
import random

# Inisialisasi Pygame
pygame.init()

# Warna
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (213, 50, 80)
GREEN = (0, 255, 0)
BLUE = (50, 153, 213)

# Dimensi layar
WIDTH, HEIGHT = 600, 400
CELL_SIZE = 20

# Layar game
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Snake Game '97")

# Kecepatan snake
clock = pygame.time.Clock()
SPEED = 10

# Font
font_style = pygame.font.SysFont("bahnschrift", 25)
score_font = pygame.font.SysFont("comicsansms", 35)

# Fungsi menampilkan skor
def show_score(score):
    value = score_font.render(f"Score: {score}", True, GREEN)
    screen.blit(value, [10, 10])

# Fungsi menampilkan pesan
def message(msg, color):
    mesg = font_style.render(msg, True, color)
    screen.blit(mesg, [WIDTH / 6, HEIGHT / 3])

# Main game loop
def game_loop():
    game_over = False
    game_close = False

    x, y = WIDTH // 2, HEIGHT // 2
    dx, dy = 0, 0

    snake = []
    snake_length = 1

    food_x = round(random.randrange(0, WIDTH - CELL_SIZE) / CELL_SIZE) * CELL_SIZE
    food_y = round(random.randrange(0, HEIGHT - CELL_SIZE) / CELL_SIZE) * CELL_SIZE

    while not game_over:
        while game_close:
            screen.fill(BLACK)
            message("Game Over! Press C to Continue or Q to Quit", RED)
            show_score(snake_length - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        game_loop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT and dx == 0:
                    dx = -CELL_SIZE
                    dy = 0
                elif event.key == pygame.K_RIGHT and dx == 0:
                    dx = CELL_SIZE
                    dy = 0
                elif event.key == pygame.K_UP and dy == 0:
                    dx = 0
                    dy = -CELL_SIZE
                elif event.key == pygame.K_DOWN and dy == 0:
                    dx = 0
                    dy = CELL_SIZE

        if x >= WIDTH or x < 0 or y >= HEIGHT or y < 0:
            game_close = True

        x += dx
        y += dy
        screen.fill(BLACK)
        pygame.draw.rect(screen, GREEN, [food_x, food_y, CELL_SIZE, CELL_SIZE])

        snake_head = [x, y]
        snake.append(snake_head)
        if len(snake) > snake_length:
            del snake[0]

        for segment in snake[:-1]:
            if segment == snake_head:
                game_close = True

        for segment in snake:
            pygame.draw.rect(screen, BLUE, [segment[0], segment[1], CELL_SIZE, CELL_SIZE])

        show_score(snake_length - 1)
        pygame.display.update()

        if x == food_x and y == food_y:
            food_x = round(random.randrange(0, WIDTH - CELL_SIZE) / CELL_SIZE) * CELL_SIZE
            food_y = round(random.randrange(0, HEIGHT - CELL_SIZE) / CELL_SIZE) * CELL_SIZE
            snake_length += 1

        clock.tick(SPEED)

    pygame.quit()
    quit()

# Jalankan game
game_loop()
# snake-game
