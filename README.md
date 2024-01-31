import pygame
import time
import random

pygame.init()

# Set up display
width, height = 800, 600
display = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game")

# Colors
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)
green = (0, 255, 0)

# Snake properties
snake_block = 10
snake_speed = 15

# Initialize snake
snake = [(width / 2, height / 2)]
snake_direction = "RIGHT"
change_to = snake_direction

# Food properties
food_size = 10
food_position = [random.randrange(1, (width//food_size)) * food_size,
                 random.randrange(1, (height//food_size)) * food_size]

# Game over flag
game_over = False

# Function to draw the snake
def draw_snake(snake):
    for segment in snake:
        pygame.draw.rect(display, green, [segment[0], segment[1], snake_block, snake_block])

# Function to draw the food
def draw_food(food_position):
    pygame.draw.rect(display, red, [food_position[0], food_position[1], food_size, food_size])

# Main game loop
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP and not snake_direction == "DOWN":
                change_to = "UP"
            elif event.key == pygame.K_DOWN and not snake_direction == "UP":
                change_to = "DOWN"
            elif event.key == pygame.K_LEFT and not snake_direction == "RIGHT":
                change_to = "LEFT"
            elif event.key == pygame.K_RIGHT and not snake_direction == "LEFT":
                change_to = "RIGHT"

    # Update snake direction
    if change_to == "UP" and not snake_direction == "DOWN":
        snake_direction = "UP"
    elif change_to == "DOWN" and not snake_direction == "UP":
        snake_direction = "DOWN"
    elif change_to == "LEFT" and not snake_direction == "RIGHT":
        snake_direction = "LEFT"
    elif change_to == "RIGHT" and not snake_direction == "LEFT":
        snake_direction = "RIGHT"

    # Move the snake
    if snake_direction == "UP":
        snake[0] = (snake[0][0], snake[0][1] - snake_block)
    elif snake_direction == "DOWN":
        snake[0] = (snake[0][0], snake[0][1] + snake_block)
    elif snake_direction == "LEFT":
        snake[0] = (snake[0][0] - snake_block, snake[0][1])
    elif snake_direction == "RIGHT":
        snake[0] = (snake[0][0] + snake_block, snake[0][1])

    # Check for collisions with walls
    if snake[0][0] >= width or snake[0][0] < 0 or snake[0][1] >= height or snake[0][1] < 0:
        game_over = True

    # Check for collisions with self
    for segment in snake[1:]:
        if snake[0] == segment:
            game_over = True

    # Check for collision with food
    if snake[0][0] == food_position[0] and snake[0][1] == food_position[1]:
        food_position = [random.randrange(1, (width//food_size)) * food_size,
                         random.randrange(1, (height//food_size)) * food_size]
        snake.append((0, 0))  # Add a new segment to the snake

    # Update display
    display.fill(black)
    draw_snake(snake)
    draw_food(food_position)
    pygame.display.update()

    # Control snake speed
    pygame.time.Clock().tick(snake_speed)

# Quit the game
pygame.quit()
