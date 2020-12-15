# Timers Lab

In this lab you'll start to get an appreciation of what a timer is, how
it works, and how we can make something that is pretty cool using it.

For this demo we're going to start with a very basic timer. You'll be
able to get timers lab by checking out the repository from github here:
<http://go.djosv.com/timer> . Once you accept the assignment, make sure
to clone the repository in eclipse, making sure to select the ***import
existing projects into eclipse*** checkbox. Be sure to read the entire
document, as I'll be asking you to commit and push at various parts
while you're working on the project.

## What is a timer?

A **timer** is a Java class whose sole responsibility is to wake up
every X milliseconds. You end up specifying how often you want a timer
to wake up. From an analogy standpoint, it's very much like a repeating
alarm. Since a ```Timer``` is a class, to create one of these alarms, we
have to create a timer using similar syntax that we've used with any
other class:

```java
Timer someTimerVar = new Timer(1000, objectToBeWokenUp);
```

Since the java library is so vast, there are actually multiple classes
that are named ```Timer```. The reason for this is outside the scope of this
lab, but we will use the ```javax.swing.Timer``` class, so you should
```import javax.swing.*;```

The two arguments that a ```Timer``` takes is the number of ms that should
pass before it wakes up, and which class should be woken up. This can be
another class or object altogether (like I have written with
```objectToBeWokenUp```). If you would like the same class where you are
writing the Timer code to wake up instead, then you would use the
special keyword ```this``` … ```this``` refers to yourself, the object that you
are currently in. Any class that would like to be responsible party for
handling the alarm must implement the ```ActionListener``` interface. The
```ActionListener``` interface has just one method, ```public void
actionPerformed(ActionEvent e)```. So every time the timer wakes up, it
calls the ```objectToBeWokenUp```'s ```actionPerformed``` method. The structure
that you follow is very similar to implementing ```mouseDragged```, except
that rather than you responding to mouse events, you are responding to
events that the ```Timer``` is initiating. This also means that we don't
want to spend too much time doing anything complicated in mouse events
or in handlers like ```actionPerformed```. If you are curious, put a really
large for loop that counts from 0 to a billion in ```mousePressed```, and
watch the program grind to its knees. It does this because java treats
timers and mouse events as being very important, so important that it
stops doing other things (like listening to other events) until those
handlers are finished. So what you do in each should be small, and not
have too many nested for loops or very long loops, so that your program
doesn't hang.

While the new ```Timer``` line shown at the top of the page sets up the
```Timer```, it does not actually turn the timer on. To start the timer so
that it repeatedly calls ```actionPerformed```, you must tell that
particular timer to start, by calling its ```start``` method. So to start
the Timer, you'll see a line like this shortly after creating a ```Timer```
object.

```java
someTimerVar.start();
```

## MyFirstTimer.java

Let's go ahead and open up **MyFirstTimer.java** to start writing in the
code. Notice that here we have a simple ```GraphicsProgram``` with a
```GLabel``` added to the screen. If you run the program, you'll get a
simple window with a label that does nothing. Let's first start by
adding a Timer to the **run** method. You can name the timer anything
you want, but have the 1<sup>st</sup> argument be ```1000``` and then place
**this** as the 2<sup>nd</sup> argument. So ```MyFirstTimer``` will be
notified every second or 1000ms. Because of this, you'll see some errors
at the moment, which just means that if we want to be notified, we need
to implement the ```ActionListener``` interface. Let's add ```implements
ActionListener``` to the top of the class, as well as an ```actionPerformed```
method.

```java
public void actionPerformed(ActionEvent e) {
    //TODO add what we want to do every two seconds
}
```

One thing we can do is simply move the label horizontally every 1000ms.
Go ahead and have it move 5 pixels to the right, while moving it zero
pixels in the y direction. Run the program again to watch it change.

Before we move on to the next program, it's important to understand a
classic procedure, which is to have a variable that keeps track of how
many times ```actionPerformed``` has been called. Let's create a private
variable ```numTimes``` that keeps track of how many times ```actionPerformed```
has been called.

To do this in ```run()```, make sure to initialize that number to 0, and
increment it at the end of ```actionPerformed```. Now just for fun, let's
change the text of the label so that it updates the label with the
```numTimes```, we can do this by calling ```setLabel```, or something like
this:

```java
myLabel.setLabel("times called? " + numTimes);
```

When you run this again, you'll notice the text will slowly move and
will also update every second or so with the number of times that it has
been called.

The **Timer** objects in java have a lot of flexibility, and so I
encourage you to later look at all the ways you can use and leverage a
timer. But for now I want to direct you to two methods, one is **stop**
which would stop the timer and the other is ```setInitialDelay```, which
takes an integer for how much you want to delay before starting the
constantly running the timer. It's best to add the delay before you
call. Try to add a three second delay before the timer starts. While the
concept of **stop** seems simple enough, it may not always make sense as
to when or who should be able to make the timer stop. Let's add a call
to stop the timer in ```actionPerformed```. However, if we declared our
Timer as a local variable, we'll have to switch it to be an instance
variable instead. Since ```actionPerformed``` is called many times, there is
probably a special time when we would want to stop the timer. In this
situation, use an ```if``` statement to say that we want to stop the timer
when ```numTimes``` has reached 20. When you run your program, you should
get to a screenshot that looks like this.

![](lab10media/media/image1.png)

Once you have that, **commit and push your work**, make sure to **put a
brief message in the commit that explains what you did** and then move
on to the next task.

## BallLauncher.java

So far, we've mostly been showing the basics of Timers, however, Timers
end up being extremely useful in many situations, since what they allow
you to do is to have a separate process (which I'll call thread) that is
running independently of the program's main loop. This is going to be
useful for creating animations or having any movement so that the
movement and animations can be made independently of what the user
clicks on or interacts with.

Similar to mouse events, the timer events happen independently of what
is going on in other parts of the game, this allows us to create some
very flexible & awesome programs. While it may be tempting to have many
different timers, *I would instead suggest for now you have one timer
and have it be called more often*. If you only need something to happen
every second and your timer is happening every 500 ms, just have a
```numTimes``` variable and check to see if ```numTimes``` is even. Let's go
through a slightly more complicated example by opening up
**BallLauncher.java**:

If you run at this point, what you'll notice is something similar to our
earlier interactivity lab. When we are clicking the mouse button, it is
making a ball at the same height as our mouse cursor but on the left
hand of the screen. For example, if you click near the top, middle and
bottom of the screen, you'll get three red circles on the left hand side
like so:

![](lab10media/media/image2.png)

The code that is in the file is the following:

```java
public class BallLauncher extends GraphicsProgram {
    public static final int SIZE = 25;
    
    public void run() {
        addMouseListeners();
    }

    public void mousePressed(MouseEvent e) {
        GOval ball = makeBall(SIZE/2, e.getY());
        add(ball);
    }

    public GOval makeBall(double x, double y) {
        GOval temp = new GOval(x-SIZE/2, y-SIZE/2, SIZE, SIZE);
        temp.setColor(Color.RED);
        temp.setFilled(true);
        return temp;
    }
}

```

Most of this should be a refresher, the one thing to notice is how we
are using a constant ```SIZE``` to create a circle. If you remember the
reason why in ```GOval``` we are doing subtracting ```SIZE/2``` is because we
want the x and y coordinates given to be the center of the circle,
instead of the top left, which is what happens whenever we create an
oval or rect, (the x and y given end up representing the top left), so
subtracting half makes it the center). ```makeBall``` is just a convenience
method to help make a red filled ball. What we are going to do is to
move all of the balls that are created to the right. To start this, we
want to make an ```ArrayList``` of ```GOval```s, which we can call ```balls```. Go
ahead and declare a new ```ArrayList``` as a private instance variable at
the top, initializing it in ```run```. The other thing we want to do is
after ```makeBall``` is called, we want to add that ball to our list of
balls by calling the ```add``` method. If you run the program now everything
would still be the same. Let's change that by telling the program to
setup a timer that moves all of the balls in our ```ArrayList```.

Do this by making a **Timer** object and starting it. To help us with
the timer object, make two more constants, ```public static final int```,
just like the ```SIZE``` constant. One constant should store how often we
want the timer to be woken up (set it to 50), and another should tell us
how much to move each ball by each time (set this to 2). Like in
```MyFirstTimer```, we will make ourselves the ones to respond to the timer,
which means we will have to implement ```ActionListener``` and define the
```actionPerformed``` method. Our ```actionPerformed``` method will be fairly
simple, we will have it iterate through the balls ```ArrayList``` and move
each one. If you haven't tried doing a for loop using the **:** operator
to iterate through a list, now would be a good time to try.

Once you handled the ```actionPerformed``` event by moving all the balls in
the list, created and started the timer, you should run your program
again, and click around and notice how you have something that feels
much more dynamic and expressive\! Once this is working, you should
**commit and push your code** again using the message **"all balls move
across the screen"**. If it's not behaving how you would expect, some
things to check are that you added everything to the list (and to the
screen), you created and started the timer, your ```actionPerformed```
method is being called.

The last thing we are going to add is a limit to the number of balls
that someone can launch at any given time. While there are many ways to
do this, what we are going to do is simply cycle through the list of
balls, and look to see if any have an x coordinate that is less than 100
(by calling ```getX()``` on a single ball). If there is a ball that has an x
coordinate that is less than 100, then we know ```mousePressed``` was just
recently launched, so we can just return from ```mousePressed```, and ignore
the user's press of the mouse button, once you fix that and read the
note below, commit and push your latest change. **However, as part of
your commit, *in your own words ****write a sentence or two in the
commit message text that would describe what would happen if you were to
use ```pause``` in an ```actionPerformed``` method**.***

***Additional note on Ball Launcher:*** One thing some of you might have
remembered is that ```GraphicsProgram``` has a ```pause()``` method that would
simply pause the program for a certain amount of time. Why do we have to
go through all this trouble of doing timers if we can simply call
```pause()```? The reason is because of how mouse events work together with
animations. To make things animate and still respond to mouse events,
all of those events need to be done in a way where the computer can
respond to both and not get confused. If you think about our discussion,
what would happen when you click the mouse while the program is going
through the list and moving all of the balls. In the case of having the
timer, the program will actually wait until it is done with the
```actionPerformed``` before it calls ```mousePressed```, so that it feels like
both work at the same time (aka concurrently). Because mouse events and
our timers both involve the user interface, java will know enough that
it can coordinate between the two. However, if you decide to use
```pause```, java does not realize that you are just pausing to add some
pizzazz to your movement. So if the mouse is pressed, it will interrupt
whatever loop it is in at the moment. If you try to replace your timer
with a pause and a while loop, you'll notice that once you run the
program and click a couple of times, you'll get a
```ConcurrentModificationException```. This exception means that you are
trying to modify the list of balls, while the program is currently
cycling through and moving them, something that you cannot do when you
use the for loop with the : operator. I've given the bad example below
of what your run shouldn't do (unless you want to get the
```ConcurrentModificationException```.

```java
// This is buggy code
public void run() {
    balls = new ArrayList<GOval>();
    // movement = new Timer(MS, this);
    // movement.start();
    addMouseListeners();

    // Having this while loop and for loop is no bueno! 
    while(true) {
        for(GOval ball:balls) {
            ball.move(SPEED, 0);
            pause(MS);
        }
    }
}
```

## DodgeBall.java

Alright, so now that you have some experience let's try to make a very
simple game by using timers and some randomness. We're going to add a
little bit to our ball launcher game but rather than just add on top of
that one, we're going to work with a copy of it. Open **DodgeBall.java
and run it**. Notice that it works the same as BallLauncher at the
moment. What I've done is added a couple of additional convenience
methods. With our newfound ability to launch balls, let's try to have
our users aim those balls at others. To help us with this, I ended up
adding an ```ArrayList``` of ```GRects``` called ```enemies```, which are just going
to be green rectangles. I have already added the code to create the
empty list of enemies in ```run```. There is also an analogous ```makeEnemy```
function which will create a green square to represent our enemies. To
make it easier to use, I only have it provide a **y** coordinate, and
the method automatically places the enemy on the right hand side of the
screen. You'll also notice that we have a ```RandomGenerator rgen``` that we
have created. One last refactoring that you'll see is that I created
both an ```addABall``` and ```addAnEnemy``` function, each of which will call
their respective ```make____``` functions, and then add them to the
screen and their respective lists. Lastly, I moved the ball movement for
loop into its own method, ```moveAllBallsOnce()```, which will just go
through that for loop once. The last thing is that you'll see that I
have an if statement to ignore mouse presses if the user tries to click
too soon.

Our first step is to call ```addAnEnemy``` in our timer. However, we don't
want to call ```addAnEnemy```, everytime ```actionPerformed``` is called, but
rather every once in a while. This is where something like that ```%```
operator works great with a ```numTimes``` variable like we discussed in the
first timer example. Because we are keeping track of the number of times
```actionPerformed``` is called, we can simply say every 40<sup>th</sup>
time, go ahead and make a new enemy,

```java
if(numTimes % 40 == 0) {
    addAnEnemy();
}
```

Normally I would want you to make 40 a constant, but you can leave it
like this for today (or you can just click on 40 and then click
*Refactor-\>Constant* in the menu). Once you add these lines of code in
```actionPerformed```, run the code again and watch the green squares slowly
take over\! When the squares appear **commit and push your code again**
with the message **"enemies populate screen regularly"**.

![](lab10media/media/image3.png)![](lab10media/media/image4.png)

You can still have circles appear but right now they will just run over
the squares. We'll get to that in a bit. The next thing we want to do
however is to make the enemies have some type of movement. The easiest
thing to try to do is to have each enemy randomly move between say a -2
and +2, so we can use ```rgen.nextInt``` to give us a random number between
these and then have it move this much. Leveraging the code we have for
```moveAllBallsOnce```, create a method calls ```moveAllEnemiesOnce``` that will
cycle through the list of enemies and call move on each one, moving it 0
pixels in the ```x``` direction and a random integer (```SPEED```) in the ```y```
direction. Then make sure that you call ```moveAllEnemiesOnce``` in
```actionPerformed```. Once you run the program now, the enemies will come
to life\! They will be very wiggly. At this point you should **commit
and push again,** with the message **"added wiggly enemy movement"**.

At this point I'm guessing that you probably want some way to get rid of
these green squares using the ball. While there are many ways for
checking to see if a ball and rectangle overlap, that's not really part
of this course, so I'm going to explain a very simple way of checking to
see if the ball may have hit a square. One thing you can do is that for
every red ball, when it's their time to move, check a point just outside
of the ball and call ```getElementAt```. If the object that is given back is
an instance of a ```GRect```, we could say it's an enemy and then make that
enemy disappear. In this, case we'll want to check the point that is
highlighted in red below.

If it turns out that we do have potential enemy in front of us (by
calling ```getElementAt``` using the measurements shown above), then we want
to remove it not only from the screen (by calling **remove**) but also
from the ```enemies```. Once you run this you should be able to make the
green squares disappear by having the balls hit them\! Once again,
**commit and push** since you've added something cool, in the commit
message detail what it is you added.

While this does not yet have the polish of some of the games you may
tend to play, there are many things you can do to make it more polished.
For example, you could add a label that is continually updated or set in
```actionPerformed``` that would update the number of green squares on the
screen (see bottom left). You could also add in checks to say if there
are more than ```MAX\_ENEMIES``` on the screen, then stop the timer, remove
everything an add a message telling them they lost. Lastly, it's fairly
simple to give them some indicator of progress or score by simply on the
lose screen presenting them with the ```numTimes``` variable, which is in a
way, representative of how long they were able to last in playing the
game. Hopefully you can see how easy this is to extend and how much fun
you can start to have in creating these things and making the game your
own, with your own changes. Hopefully you see the powers that timers can
have in making your programs more dynamic and more expressive.

To get full credit for this lab, you should have your program make the
squares disappear **and follow all the directions on when to push your
code**. Make sure to push your final changes to your repository by the
deadline.

![](lab10media/media/image5.png)![](lab10media/media/image6.png)