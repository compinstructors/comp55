# HashMap Lab (Part 1)

- [Objectives](#objectives)
- [Hash-what?](#hash-what)
- [Why didn't they just call it a hashtable then?](#why-didnt-they-just-call-it-a-hashtable-then)
- [Basic HashMap Usage](#basic-hashmap-usage)
- [Intermediate HashMap Usage](#intermediate-hashmap-usage)
- [More advanced rules for HashMaps](#more-advanced-rules-for-hashmaps)
- [But...what if there are multiple meanings for the same word?](#butwhat-if-there-are-multiple-meanings-for-the-same-word)
- [Ok, so how do we use this?](#ok-so-how-do-we-use-this)
- [Getting credit for part 1](#getting-credit-for-part-1)

## Objectives

This lab has two components.
The first part is reading heavy,
but not too long.
In this lab, you're going to learn

- *What a HashMap is*
- *How it differs from a Hashtable*
- *How to store and retrieve items from a HashMap*
- *In which ways HashMaps are useful*

In the [second part of the lab](12-A-Hash2.html),
you'll walk through a program in a style similar to what we've done previously.
This time though,
we will show you how to use a ```HashMap```.

## Hash-what?

Similar to an ```ArrayList```,
a ```HashMap``` stores a collection of items.
A ```HashMap``` is a standard part of the Java Library,
similar to what some of you may remember with the **map** STL library in C++.
In COMP 53,
you may have some faint recollection of what a hashtable is,
which was a data structure that would compute a **hash** based on a value,
and then use that hashed value as an index to be used to locate the index in an array.
Hashtables allowed us to search and store items into a collection in constant O(1) time,
which means that no matter the size of the hashtable,
you would be able to count on the running time not increasing
(as long as the number of buckets is roughly equal to the
number of items and you have a full hash function yada-yada)

## Why didn't they just call it a hashtable then?

By calling it a ```HashMap```,
Java is trying to make clear that ```HashMap```
implements the ```Map``` interface.
```Map```s are meant to form
a bond,
or mapping,
between two items.
Java has a ```HashTable``` class that is
nearly identical to the ```HashMap``` class,
but ```HashTable``` is now obsolete and still available for legacy reasons.
These two classes are not to be confused with Java's ```HashSet``` data structure,
which uses hashing to implement Java's **Set** interface.
All three data structures use a
[hashing function](https://computersciencewiki.org/index.php/Hashing) for different purposes,
and there are still other
[hashtable-esque objects meant for thread-safe operations](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/ConcurrentHashMap.html).

Java's ```HashMap``` is often thought of or referred to
as a **dictionary** or **associative array**,
where the key is hashed.
Like a dictionary,
when you end up looking up a word in a dictionary,
you get a definition for that word.
In a way, it links or maps the word with the definition.
In computer science lingo,
the word would end up being called the **key**,
while the definition would be called the **value**.
To make ```HashMaps``` truly work,
```HashMaps``` have a condition that a
**key can only correspond to one value**.
So for example,
going back to the dictionary example the key *nomophobia* would be linked to a specific value:
in our case,
the value will be
"*the fear of losing mobile phone service*".
For a ```HashMap```,
what you would do is put the key and value pairs into a map and then retrieve them in the future.

## Basic HashMap Usage

Like an ```ArrayList```,
we need to first let Java know that we want to use one of these data structures.
One syntactical difference is that Java would like to know the type
for both the key and the value in the angle brackets.
While I have only shown strings,
the keys and values can be any type of Object.
In this particular case though
since the type of both the definition (*value*) and word (*key*) are both strings
we'll declare them in the HashMap like this:

```java
HashMap<String, String> phobias = new HashMap<String, String>();
```

To create a linkage in the ```HashMap```.
One would use the ```put``` call to place something into the ```HashMap```.

```java
phobias.put("nomophobia", "the fear of losing mobile phone service");
```

to put more definitions,
you would simply keep using the put call to create more mappings between keys and values.

```java
phobias.put("pentheraphobia", "the fear of your mother-in-law");
phobias.put("ergophobia", "the abnormal fear of work");
```

Once in the HashMap,
you can use ```get``` to retrieve the definition in the future.

```java
String def = phobias.get("ergophobia")
// def now has the value "the abnormal fear of work"
```

If you try to get a phobia
that has not been introduced into this particular phobias map
like say this one

```java
String possibleDef = phobias.get("sebastianphobia")
```

The ```HashMap``` will return ```null```,
which it does for any key that has not been explicitly put into the corresponding HashMap object
(in our case, ```phobias```).

## Intermediate HashMap Usage

Similar to an ```ArrayList```,
you can use ```size``` to obtain the number of definitions stored in the ```HashMap```.

```java
int numPhobias = phobias.size();
```

To check if a ```HashMap``` is empty,
you can call ```isEmpty``` to return a boolean that indicates whether it's empty or not.

```java
boolean empty = phobias.isEmpty();
```

To delete an item from a ```HashMap``` you can call ```remove```.

```java
phobias.remove("pentheraphobia");
```

This will remove both the word and definition from the ```HashMap```.
If you want it back just add it again!!

```java
phobias.put("pentheraphobia", "...");
```

But wait,
it seems your ```HashMap``` now has a wrong value,
how might you edit this? Simple..
just use ```put``` again!!

```java
phobias.put("pentheraphobia", "the fear of your mother-in-law");
```

Now the ```HashMap``` should be back to where it was,
well done!!

To check whether all your phobias are defined correctly,
you can do so by traversing through the ```HashMap``` using a ```for``` loop over the set of keys,
like so:

```java
for(String key: phobias.keySet()) {
    System.out.println(key + " : " + phobias.get(key));
}
```

This should output:

```text
ergophobia : the abnormal fear of work
nomophobia : the fear of losing mobile phone service
pentheraphobia : the fear of your mother-in-law
```

## More advanced rules for HashMaps

Remember,
***each key corresponds to one and only one value object***.
So how does this work when you have multiple objects that you want with similar values?
Let's try this first.

```java
phobias.put("ponophobia", "the abnormal fear of work")
```

If you have been reading this document closely you may notice that we
already have a name for the abnormal fear of work in *ergophobia*.
So now we have two different keys that use the same value.
This is OK.
This does not violate anything from a HashMap standpoint,
since both keys still have just one value.
However,
the opposite would not hold.
Let's explore that notion with the following statement.

```java
phobias.put("nomophobia", "the fear of losing mobile phone service or your mobile phone device")
```

Since our ```phobias``` HashMap has already linked *nomophobia*,
Java will **replace** that linkage with the one given here.
Thus,
no longer would the shorter definition be linked to *nomophobia*,
it would be replaced with this longer definition.
**Each key can only correspond to one value.**

## But...what if there are multiple meanings for the same word?

In that case, if you want to store multiple meanings,
you'll have to select an object or class that will allow you to store multiple meanings.
For example,
you can have a word that has multiple meanings sometimes even contradictory.
Let's start up another ```HashMap```,
this time for contronyms which are words that often have opposite meanings.
For example,
think about the word ***dust***.
Usually, it means to take off something right?
For example,
when you want to *dust* the furniture.
However,
the word *dust* can also mean to add something instead of taking it away!
For example, you may want to *dust* a pan with flour.
In this case, the word **dust** has two meanings.
Let's say we want to create a map that has both.
How would this be done?
In this situation, we could create some long string,
but what we can do is simply create a different linkage,
one in which we'll link a word,
with another storage mechanism that would allow us to store multiple definitions.
The easiest structure you have for that is to use an ```ArrayList```.
So we are going to link a key which will be a ```String```
with a value that will be an ```ArrayList``` of definitions.
Let's create the ```ArrayList``` first.

```java
ArrayList<String> dustDefs = new ArrayList<String>();
dustDefs.add("To take away fine particles");
dustDefs.add("To add particles");
```

Now to create this linkage in a map,
we'll create a new ```HashMap``` that links the word with this ```ArrayList``` that we just created.

```java
HashMap<String, ArrayList<String>> contronyms = new HashMap<String, ArrayList<String>>();

contronyms.put("dust", dustDefs);
```

Then,
whenever you call ```get```,
you will store the value that you receive back from the HashMap as an ```ArrayList```.
For example:

```java
ArrayList<String> wordDefs = contronyms.get("dust");
```

## Ok, so how do we use this?

The possibilities are endless here!

Here is a sample piece of code that I see often
that I think can be done more nicely by using hashmaps.
First study the code below.

```java
if(state == "up") {
    upOval.move(0, -5);
    sendMessage("north");
}else if(state == "down") {
    downOval.move(0, 3):
    sendMessage("south");
}else if(state == "left") {
    leftOval.move(-7, 0);
    sendMessage("west");
}else if(state == "right") {
    rightOval.move(8, 0);
    sendMessage("east");
}
```

Now there are plenty of variations to this,
but they are largely the same issue,
which is that people feel that they cannot make this type of function better,
since there are too many things going on here.
You have different objects with different variables and parameters for each one.
How could this be cleaner?
Let's first say what if we had some type of ```Movement``` class?

```java
public class Movement {
    private String direction;
    private int dy;
    private int dx;
    private GOval oval;

    //getters and such
    public void move() {
        oval.move(dx, dy);
    }
}
```

Now what you can do is create a ```HashMap``` between this state and this
movement class.

For example:

```java
HashMap<String, Movement> moveMap = new HashMap<String, Movement>();

moveMap.put("up", new Movement(upOval, 0, -5, "north"));
moveMap.put("down", new Movement(downOval, 0, 3, "south"));
moveMap.put("left", new Movement(leftOval, -7, 0, "west"));
moveMap.put("right", new Movement(rightOval, 8, 0, "east"));
```

Then when you have to do the movement,
you can write this.

```java
Movement moveData = moveMap.get(state);
if(moveData != null) {
    moveData.move(); 
    sendMessage(moveData.getDirection());
}
```

Now some of you may rightfully say that this is more code,
but think about what this represents.
We are putting all of the code that deals with changes into one place.
And what would happen if we move to 8 directions?
And what if we need to add two more lines
of code inside of our if statement that needs more movement data?
Hopefully, you start to see that this would be a cleaner solution.

## Getting credit for part 1

Part of the credit for this lab
will be in thinking about what hashmaps are by thinking about the four questions up top.
You should discuss this with your team,
but what I'd like you to do is
**answer the four questions at the beginning of the document in your own words (not your team's)**.
For the useful part,
you will come up with at least three different potential places
where you could use a ```HashMap``` in your project,
and provide example code for how it can improve your project.
Remember that what I've shown you here has been using mostly strings as keys,
however,
your key can be any object that you choose.
Think about how you could implement or link a ```GObject``` with a class,
or vice-versa.
HashMaps are pretty cool and can save us a lot of work.

Once you are done,
you can [move on to part 2](12-A-Hash2.html).
