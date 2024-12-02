## Hi there 👋

<!--
**Roxmeena/Roxmeena** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
import pygame
import random

# Initialize pygame
pygame.init()

# Set up the game window
width, height = 400, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption('Flappy Bird Clone')

# Define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# Define the bird
bird_width = 40
bird_height = 40
bird_x = 50
bird_y = height // 2
bird_velocity = 0
gravity = 0.5
jump_strength = -10

# Define the pipes
pipe_width = 60
pipe_gap = 150
pipe_velocity = 4
pipes = []

# Score
score = 0
font = pygame.font.SysFont('Arial', 32)

# Game loop
running = True
clock = pygame.time.Clock()

while running:
    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE:
            bird_velocity = jump_strength

    # Update bird position
    bird_velocity += gravity
    bird_y += bird_velocity

    # Create new pipes
    if len(pipes) == 0 or pipes[-1][0] < width - 200:
        pipe_height = random.randint(100, height - pipe_gap - 100)
        pipes.append([width, pipe_height])

    # Move pipes
    for pipe in pipes:
        pipe[0] -= pipe_velocity

    # Remove pipes that are out of the screen
    pipes = [pipe for pipe in pipes if pipe[0] > -pipe_width]

    # Check for collisions with pipes or ground
    for pipe in pipes:
        if bird_x + bird_width > pipe[0] and bird_x < pipe[0] + pipe_width:
            if bird_y < pipe[1] or bird_y + bird_height > pipe[1] + pipe_gap:
                running = False

    if bird_y + bird_height > height or bird_y < 0:
        running = False

    # Increment score
    for pipe in pipes:
        if pipe[0] + pipe_width < bird_x and not pipe.get('scored', False):
            score += 1
            pipe['scored'] = True

    # Draw everything
    screen.fill(BLUE)

    # Draw pipes
    for pipe in pipes:
        pygame.draw.rect(screen, GREEN, (pipe[0], 0, pipe_width, pipe[1]))
        pygame.draw.rect(screen, GREEN, (pipe[0], pipe[1] + pipe_gap, pipe_width, height - pipe[1] - pipe_gap))

    # Draw bird
    pygame.draw.rect(screen, BLACK, (bird_x, bird_y, bird_width, bird_height))

    # Draw score
    score_text = font.render(f'Score: {score}', True, WHITE)
    screen.blit(score_text, (10, 10))

    # Update the display
    pygame.display.flip()

    # Set the frame rate
    clock.tick(60)

# Quit the game
pygame.quit(click home)
