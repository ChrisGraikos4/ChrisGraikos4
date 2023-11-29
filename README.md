import pygame
import sys

# Αρχικοποίηση Pygame
pygame.init()

# Ορισμός διαστάσεων παραθύρου
width, height = 800, 600
win = pygame.display.set_mode((width, height))
pygame.display.set_caption("Ποδοσφαιρικό Παιχνίδι")

# Χρώματα
white = (255, 255, 255)
black = (0, 0, 0)

# Παίκτης
player_width, player_height = 50, 50
player_x = width // 2 - player_width // 2
player_y = height - player_height - 10

# Κίνηση παίκτη
player_speed = 5

# Κεντρικός βράχος
rock_width, rock_height = 50, 50
rock_x = width // 2 - rock_width // 2
rock_y = 0
rock_speed = 3

# Βαθμολογία
score = 0
font = pygame.font.Font(None, 36)

# Κύκλος παιχνιδιού
clock = pygame.time.Clock()

# Κεντρικός βράχος
def draw_rock(x, y):
    pygame.draw.rect(win, white, [x, y, rock_width, rock_height])

# Παίκτης
def draw_player(x, y):
    pygame.draw.rect(win, white, [x, y, player_width, player_height])

# Βαθμολογία
def show_score(score):
    score_text = font.render("Βαθμοί: " + str(score), True, white)
    win.blit(score_text, [10, 10])

# Κύκλος παιχνιδιού
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < width - player_width:
        player_x += player_speed

    # Κίνηση βράχου προς τα κάτω
    rock_y += rock_speed
    if rock_y > height:
        rock_y = 0
        rock_x = width // 2 - rock_width // 2
        score += 1

    # Έλεγχος σύγκρουσης
    if (
        player_x < rock_x + rock_width
        and player_x + player_width > rock_x
        and player_y < rock_y + rock_height
        and player_y + player_height > rock_y
    ):
        print("Game Over!")
        running = False

    # Εμφάνιση γραφικών
    win.fill(black)
    draw_player(player_x, player_y)
    draw_rock(rock_x, rock_y)
    show_score(score)

    pygame.display.update()

    # Ρυθμός ανανέωσης παραθύρου
    clock.tick(60)

# Κλείσιμο Pygame
pygame.quit()
sys.exit()
