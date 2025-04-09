# game
import pygame
import random

pygame.init()

# Розміри
WIDTH, HEIGHT = 600, 400
CELL_SIZE = 20
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Snake Game")

# Кольори
BLACK = (0, 0, 0)
GREEN = (0, 200, 0)
RED = (255, 0, 0)
WHITE = (255, 255, 255)

# Шрифт
font = pygame.font.SysFont("Arial", 30)

# Змійка
snake = [(100, 100)]
dx, dy = CELL_SIZE, 0

# Їжа
food = (random.randrange(0, WIDTH, CELL_SIZE),
        random.randrange(0, HEIGHT, CELL_SIZE))

# Швидкість
clock = pygame.time.Clock()
score = 0

game_over = False

# Головний цикл
running = True
while running:
    screen.fill(BLACK)

  for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

   keys = pygame.key.get_pressed()
    if not game_over:
        if keys[pygame.K_LEFT] and dx == 0:
            dx, dy = -CELL_SIZE, 0
        elif keys[pygame.K_RIGHT] and dx == 0:
            dx, dy = CELL_SIZE, 0
        elif keys[pygame.K_UP] and dy == 0:
            dx, dy = 0, -CELL_SIZE
        elif keys[pygame.K_DOWN] and dy == 0:
            dx, dy = 0, CELL_SIZE

   # Рух змійки
   head = (snake[0][0] + dx, snake[0][1] + dy)
        snake.insert(0, head)

   # Перевірка на їжу
  if head == food:
            score =+1
            food = (random.randrange(0, WIDTH, CELL_SIZE),
                    random.randrange(0, HEIGHT, CELL_SIZE))
        else:
            snake.pop()

   # Умова програшу
  if (head in snake[1:] or
                head[0] < 0 or head[1] < 0 or
                head[0] >= WIDTH or head[1] >= HEIGHT):
            game_over = True

  # Малювання
  for part in snake:
            pygame.draw.rect(screen, GREEN, (*part, CELL_SIZE, CELL_SIZE))
        pygame.draw.rect(screen, RED, (*food, CELL_SIZE, CELL_SIZE))
        # Вивести рахунок
        score_text = font.render(f"Score: {score}", True, WHITE)
        screen.blit(score_text, (10, 10))

   else:
        text = font.render("Game Over! Press R to restart", True, WHITE)
        screen.blit(text, (WIDTH // 2 - text.get_width() // 2, HEIGHT // 2))
        if keys[pygame.K_r]:
            snake = [(100, 100)]
            dx, dy = CELL_SIZE, 0
            food = (random.randrange(0, WIDTH, CELL_SIZE),
                    random.randrange(0, HEIGHT, CELL_SIZE))
            game_over = False

   pygame.display.update()
    clock.tick(10)
