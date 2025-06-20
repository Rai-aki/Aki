import pygame
import random

# Initialize Pygame
pygame.init()

# Set up some constants
WIDTH, HEIGHT = 800, 600
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLACK = (0, 0, 0)

# Set up the display
screen = pygame.display.set_mode((WIDTH, HEIGHT))

# Set up the font
font = pygame.font.Font(None, 36)

# Set up the zombie class
class Zombie:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.health = 100

    def draw(self):
        pygame.draw.rect(screen, RED, (self.x, self.y, 50, 50))
        health_text = font.render(str(self.health), True, BLACK)
        screen.blit(health_text, (self.x, self.y - 20))

    def is_clicked(self, mouse_pos):
        return (self.x < mouse_pos[0] < self.x + 50 and
                self.y < mouse_pos[1] < self.y + 50)

# Set up the player
class Player:
    def __init__(self):
        self.damage = 50

# Set up the game
player = Player()
zombies = [Zombie(random.randint(0, WIDTH - 50), random.randint(0, HEIGHT - 50)) for _ in range(10)]

# Game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.MOUSEBUTTONDOWN:
            for zombie in zombies:
                if zombie.is_clicked(event.pos):
                    zombie.health -= player.damage
                    if zombie.health <= 0:
                        zombies.remove(zombie)
                        zombies.append(Zombie(random.randint(0, WIDTH - 50), random.randint(0, HEIGHT - 50)))

    # Draw everything
    screen.fill(WHITE)
    pygame.draw.rect(screen, (0, 255, 0), (WIDTH // 2 - 50, HEIGHT // 2 - 50, 100, 100))  # House
    for zombie in zombies:
        zombie.draw()

    # Update the display
    pygame.display.flip()

# Quit Pygame
pygame.quit()
