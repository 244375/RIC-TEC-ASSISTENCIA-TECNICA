import pygame
import random

# Configurações do jogo
WIDTH, HEIGHT = 500, 600
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
MARIO_SIZE = (40, 40)
GRAVITY = 0.5
JUMP_STRENGTH = -10

# Inicializa o Pygame
pygame.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Mario Jump")
clock = pygame.time.Clock()

# Carregar imagens
mario_img = pygame.image.load("mario.png")
mario_img = pygame.transform.scale(mario_img, MARIO_SIZE)

# Classe do Mario
class Mario:
    def __init__(self):
        self.rect = pygame.Rect(WIDTH//2, HEIGHT//2, *MARIO_SIZE)
        self.velocity_y = 0

    def jump(self):
        self.velocity_y = JUMP_STRENGTH

    def update(self):
        self.velocity_y += GRAVITY
        self.rect.y += self.velocity_y
        if self.rect.y > HEIGHT:
            self.rect.y = HEIGHT//2
            self.velocity_y = 0

    def draw(self):
        screen.blit(mario_img, (self.rect.x, self.rect.y))

# Classe das plataformas
class Platform:
    def __init__(self, x, y, width=100, height=10):
        self.rect = pygame.Rect(x, y, width, height)

    def draw(self):
        pygame.draw.rect(screen, BLUE, self.rect)

# Criando objetos
mario = Mario()
platforms = [Platform(random.randint(0, WIDTH - 100), random.randint(100, HEIGHT - 20)) for _ in range(5)]

# Loop principal do jogo
running = True
while running:
    screen.fill(WHITE)
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                mario.jump()
    
    mario.update()
    mario.draw()
    
    for platform in platforms:
        platform.draw()
        if mario.rect.colliderect(platform.rect) and mario.velocity_y > 0:
            mario.jump()
    
    pygame.display.flip()
    clock.tick(30)

pygame.quit()
