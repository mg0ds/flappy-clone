import pygame
import os
import random
import math

pygame.init()
szer = 600
wys = 600
screen = pygame.display.set_mode((szer,wys))

def napisz(tekst, x, y, rozmiar):
    cz = pygame.font.SysFont("Arial", rozmiar)
    rend = cz.render(tekst, 1, (255,100,100))
    screen.blit(rend, (x,y))

copokazuje = "menu"

class przeszkoda():
    def __init__(self, x, szerokosc):
        self.x = x
        self.szerokosc = szerokosc
        self.y_gora = 0
        self.wys_gora = random.randint(100,250)
        self.odstep = random.randint(200,300)
        self.y_dol = self.wys_gora + self.odstep
        self.wys_dol = wys - self.y_dol
        self.kolor = (random.randint(150,170), random.randint(130,150), random.randint(180,200))
        self.ksztalt_gora = pygame.Rect(self.x, self.y_gora, self.szerokosc, self.wys_gora)
        self.ksztalt_dol = pygame.Rect(self.x, self.y_dol, self.szerokosc, self.wys_dol)

    def rysuj(self):
        pygame.draw.rect(screen, self.kolor, self.ksztalt_gora, 0)
        pygame.draw.rect(screen, self.kolor, self.ksztalt_dol, 0)

    def ruch(self, v):
        self.x = self.x - v
        self.ksztalt_gora = pygame.Rect(self.x, self.y_gora, self.szerokosc, self.wys_gora)
        self.ksztalt_dol = pygame.Rect(self.x, self.y_dol, self.szerokosc, self.wys_dol)

    def kolizja(self, player):
        if self.ksztalt_gora.colliderect(player) or self.ksztalt_dol.colliderect(player):
            return True
        else:
            return False

class Helikopter():
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.wysokosc = 30
        self.szerokosc = 80
        self.ksztalt = pygame.Rect(self.x, self.y, self.szerokosc, self.wysokosc)
        self.grafika = pygame.image.load(os.path.join("heli.png"))

    def rysuj(self):
        screen.blit(self.grafika, (self.x, self.y))

    def ruch(self, v):
        self.y = self.y + v
        self.ksztalt = pygame.Rect(self.x, self.y, self.szerokosc, self.wysokosc)

przeszkody = []
for i in range(21):
    przeszkody.append(przeszkoda(i*szer/20, szer/20))

gracz = Helikopter(250, 275)

dy = 0

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            quit()
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                dy = -0.1
            if event.key == pygame.K_DOWN:
                dy = 0.1
            if event.key == pygame.K_SPACE:
                if copokazuje != "rozgrywka":
                    gracz = Helikopter(250,275)
                    dy = 0
                    copokazuje = "rozgrywka"
                    punkty = 0
    screen.fill((0,0,0))
    if copokazuje == "menu":
        napisz("Nacisnij spacje", 80, 450, 20)
        grafika = pygame.image.load(os.path.join("helikopter.jpg"))
        screen.blit(grafika, (45,30))
    elif copokazuje == "rozgrywka":
        for p in przeszkody:
            p.ruch(0.1)
            p.rysuj()
            if p.kolizja(gracz.ksztalt):
                copokazuje = "koniec"
        for p in przeszkody:
            if p.x <= -p.szerokosc:
                przeszkody.remove(p)
                przeszkody.append((przeszkoda(szer, szer/20)))
                punkty = punkty + math.fabs(dy*10)
        gracz.rysuj()
        gracz.ruch(dy)
        napisz(str(punkty), 50, 50, 20)
    elif copokazuje == "koniec":
        grafika = pygame.image.load(os.path.join("helikopter.jpg"))
        screen.blit(grafika, (45, 30))
        napisz("Niestety przegrywasz", 80, 450, 20)
        napisz("Nacisnij spacje zeby zagrac jeszcze raz", 80, 480, 20)
        napisz("Twoj wynik to "+str(punkty), 80, 510, 20)


    pygame.display.update()
