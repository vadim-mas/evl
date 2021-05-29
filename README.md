# evl

from pygame import *
import random

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_energy, player_days):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (55, 55))
        self.speed = player_energy
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
        self.days = player_days

    def reset(self):
        background.blit(self.image, (self.rect.x, self.rect.y))

class Animal(GameSprite):
    def __init__(self, player_image, player_x, player_y, player_energy, player_days):
        super().__init__(self, player_image, player_x, player_y, player_energy, player_days)
        self.image = transform.scale(image.load('sprite1.png'), (55, 55))
        self.energy = random.randint(60, 100)
        self.days = random.randrange(300, 1000)
        self.x = random.randint(1, 20)
        self.y = random.randint(1, 20)

    def die(self):
        if self.energy <= 0:
            self.alive = False

    def live(self):
        self.alive = True

    def moving(self):
        self.x = self.x + int(random.randint(-1, 1))
        self.y = self.y + int(random.randint(-1, 1))

    def nutrition(self):
        if sprite.collide_rect(animals, mangos):
            self.energy += 20

    def reproduction(self):
        if self.energy >= 75:
            animals.add(animal)
            self.energy -= 25

    def update(self):
        self.days -= 1
        self.x = random.randint(1, 20)
        self.y = random.randint(1, 20)
        while game.grid[rand_n].free != True:
            rand_n = random.randint(0, game.grid.__len__)
        self.node = game.grid[rand_n]
        self.alive = True

animal = Animal()

class Mango(GameSprite):
    def __init__(self, player_image, player_x, player_y, player_speed):
        super().__init__(player_image, player_x, player_y, player_speed)
        super().__init__(player_image, player_x, player_y, player_speed)
        self.image = transform.scale(image.load('mango.jpg'), (55, 55))

        self.x = random.randint(1, 20)
        self.y = random.randint(1, 20)

mango = Mango()

win_width = 700
win_height = 500
background = transform.scale(image.load("background.jpg"), (win_width, win_height))
display.set_caption("Evolution")

finish = False
game = True
clock = time.Clock()
FPS = 60

animals = sprite.Group()
mangos = sprite.Group()

running = True
while running:
    clock.tick(FPS)
    for e in event.get():
        if e.type == QUIT:
            running = False
            if finish != True:
                background.blit(background, (0, 0))
                Animal.moving()
                Animal.reproduction()
                for i in range(5, 25):
                    mangos.add(mango)

display.update()
clock.tick(FPS)
time.delay(50)
