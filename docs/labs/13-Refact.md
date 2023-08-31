# Refactoring Lab

- [Rationale](#rationale)
- [The biggest culprit *if statements*](#the-biggest-culprit-if-statements)
    - [Principles to follow](#principles-to-follow)
        - [1. If you see a comparison to a boolean, take it out](#1-if-you-see-a-comparison-to-a-boolean-take-it-out)
        - [2. Avoid using negations](#2-avoid-using-negations)
        - [3. Simplify if statements to use return when possible](#3-simplify-if-statements-to-use-return-when-possible)
        - [4. Refactor out the duplicate parts of if statements](#4-refactor-out-the-duplicate-parts-of-if-statements)
        - [5. Use variables to help you refactor code](#5-use-variables-to-help-you-refactor-code)
        - [6. Use data instead of logic to store variations](#6-use-data-instead-of-logic-to-store-variations)
        - [7. Methods can also make your code more readable](#7-methods-can-also-make-your-code-more-readable)
        - [8. Make constants for repeating values](#8-make-constants-for-repeating-values)
- [Refactoring Lab Directions](#refactoring-lab-directions)

## Rationale

By this point, you should have a feeling that there are many different ways for someone to write code,
and one thing that we would like to do in this class
(and for you to do in the future)
is to make sure you write your code in a way that can make it easy to change and be flexible.
**Software changes**.
Most of the time we are worried about correctness or speed,
but we should also concentrate on readability.
This is especially true when we are coding with other people.
If the code ends up being difficult to read or overly complicated,
it's less likely to be tested and reviewed for errors.
What we are going to do today is start learning strategies we can use
to do what's called **refactor code**,
which means revise the code.
Refactoring not only can increase readability,
but it can also reduce complexity,
which will make your code easier to test and debug.

## The biggest culprit *if statements*

In case you don't know,
I'm not a big fan of ```if``` statements.
If statements are often overused,
especially by budding programmers.
One of the common issues is using too many ```if``` statements,
when in fact you would only need one or two.
This becomes very egregious when the ```if``` statements then lead to code
that looks mostly the same in the different sections of the if statement,
resulting in the loss of many kittens from all the copying and pasting that is being done.
This lab will go over a set of steps that you can perform
so that you eliminate redundancy and complexity from your code,
which is a win for everyone.
Today is about teaching you some of the common steps you can take to refactor your code.
Let's go over some basics first.
You'll see code with different emojis based on whether it needs to be refactored or not,
we'll use the 😱 when it's problematic,
😑 when its somewhat troublesome and ❤️ when it's a good example to follow.

### Principles to follow

#### 1. If you see a comparison to a boolean, take it out

If the boolean is false,
simply use a negation

| 😱 | ❤️ |
|:---:|:---:|
|```if(isLow == true)```|```if(isLow)```|
|```if(isLow == false)```|```if(!isLow)```|

#### 2. Avoid using negations

Negations can often be rewritten or restructured.
For example,
in the step above,
if ```isLow``` is only being used once and you are constantly checking ```if(!isLow)```,
then why not change the variable to make it ```isHigh```?
There are usually ways for you to minimize the number of negations that you have,
as negations just make things confusing.
If you don't believe me,
then just think about what this statement means here in English.

```java
// What does this condition test?
if(x != 4 || x != 5) //😱😱😱
```

While many of you look at it and think that the *statement evaluates to true with numbers
that aren't equal to four or five,*
*that is incorrect. It works for all numbers.*
You can use some boolean theory to extract this statement to its equivalent version,
which is here:

| 😱 | ❤️ |
|:---:|:---:|
|```if(x != 4 || x != 5)```|```if(!(x == 4 && x == 5))```|

Based on DeMorgan's laws,
the two statements here are the same,
yet one is more confusing to decipher.

#### 3. Simplify if statements to use return when possible

*If you have an if/else statement
and all it's doing is returning ```true``` or ```false``` based on the condition,
use a single ```return``` statement instead.*

| 😱 | ❤️ |
|:---|:---|
|<code lang="java">if(isLow) {<br/>&nbsp;&nbsp;&nbsp;&nbsp;return true;<br/>}else{<br/>&nbsp;&nbsp;&nbsp;&nbsp;return false;<br/>}</code>|```return isLow;```

#### 4. Refactor out the duplicate parts of if statements

*If you see a lot of code that looks similar in each branch of an if/else statement,
take the necessary steps so that it is not repeated.*

The easiest way to deal with this is when the code looks the same in both sections:

| 😱 | ❤️ |
|:---|:---|
|<code lang="java">if(x > 5) {<br/>&nbsp;&nbsp;&nbsp;&nbsp;x+=10;<br/>&nbsp;&nbsp;&nbsp;&nbsp;System.out.println("another # plz");<br/>}else{<br/>&nbsp;&nbsp;&nbsp;&nbsp;x-=10;<br/>&nbsp;&nbsp;&nbsp;&nbsp;System.out.println("another # plz");<br/>}</code>|<code lang="java">if(x > 5) {<br/>&nbsp;&nbsp;&nbsp;&nbsp;x+=10;<br/>}else{<br/>&nbsp;&nbsp;&nbsp;&nbsp;x-=10;<br/>}<br/>System.out.println("another # plz");</code>|

Most of the time you will have code at the beginning or the end of the curly braces
that are in both sets of statements.
If it is the same at the beginning,
take it out of the ```if``` statement,
and then place it either before or after the if statement depending on where it's located.

While this example may seem highly trivial,
slightly more complex versions of this tend to happen in many places.
Sometimes students will see that one small change happens in the middle of six lines of code,
and for that reason end up repeating everything,
when some should be moved before the if statement and the other lines should be moved after.

#### 5. Use variables to help you refactor code

If you do see if statements that look the same,
but feel that it needs to be repeated,
ask yourself –
*"Is there some way to standardize the operations
by storing the different information in variables?"*

I get it,
the last example was quite trivial,
which we are doing for the sake of having a short lab,
but let's elaborate on our previous example a little more.

😱

```java
if(x > 5) {
    System.out.println("I'm going to add 7 here");
    x+=7;
    System.out.println("now pick the next number");
}else{
    System.out.println("I'm going to subtract 10 here");
    x-=10;
    System.out.println("now pick the next number");
}
```

😱

You may have seen code that looks similar to this before,
you have some branches in your code that have some differences,
and so you tend to write them out.
However,
in both branches,
you are printing out a message,
doing some addition/subtraction,
and then printing out another message.

Remember that subtraction can always be made to be addition,
which means that we can change our text slightly.
And there are different ways to do this,
but here's a slightly better solution

😑

```java
int amount = -10;
String action = "subtract";

if(x > 5) {
    amount = 5;
    action = "add";
}

System.out.println("I'm going to " + action + Math.abs(amount) + "here");
x+=amount;
System.out.println("now pick the next number");
```

You may look at the above solution and think
"*Lolz well that's the same number of linezzz*"
as the previous set of statements and believe that therefore both snippets are the same,
however,
this is one simple example of the type of scenario where that would happen,
and what we are doing is what the book said,
which is trying to separate only what is different between the two cases.
**The point is for you to think about all the scenarios
where code in different if sections is the same,
and then to figure out what does not have to be repeated over and over again.
Whatever both pieces share should seriously be considered for rewriting.**

Again,
while the previous code may not feel like much of an improvement,
I would argue that the following code does a better job as it is more readable

❤️

```java
if(x > 5) {
    takeAction("add", 5);
}else{
    takeAction("subtract", -10);
}

// Meanwhile, in some other part of the code...
// (future snippets will use -------)

private void takeAction(String action, int amount) {
    System.out.println("I'm going to " + action + Math.abs(amount) + "here");
    x+=amount;
    System.out.println("now pick the next number");
}
```

Again,
these are trivial examples,
but in most situations,
having this latter version is going to be better than the previous action that you created
because we are reducing the amount of duplicate code.
Ultimately,
depending on what we were trying to do with the function ```takeAction```, we could
decide to go further with that function
by for example taking out the action to add or subtract
as something that is being passed in as a string.
I only provide that here to show how the refactoring would be done.
If I were to refactor this method again,
I would most likely just include the number
and then inside of the function figure out if it's positive or negative
so that I can provide the correct label
(add or subtract).
Another part of making functions is not just to refactor the code,
but to make the functions usable to people in the future
and reduce the number of errors that you could get
while making it easy for your teammates to use.

❤️

```java
if(x > 5) {
    takeAction(5);
}else{
    takeAction(-10);
}

// ----------------------------------

private void takeAction(int amount) {
    String action = "add";

    if(amount < 0) {
        action = "subtract";
    }

    System.out.println("I'm going to " + action + Math.abs(amount) + "here");
    x+=amount;
    System.out.println("now pick the next number");
}
```

For those of you who would like to go further or don't want the extra amount here,
you could go with the ternary operator as well to eliminate the extra lines of code

❤️

```java
private void takeAction(int amount) {
    System.out.println("I'm going to " + (amount > 0)? "add":"subtract" + Math.abs(amount) + "here");
    x+=amount;
    System.out.println("now pick the next number");
}
```

The ternary operator is not something that is used frequently however as it can make
the code dense and more difficult to follow. Again, as previously mentioned,
writing code is expressive and there are different options or strategies that you can take.
*However, the biggest thing that I would like you to get out of this class is
to stop repeating and pasting so much code.*
For this class,
I'm going to be looking a lot at the code that you are writing
and making sure that there isn't much repetition in it.
**<u>The amount of repetition that you have in your code will impact
your final project grade.</u>**

#### 6. Use data instead of logic to store variations

*Are you making a lot of variables?
Consider using a list or other data structure to store them instead.*

I think for this project,
it's pretty easy to start thinking of making instance variables for everything.
Instance variables are nice because they are visible in all methods of every method in a class,
which means you don't have to worry about passing information to and from a function.

However,
the ability to pass information back and forth between methods and understanding scope
is an important tenet in developing software.
Otherwise,
having so many instance variables is akin to giving everyone in your dorm a key to your room.
Sure it's convenient to give everyone a key,
but it's not secure,
and more importantly, when you're trying to debug and figure out who borrowed your Playstation,
it makes it that much more difficult to find out who it was and when it happened.

Like with your dorm room,
there are situations in which your program and all of your functions
do not need to have access to every single one of those instance variables,
so consider using local variables when a variable is only needed in one function.
If you do need to have a collection of variables,
like say when you need to keep track of multiple ```GObject```s on the screen,
it may make more sense for you to create all of the objects and store them in a list,
or potentially a ```HashMap```.
This also makes it nice because then you could create a loop and function
that will create a series of these objects.

For example,
you might remember a function in the [Timers Lab](10-Timer.html##BallLauncher.java)
called ```makeBall```.
What if instead, you create a more general function that passes in
```(x, y, width, height, color, fill)``` as parameters,
so that way you could just call ```makeBall``` and a ball appears?

Here is an example of what that ```makeBall``` function could look like

❤️

```java
void makeBall(double x, double y, double width, double height, Color col, boolean shouldFill) {
        GOval ball = new GOval(x, y, width, height);
        ball.setFilled(shouldFill);
        ball.setColor(col);
        add(ball)
        listOfBalls.add(ball);
}
```

If you are creating 5 balls,
wouldn't it be nice to have a function that was like
```makeBall(30, 30, SIZE, SIZE, Color.BLUE, true)```
and then know it that it will just show up?  
This does not even get to what you could do later,
which is to store all of the information in an object,
and then have that be passed into your for loop instead!
Assume there was a simple ```BallSpec``` class that worked like simple classes before,
meant to just hold some information and store it.
Then look at what could happen to our code.

❤️

```java
// Assume the info was loaded from a file or somewhere else
for(int i = 0; i < ballSpecs.length; i++) {
    makeBall(ballSpecs[i]);
}
```

Then our ```makeBall``` fucntion would merely call getters
for the ```x```, ```y```, ```width```, ```height```, ```color``` and ```shouldFill```
for example to have that all be filled in.

#### 7. Methods can also make your code more readable

**Does the code look confusing? Consider making methods to help you
    make parts of that code more readable.**

If there are sections of code that look crazy,
or feel dense, it's best to go back through and look into making those methods into classes,
since that will make the code more readable,
and if you come up with methods that have return-values based on parameters,
will make them easier to generate test methods for as well,
since you'll be able to say,
*when I expect this, I should get back this*,
which is the basis of unit testing that we went over in the previous lab.

Take the following code as an example.
😱

```java
@Override
public void mouseMoved(MouseEvent e) {
    if(e.getX() < frame.getX() || e.getX() > frame.getX() + frame.getWidth() || e.getY() < frame.getY() || e.getY() > frame.getY() + frame.getHeight()) {
        // do something
    }
}
```

Is it immediately clear what the above if statement does?
Maybe, maybe not.
It's difficult to understand without some context
that the conditions check the location of ```e``` relative to ```frame```.
However,
using a function could make this code clearer.
Rewriting the code above using a function could look something like this:

❤️

```java
private boolean outsideOf(GImage frame, MouseEvent e) {
    return e.getX() < frame.getX() || e.getX() > frame.getX() + frame.getWidth() || 
           e.getY() < frame.getY() || e.getY() > frame.getY() + frame.getHeight();
}

@Override
public void mouseMoved(MouseEvent e) {
    if(outsideOf(frame, e)) {
        // do something
    }
}
```

With this change,
the ```if``` statement remains functionally the same.
However,
by removing the content of the if statement and putting it into a function,
the ```if``` statement becomes more readable.
Additionally,
the ```outsideOf``` function will be easy to test
and can be called again as needed in other parts of the code.

Indeed, one of the common issues is a lack of methods/functions
I understand that making them takes time, but again,
remember that eclipse does offer the *Alt-Shift-M* keyboard shortcut
to help you make or extract a method out of a piece of code,
so I would encourage you to use that to make things more readable and more
testable.

#### 8. Make constants for repeating values

*Do you see a number over and over and over again?
That should be a constant.
If a formula is repeated that either needs to be in a variable or a function*

If a number is specified in your code and it isn't a 0, 1, or 2,
then I generally have a rule of declaring it as a constant.
If you have a number say 30,
or 300 or 50 that is used more than once,
it should be defined as a constant.
Let's take this example:

😱

```java
body = new GOval(200, 400, 100, 100);
head = new GOval(225, 350, 50, 50);
```

While this situation is slightly more complex,
the numbers are much more likely to be repeated.

In this case,
let's first take the number 100,
which seems to be the size of the body.
You could simply decide to make that a constant that you could change up top.

😑

```java
public static final int SIZE = 100;

// ----------------------------------

body = new GOval(200, 400, SIZE, SIZE);
head = new GOval(225, 350, 50, 50);
```

But why stop there? Use formulas to establish the relationships between all of these numbers,
since they are in this case referencing each other.

❤️

```java
public static final int SIZE = 100;
public static final int BODY_X = 200;
public static final int BODY_Y = 400;

// ----------------------------------

body = new GOval(BODY_X, BODY_Y, SIZE, SIZE);
head = new GOval(BODY_X+SIZE/4, BODY_Y-SIZE/2, SIZE/2, SIZE/2);
```

Here,
we are linking these previous arbitrary numbers in your equations to constant variables.
Now,
this new relation does several things for you as the developer.
First,
your code becomes much easier to understand! Since all of your parameters are labeled,
a reader can quickly ascertain the relationships between the parameters.
Secondly,
you can now efficiently and reliably change the values of the constant variables,
and your program will calculate the same equations but with new values.
This will save you lots of time when the alternative is searching every instance of your old number,
trying to remember if this particular instance is related to the change you want to make,
and then praying nothing was missed or overlooked.
This concept makes a lot of sense when applied to filenames,
making them string constants for ease of editing when a filepath is changed.
For example,
here was my traffic jam way of reading files from the project.
Which were *images/car.png, images/car_vert.png, images/truck.png, images/truck_vert.png*,
etc.

❤️

```java
public static final String IMG_EXTENSION = ".png";
public static final String V_IMG_ENDING = "_vert";
public static final String IMG_PATH = "images/";

// ----------------------------------

private void drawCar(Vehicle v) {

    String suffix = IMG_EXTENSION;
    if(v.isVertical()) {
        suffix = V_IMG_ENDING + suffix;
    }

    GImage g = new GImage(IMG_PATH + v.getVehicleType() + suffix, …)
```

---

## Refactoring Lab Directions

Once you have finished reading these questions,
you can now download and take a program to try to do some refactoring:

Your goal for the rest of class is to take these rules and apply them to
the linked project that I've shared with you on canvas,
and by the deadline come up with a refactored solution that still works.
**Don't spend your time playing and exploring the game**,
but rather reading the code and making changes.
This isn't as much about testing and looking for bugs or critiquing
as much as practicing the refactoring principles I've given you here.
Credit will be given to how much more readable,
flexible and possibly simple the code is in relation to the original.

**You can work on this project in pairs,
but should not break up or attempt to break up the work.
Rather you should:**

1. **work on it together,**
2. **propose ideas to each other about how to refactor**
3. **propose ideas on what should be refactored.**

This is not group work but teamwork.
If you're unsure where to start,
I would start by looking at any of the classes that have a *Pane* in the name of the java file.
You should commit  every time you have a change that you'd like to propose…
change it and then commit and push.
Good luck!
