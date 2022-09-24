import turtle
import random
import time

Score =0
highScore=0

#setup screen
wn=turtle.Screen()
wn.title("Snake Game")
wn.bgcolor("#FCFCFC")
wn.setup(height=700,width=700)
wn.tracer(0)

#snake head
head=turtle.Turtle()
head.speed(0)
head.shape("square")
head.color("#104E8B")
head.penup()
head.goto(0,0)

head.direction="stop"

segments=[]

#score board

scoreboard=turtle.Turtle()
scoreboard.speed(0)
scoreboard.shape("square")
scoreboard.penup()
scoreboard.hideturtle()
scoreboard.goto(0,310)
scoreboard.write("Score: 0 High Score: 0", align="center",font="Consolas")

#snake food
food=turtle.Turtle()
food.speed(0)
food.shape("square")
food.color("red")
food.penup()
food.goto(100,100)

#walls

wall=turtle.Turtle()
wall.width(5)
wall.penup()
wall.speed(0)
wall.goto(-290,290)
wall.pendown()
for i in range(4):
    wall.fd(580)
    wall.right(90)



#function (Direction of snake)

def go_up():
    if head.direction!="down":
        head.direction="up"

def go_down():
    if head.direction!="up":
        head.direction="down"

def go_left():
    if head.direction!="right":
        head.direction="left"

def go_right():
    if head.direction!="left":
        head.direction="right"                

#moving snake using above function
def move():
    if head.direction=="up":
        y=head.ycor()
        head.sety(y+20)

    if head.direction=="down":
        y=head.ycor()
        head.sety(y-20)

    if head.direction=="left":
        x=head.xcor()
        head.setx(x-20)

    if head.direction=="right":
        x=head.xcor()
        head.setx(x+20)    

#keboard binding            

wn.listen()
wn.onkeypress(go_up,"w")
wn.onkeypress(go_down,"s")
wn.onkeypress(go_left,"a")
wn.onkeypress(go_right,"d")

while True:
    wn.update()

    if head.xcor()>270 or head.xcor()<-270 or head.ycor()>270 or head.ycor()<-270:
        time.sleep(1)
        head.goto(0,0)
        head.direction="stop"

        score=0
        scoreboard.clear()
        scoreboard.write("Score: {}  High Score: {}".format(score,highScore), align="center",font="Consolas")
        for segment in segments:
            segment.goto(1000,1000)
        segments.clear()    

    if head.distance(food)<20:
        x=random.randint(-270,260)
        y=random.randint(-270,260)
        food.goto(x,y)

        #snake long
        new_segment=turtle.Turtle()
        new_segment.speed(0)
        new_segment.shape("square")
        new_segment.color("#104E8B")
        new_segment.penup()
        segments.append(new_segment)
        
        Score+=10
        scoreboard.clear()
        scoreboard.write("Score: {}  High Score: {}".format(Score,highScore), align="center",font="Consolas")
        
        if Score>highScore:
            highScore=Score
        scoreboard.clear()
        scoreboard.write("Score: {}  High Score: {}".format(Score,highScore), align="center",font="Consolas")

        #moving the segment follow head and previous

    for index in range(len(segments)-1,0,-1):
        x=segments[index-1].xcor()
        y=segments[index-1].ycor()
        segments[index].goto(x,y)

    if len(segments)>0:
        x=head.xcor()
        y=head.ycor()
        segments[0].goto(x,y)


    move()

    for segment in segments:
        if segment.distance(head)<20:
            time.sleep(1)
            head.goto(0,0)
            head.direction ="stop"

            score=0
            scoreboard.clear()
            scoreboard.write("Score: {}  High Score: {}".format(score,highScore), align="center",font="Consolas")

            for segment in segments:
                segment.goto(1000,1000)
            segments.clear()  
            
    time.sleep(0.1)

wn.mainloop()
