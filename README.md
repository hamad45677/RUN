import pygame
import sys
import random

# تهيئة Pygame
pygame.init()

# إعدادات الشاشة
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("لعبة المغامرة")

# الألوان
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# اللاعب
player_image = pygame.Surface((50, 50))
player_image.fill(RED)
player_rect = player_image.get_rect(center=(SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2))
player_speed = 5

# الوحوش
num_monsters = 5
monster_list = []
for _ in range(num_monsters):
    monster_image = pygame.Surface((30, 30))
    monster_image.fill(GREEN)
    monster_rect = monster_image.get_rect(center=(random.randint(0, SCREEN_WIDTH), random.randint(0, SCREEN_HEIGHT)))
    monster_list.append(monster_rect)

# معالجة الأحداث
def handle_events():
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

# تحديث اللعبة
def update():
    global player_rect

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_rect.x -= player_speed
    if keys[pygame.K_RIGHT]:
        player_rect.x += player_speed
    if keys[pygame.K_UP]:
        player_rect.y -= player_speed
    if keys[pygame.K_DOWN]:
        player_rect.y += player_speed

    # التصادم مع الوحوش
    for monster in monster_list:
        if player_rect.colliderect(monster):
            if pygame.mouse.get_pressed()[0]:  # عند النقر الأيسر
                monster_list.remove(monster)

# رسم اللعبة
def draw():
    screen.fill(WHITE)
    screen.blit(player_image, player_rect)
    for monster in monster_list:
        screen.blit(monster_image, monster)
    pygame.display.flip()

# الحلقة الرئيسية للعبة
def main():
    clock = pygame.time.Clock()
    while True:
        handle_events()
        update()
        draw()
        clock.tick(30)

if __name__ == "__main__":
    main()

