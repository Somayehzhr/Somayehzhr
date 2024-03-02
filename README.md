import turtle
import time
import random

delay = 0.1
score = 0

# Create window
wn = turtle.Screen()
wn.title("Snake Game")
wn.bgcolor("black")
wn.setup(width=600, height=600)
wn.tracer(0)

# Create border
border = turtle.Turtle()
border.speed(0)
border.color("white")
border.penup()
border.hideturtle()
border.goto(-290, -290)
border.pendown()
for _ in range(4):
    border.forward(580)
    border.left(90)

# Create snake head
head = turtle.Turtle()
head.speed(0)
head.shape("square")
head.color("white")
head.penup()
head.goto(0, 0)
head.direction = "Right"

# Create fruit
fruit = turtle.Turtle()
fruit.speed(0)
fruit.shape("circle")
fruit.color("red")
fruit.penup()
fruit.goto(random.randint(-270, 270), random.randint(-270, 270))

# Create score display
score_display = turtle.Turtle()
score_display.speed(0)
score_display.color("white")
score_display.penup()
score_display.hideturtle()
score_display.goto(0, 260)
score_display.write("Score: {}".format(score), align="center", font=("Courier", 24, "normal"))

# Movement functions
def go_up():
    if head.direction != "Down":
        head.direction = "Up"

def go_down():
    if head.direction != "Up":
        head.direction = "Down"

def go_left():
    if head.direction != "Right":
        head.direction = "Left"

def go_right():
    if head.direction != "Left":
        head.direction = "Right"

# Set up key bindings
wn.listen()
wn.onkey(go_up, "w")
wn.onkey(go_down, "s")
wn.onkey(go_left, "a")
wn.onkey(go_right, "d")

# Main game loop
while True:
    wn.update()

    # Move the snake head
    if head.direction == "Up":
        head.sety(head.ycor() + 20)
    elif head.direction == "Down":
        head.sety(head.ycor() - 20)
    elif head.direction == "Left":
        head.setx(head.xcor() - 20)
    elif head.direction == "Right":
        head.setx(head.xcor() + 20)

    # Check for collision with the borders
    if head.xcor() > 290 or head.xcor() < -290 or head.ycor() > 290 or head.ycor() < -290:
        time.sleep(1)
        head.goto(0, 0)
        head.direction = "Right"

    # Check for collision with the fruit
    if (
        fruit.xcor() - 15 < head.xcor() < fruit.xcor() + 15
        and fruit.ycor() - 15 < head.ycor() < fruit.ycor() + 15
    ):
        score += 1
        score_display.clear()
        score_display.write("Score: {}".format(score), align="center", font=("Courier", 24, "normal"))
        fruit.goto(random.randint(-270, 270), random.randint(-270, 270))

    # Delay for smooth movement
    time.sleep(delay)
