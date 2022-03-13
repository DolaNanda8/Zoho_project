# Zoho_projectfrom visual import *

# Makes the screen range between -5 and 5 in the x and y directions.
scene.range = 5
# Creates the bricks and the paddle.
ball = sphere(pos=(0,0,0), radius = .3, color=color.red)
brick = box(pos=(-4,4.5,0), length = 1.5, height = .1, width = 0, color=color.cyan)
brick2 = box(pos=(-2,4.5,0), length = 1.5, height = .1, width = 0, color=color.cyan)
brick3 = box(pos=(0,4.5,0), length = 1.5, height = .1, width = 0, color=color.cyan)
brick4= box(pos=(2,4.5,0), length = 1.5, height = .1, width = 0, color=color.cyan)
brick5 = box(pos=(4,4.5,0), length = 1.5, height = .1, width = 0, color=color.cyan)

paddle = box(pos=(0,-4.5,0), length = 1.5, height = .1, width = 0,color=color.blue)
dt = 0.01

# The following variables are used for the velocity and direction of the ball.
ballXV = 1
ballYV = 4
ballDirectionUp = 1
ballDirectionRight = 0

drag2_pos = None

# The defining functions are being used to grab, move, and drop the paddle.
# The parts that are commented out are so you can click on the mouse
# and not have to hold down the mouse button, but can still move the mouse.
def grab2(evt2):
    global drag2_pos
    if evt2.pick == paddle:
        drag2_pos = evt2.pickpos
        scene.bind('mousemove', move2, paddle)
## scene.bind(‘mouseup’, drop2)

def move2(evt2, obj2):
    global drag2_pos
    new2_pos = scene.mouse.project(normal=(0,0,1))
    if new2_pos != drag2_pos:
        obj2.pos += new2_pos - drag2_pos
        drag2_pos = new2_pos
        paddle.pos.x = drag2_pos.x
        paddle.pos.y = drag2_pos.y

##def drop2(evt2):
## scene.unbind(‘mousemove’,move2)
## scene.unbind(‘moveup’,drop2)

scene.bind('mousedown', grab2)

#Establish a velocity for the ball
ball.velocity = vector(ballXV,ballYV,0)

#Going to be used to make the bricks disappear.
brickBroke = True
brick2Broke = True
brick3Broke = True
brick4Broke = True
brick5Broke = True

# This while loop runs the game.
while 1:
    rate(100)
    # Gives movement to the ball
    ball.pos = ball.pos + ball.velocity*dt

    # If the ball hits a brick, then the brick color will be changed to black, brickBroke
    # will be changed to false, and the brick will be able to be passed through.
    if ball.pos.y > brick.pos.y and ball.pos.x >= -4.75 and ball.pos.x < -3.25 and brickBroke == True:
        ball.velocity = vector(ballXV * -1, ballYV * -1,0)
        brickBroke = False
        brick.color = color.black
    if ball.pos.y > brick2.pos.y and ball.pos.x >= -2.75 and ball.pos.x < -1.25 and brick2Broke == True:
        ball.velocity = vector(ballXV * -1, ballYV * -1,0)
        brick2Broke = False
        brick2.color = color.black
    if ball.pos.y > brick3.pos.y and ball.pos.x >= -0.75 and ball.pos.x < .75 and brick3Broke == True:
        ball.velocity = vector(ballXV * -1, ballYV * -1,0)
        brick3Broke = False
        brick3.color = color.black
    if ball.pos.y > brick4.pos.y and ball.pos.x >= 1.25 and ball.pos.x < 2.75 and brick4Broke == True:
        ball.velocity = vector(ballXV, ballYV * -1,0)
        brick4Broke = False
        brick4.color = color.black
    if ball.pos.y > brick5.pos.y and ball.pos.x >= 3.25 and ball.pos.x < 4.9 and brick5Broke == True:
        ball.velocity = vector(ballXV, ballYV * -1,0)
        brick5Broke = False
        brick5.color = color.black
    ## The ball will always bounce off the paddle into the positive y-direction
    # If the ball hits the paddle while approaching from the left,
    # it will bounce off and move in the positve x-direction.
    if ball.pos.y - 0.3 <= paddle.pos.y and ball.pos.x > paddle.pos.x - .75 and ball.pos.x < paddle.pos.x + .75 and ballDirectionRight == 0 and ballDirectionUp == 0:
        ball.velocity = vector(ballXV, ballYV,0)
        ballDirectionUp = 1
        ballDirectionRight == 0
    # If the ball hits the paddle while approaching from the right,
    # it will bounce off and go in the negative x-direction direction.
    if ball.pos.y - 0.3 <= paddle.pos.y and ball.pos.x > paddle.pos.x - .75 and ball.pos.x < paddle.pos.x + .75 and ballDirectionRight == 1 and ballDirectionUp == 0:
        ball.velocity = vector(ballXV * -1, ballYV,0)
        ballDirectionUp = 1
        ballDirectionRight == 1

    if ball.pos.y - 0.3 <= paddle.pos.y and ball.pos.x > paddle.pos.x - .75 and ball.pos.x < paddle.pos.x + .75 and ballDirectionRight == 1 and ballDirectionUp == 1:
        ball.velocity = vector( ballXV * -1, ballYV, 0)
        ballDirectionRight = 1
        ballDirectionUp = 1

    # If the ball hits the bottom of the screen the user loses
    if ball.pos.y < -4.9:
        text(text='You LOSE', align = 'center', depth = 4, color=color.blue)
        break

    # If the ball hits the top boundary while approaching from the left,
    # it will bounce down and in the opoosite direction.
    if ball.pos.y > 4.9 and ballDirectionRight == 0 and ballDirectionUp == 1:
        ball.velocity = vector(ballXV, ballYV * -1, 0)
        ballDirectionUp = 0
        ballDirectionRight = 0
    # If the ball hits the top boundary while approaching from the right,
    # it will bounce down in the opposite direction.
    if ball.pos.y > 4.9 and ballDirectionRight == 1 and ballDirectionUp == 1:
        ball.velocity = vector(ballXV * -1, ballYV * -1, 0)
        ballDirectionUp = 0
        ballDirectionRight = 1

    # If the ball hits the top boundary while approaching from the left,
    # it will bounce down and in the opoosite direction.
    if ball.pos.y > 4.9 and ballDirectionRight == 0 and ballDirectionUp == 0:
        ball.velocity = vector(ballXV, ballYV * -1, 0)
        ballDirectionUp = 0
        ballDirectionRight = 0

    # If the ball hits the right boundary while moving up, then the x-direction will
    # change and the ball will continue to move up.
    if ball.pos.x > 4.9 and ballDirectionRight == 0 and ballDirectionUp == 1:
        ball.velocity = vector(ballXV * -1, ballYV, 0)
        ballDirectionRight = 1
        ballDirectionUp = 1

    if ball.pos.x > 4.9 and ballDirectionRight == 1 and ballDirectionUp == 0:
        ball.velovity = vector(ballXV * -1, ballYV, 0)
        ballDirectionRight = 0
        ballDirectionUp = 0

    # If the ball hits the right boundary while moving down, the the x-direction will
    # change and the ball will continue to move up
    if ball.pos.x > 4.9 and ballDirectionRight == 0 and ballDirectionUp == 0:
        ball.velocity = vector(ballXV * -1, ballYV * -1, 0)
        ballDirectionRight = 1
        ballDirectionUp = 1

    # If the ball hits the left boundary while moving up and in the negative x-direction
    # the x-direction will change and it will continue moving up.
    if ball.pos.x < -4.9 and ballDirectionRight == 1 and ballDirectionUp == 1:
        ball.velocity = vector(ballXV, ballYV, 0)
        ballDirectionRight = 0
        ballDirectionUp = 0

    # If the ball hits the left boundary while moving down, then the ball
    # will bounce in the negative x-direction.
    if ball.pos.x < -4.9 and ballDirectionRight == 1 and ballDirectionUp == 0:
        ball.velocity = vector(ballXV, ballYV * -1, 0)
        ballDirectionRight = 0
        ballDirectionUp = 0

    # If the user manages to break all of the bricks,
    # then “You Win” will be displayed across the screen and the game will end.
    if brickBroke == False and brick2Broke == False and brick3Broke == False and brick4Broke == False and brick5Broke == False:
        text(text='You WIN', align = 'center', depth = 4, color=color.blue)
        break
