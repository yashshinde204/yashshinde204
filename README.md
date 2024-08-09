import pygame
import random

# Initialize pygame
pygame.init()

# Screen dimensions
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Bubble Pop Game")

# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Bubble settings
BUBBLE_RADIUS = 30
BUBBLE_COUNT = 10

# Bubble class
class Bubble:
    def __init__(self):
        self.x = random.randint(BUBBLE_RADIUS, SCREEN_WIDTH - BUBBLE_RADIUS)
        self.y = random.randint(BUBBLE_RADIUS, SCREEN_HEIGHT - BUBBLE_RADIUS)
        self.speed = random.uniform(1, 3)
        self.radius = BUBBLE_RADIUS
        self.color = RED

    def move(self):
        self.y -= self.speed
        if self.y < -self.radius:
            self.y = SCREEN_HEIGHT + self.radius
            self.x = random.randint(BUBBLE_RADIUS, SCREEN_WIDTH - BUBBLE_RADIUS)
            self.speed = random.uniform(1, 3)

    def draw(self, screen):
        pygame.draw.circle(screen, self.color, (self.x, self.y), self.radius)

    def is_clicked(self, pos):
        return (self.x - pos[0])**2 + (self.y - pos[1])**2 <= self.radius**2

# Game loop
def game_loop():
    clock = pygame.time.Clock()
    bubbles = [Bubble() for _ in range(BUBBLE_COUNT)]
    score = 0
    font = pygame.font.Font(None, 36)

    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            elif event.type == pygame.MOUSEBUTTONDOWN:
                pos = pygame.mouse.get_pos()
                for bubble in bubbles[:]:
                    if bubble.is_clicked(pos):
                        bubbles.remove(bubble)
                        score += 1
                        break

        # Update
        for bubble in bubbles:
            bubble.move()

        # Draw everything
        screen.fill(WHITE)
        for bubble in bubbles:
            bubble.draw(screen)

        # Draw score
        score_text = font.render(f"Score: {score}", True, (0, 0, 0))
        screen.blit(score_text, (10, 10))

        pygame.display.flip()
        clock.tick(60)

    pygame.quit()

# Run the game
game_loop()

