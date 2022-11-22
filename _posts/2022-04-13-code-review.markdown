---
layout: post
title: code review
---

I've recently decided to go back to a project I started last year and then abandoned, to try and rewrite it and get it to work. I was extremely enthusiastic about the idea but somehow couldn't find the motivation to work on it, and I think part of the problem was that I wasn't writing good code to begin with. So naturally I didn't want to go back to it and fix it - because it seemed like an irredeemable mess to me.

The project was basically implementing a genetic algorithm to optimize strategies for Prisoner's Dilemma. I wanted to create a pool of strategies, play them off against each other, evaluate them using a predefined fitness function, and then use a genetic algorithm to select and recombine the most successful strategies. Ideally, this would cause the pool to evolve in the direction indicated by the fitness function.

(Some of) the issues I ran into while trying to do this:

- I was trying to do object-oriented programming but I had no idea how to do it, or what it was even for.

- I didn't write any tests. I didn't understand the concept of writing tests (that were any more sophisticated than adding print statements in random places).

- I didn't know how to separate the logic from the actual implementation. I tried to do both together and got it all mixed up.

Okay, so on to the code review now.

This part of a class that I defined nicely illustrates the first issue:

{% highlight python %}
class Genetic:
    pool = []
    fitness = []

    # Initializes pool with n strings
    def __init__(self, num=10, length=10):
        for i in range(num):
            self.pool.append(self.generate(length))

    # Generates a random string of length n
    def generate(self, n):
        return "".join(chr(97 + randint(0, 25)) for j in range(n))

{% endhighlight %}

The entire point of defining classes (at least in a project like this) is to be able to bundle together variables and methods belonging to the same object. If you want to do that without a class, the easiest way is to do the following:

- make several arrays of the same length representing different properties
- treat each array index as representing a single object, so that the value at that index in each array is the value of the property represented by the array for the object represented by that index
- pass the index to every method that's supposed to do anything with the object, and use that to operate on the arrays
    
Which is basically what I've done with the `pool` and `fitness` arrays.

I could've just made the `fitness` property part of whatever class went into the `pool` array, and then I wouldn't even need the `Genetic` class. I'd just have one piece of data, and I could write functions that operated directly on that.

Also, `pool` and `fitness` are class variables and not instance variables. The reason is that when I wrote this, I did not know the difference. They should have been instance variables. Not that it made a lot of difference, because actually this entire class was unnecessary and I did not plan on creating more than one instance of it anyway.

The `generate` method is a good example of that. It's just a method to generate a random string, and it doesn't use `self` anywhere. But it's still in the class for some reason, because I thought everything had to be in a class.

I could probably rewrite it something like this:

{% highlight python %}
def create_pool(num=10, length=10):
    # Initializes pool with n strings
    pool = []
    for i in range(num):
        pool.append(generate(length))
    return pool

def generate(n):
    # Generates a random lowercase string of length n
    return "".join(chr(97 + randint(0, 25)) for j in range(n))

pool = create_pool()

{% endhighlight %}

One reason why I might still want to have this in a class (and I think this was my reasoning at the time) would be to make the API easier to handle. I wanted several different kinds of pools that would have different methods to generate strings and calculate their fitness, but the methods would fit together in the same way. So I figured I could make a `Genetic` class that implemented a basic version of the structure I wanted, and then make lots of classes that inherited from `Genetic`.

But I'm not really at that level of complexity with this project yet, so I think it's better to keep the code as simple as possible and only add in extra frills when I have to. I can just try and get one pool working first, and then try to add in more later.

That's all for now. I'll continue this in later posts as I rewrite the code.