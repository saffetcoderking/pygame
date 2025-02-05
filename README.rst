import pygame
import random
import math

# Initialize Pygame
pygame.init()

# Screen settings
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Hide from the Monster")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (200, 0, 0)
GREEN = (0, 200, 0)

# Player settings
player_size = 40
player_x = WIDTH // 2
player_y = HEIGHT // 2
player_speed = 4

# Monster settings
monster_size = 50
monster_x = random.randint(0, WIDTH - monster_size)
monster_y = random.randint(0, HEIGHT - monster_size)
monster_speed = 2

# Hiding spot (safe zone)
hide_x, hide_y = random.randint(100, WIDTH - 100), random.randint(100, HEIGHT - 100)
hide_size = 60

# Game loop control
running = True
hidden = False  # Whether the player is hiding

# Main game loop
while running:
    pygame.time.delay(30)
    
    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Player movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]: player_x -= player_speed
    if keys[pygame.K_RIGHT]: player_x += player_speed
    if keys[pygame.K_UP]: player_y -= player_speed
    if keys[pygame.K_DOWN]: player_y += player_speed

    # Boundaries
    player_x = max(0, min(WIDTH - player_size, player_x))
    player_y = max(0, min(HEIGHT - player_size, player_y))

    # Check if player is hiding
    if (hide_x < player_x < hide_x + hide_size) and (hide_y < player_y < hide_y + hide_size):
        hidden = True
    else:
        hidden = False

    # Monster AI (chase if not hiding)
    if not hidden:
        direction_x = player_x - monster_x
        direction_y = player_y - monster_y
        distance = math.sqrt(direction_x**2 + direction_y**2)

        if distance > 0:
            monster_x += (direction_x / distance) * monster_speed
            monster_y += (direction_y / distance) * monster_speed

    # Check if monster catches player
    if not hidden and abs(player_x - monster_x) < player_size and abs(player_y - monster_y) < player_size:
        print("You got caught! Game Over!")
        running = False

    # Drawing elements
    screen.fill(BLACK)  # Dark background
    pygame.draw.rect(screen, GREEN, (hide_x, hide_y, hide_size, hide_size))  # Hiding spot
    pygame.draw.rect(screen, WHITE, (player_x, player_y, player_size, player_size))  # Player
    pygame.draw.rect(screen, RED, (monster_x, monster_y, monster_size, monster_size))  # Monster

    pygame.display.update()

pygame.quit() 
