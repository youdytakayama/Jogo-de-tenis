Vamos criar uma versão simplificada do jogo, onde dois jogadores controlam suas raquetes para rebater a bola de um lado para o outro.

Passos para criar o jogo de tênis:
Configuração Inicial: Configurar a janela do jogo e o loop principal.
Criar as Raquetes: Implementar as raquetes que os jogadores podem controlar.
Criar a Bola: Implementar a bola com física básica.
Adicionar Pontuação: Implementar um sistema de pontuação.
Detectar Colisões: Implementar a detecção de colisões entre a bola e as raquetes.
Código Inicial:
Vamos começar com a configuração básica do jogo, incluindo as raquetes e a bola.

python
Copiar código
import pygame
import random

# Inicialização do Pygame
pygame.init()

# Configurações da tela
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Jogo de Tênis")

# Cores
white = (255, 255, 255)
black = (0, 0, 0)
blue = (0, 0, 255)
red = (255, 0, 0)

# Classes
class Paddle(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.Surface([10, 100])
        self.image.fill(white)
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.speed_y = 0

    def update(self):
        self.rect.y += self.speed_y
        if self.rect.top < 0:
            self.rect.top = 0
        if self.rect.bottom > screen_height:
            self.rect.bottom = screen_height

    def move_up(self):
        self.speed_y = -6

    def move_down(self):
        self.speed_y = 6

    def stop(self):
        self.speed_y = 0

class Ball(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([20, 20])
        self.image.fill(white)
        self.rect = self.image.get_rect()
        self.rect.x = screen_width // 2
        self.rect.y = screen_height // 2
        self.speed_x = 5 * random.choice([1, -1])
        self.speed_y = 5 * random.choice([1, -1])

    def update(self):
        self.rect.x += self.speed_x
        self.rect.y += self.speed_y
        if self.rect.top < 0 or self.rect.bottom > screen_height:
            self.speed_y = -self.speed_y

# Grupos de sprites
all_sprites = pygame.sprite.Group()
paddles = pygame.sprite.Group()

# Criação das raquetes
paddle_left = Paddle(50, screen_height // 2 - 50)
paddle_right = Paddle(screen_width - 60, screen_height // 2 - 50)
all_sprites.add(paddle_left, paddle_right)
paddles.add(paddle_left, paddle_right)

# Criação da bola
ball = Ball()
all_sprites.add(ball)

# Variáveis de pontuação
score_left = 0
score_right = 0

# Fonte de texto
font = pygame.font.Font(None, 74)

# Loop principal
running = True
clock = pygame.time.Clock()

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_w:
                paddle_left.move_up()
            if event.key == pygame.K_s:
                paddle_left.move_down()
            if event.key == pygame.K_UP:
                paddle_right.move_up()
            if event.key == pygame.K_DOWN:
                paddle_right.move_down()
        if event.type == pygame.KEYUP:
            if event.key == pygame.K_w or event.key == pygame.K_s:
                paddle_left.stop()
            if event.key == pygame.K_UP or event.key == pygame.K_DOWN:
                paddle_right.stop()

    # Atualização dos sprites
    all_sprites.update()

    # Verificar colisões entre a bola e as raquetes
    if pygame.sprite.spritecollide(ball, paddles, False):
        ball.speed_x = -ball.speed_x

    # Verificar se a bola saiu dos limites e atualizar pontuação
    if ball.rect.left < 0:
        score_right += 1
        ball.rect.x = screen_width // 2
        ball.rect.y = screen_height // 2
        ball.speed_x = -ball.speed_x
        ball.speed_y = random.choice([5, -5])
    if ball.rect.right > screen_width:
        score_left += 1
        ball.rect.x = screen_width // 2
        ball.rect.y = screen_height // 2
        ball.speed_x = -ball.speed_x
        ball.speed_y = random.choice([5, -5])

    # Desenhar tudo
    screen.fill(black)
    all_sprites.draw(screen)

    # Desenhar pontuação
    score_text_left = font.render(str(score_left), True, white)
    screen.blit(score_text_left, (screen_width // 4, 10))
    score_text_right = font.render(str(score_right), True, white)
    screen.blit(score_text_right, (screen_width * 3 // 4, 10))

    # Atualizar a tela
    pygame.display.flip()

    # Controlar FPS
    clock.tick(60)

pygame.quit()

Este código cria uma janela de jogo simples com duas raquetes que podem ser controladas pelos jogadores (W/S para a raquete esquerda e as setas para a raquete direita) e uma bola que se move e rebate nas bordas da tela e nas raquetes. Ele também implementa um sistema básico de pontuação.

Para transformar isso em um jogo completo de tênis profissional, você pode adicionar funcionalidades como:

Melhorar a física da bola
Implementar diferentes tipos de jogadas e efeitos na bola
Adicionar sons e efeitos visuais
Implementar uma interface de menu e de pausa
Adicionar níveis de dificuldade e IA para um jogador contra o computador
Esses elementos podem ser adicionados passo a passo, expandindo a lógica e os sprites conforme necessário.
