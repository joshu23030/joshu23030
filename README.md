import pygame
import sys
import random

# Inicializar Pygame
pygame.init()

# Configuraciones del juego
WIDTH, HEIGHT = 600, 400
FPS = 60
GRAVITY = 0.25
JUMP_HEIGHT = -5

# Colores
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Crear la pantalla
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Flappy Bird - VAMPIR")

# Cargar imágenes
bird_img = pygame.image.load("bird.png")  # Asegúrate de tener una imagen llamada 'bird.png'
background_img = pygame.image.load("background.png")  # Asegúrate de tener una imagen llamada 'background.png'

# Escalar la imagen del pájaro
bird_img = pygame.transform.scale(bird_img, (50, 50))

# Reloj para controlar la velocidad del juego
clock = pygame.time.Clock()

# Función para mostrar el mensaje "VAMPIR"
def show_vampir_message():
    font = pygame.font.Font(None, 36)
    text = font.render("VAMPIR", True, RED)
    text_rect = text.get_rect(center=(WIDTH // 2, HEIGHT // 2))
    screen.blit(text, text_rect)
    pygame.display.flip()
    pygame.time.wait(2000)  # Esperar 2 segundos antes de salir
    pygame.quit()
    sys.exit()

# Clase Bird que representa al pájaro
class Bird(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = bird_img
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH // 4, HEIGHT // 2)
        self.velocity = 0

    def update(self):
        self.velocity += GRAVITY
        self.rect.y += self.velocity

# Grupo de sprites
all_sprites = pygame.sprite.Group()
bird = Bird()
all_sprites.add(bird)

# Bucle principal del juego
running = True
while running:
    # Eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bird.velocity = JUMP_HEIGHT

    # Actualizar
    all_sprites.update()

    # Verificar colisiones
    if bird.rect.bottom > HEIGHT:
        show_vampir_message()

    # Dibujar
    screen.blit(background_img, (0, 0))
    all_sprites.draw(screen)

    # Actualizar la pantalla
    pygame.display.flip()

    # Establecer FPS
    clock.tick(FPS)

# Salir del juego
pygame.quit()
sys.exit()
