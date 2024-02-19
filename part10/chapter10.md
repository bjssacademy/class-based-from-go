# Why Do We Make Cars This Way?

You may have noticed we construct our car by doing this:

```c#
BaseHeadlights headlights = new NormalHeadlights();
IEngine engine = new Engine1600();

Academy500Roadster roadster = new Academy500Roadster(engine, headlights);
```

We could however, change our constructor to be :

```c#
public Academy500Roadster()
{
    Engine = new Engine1600();
    Headlights = new NormalHeadlights();
}
```

And then we could construct a car without having to provide it any constructor parameters:

```c#
Academy500Roadster roadster = new Academy500Roadster();
```

Now it might seem like the latter is far easier to do - and it's a trap people fall into all the time when learning programming.

The problem this creates is of *tight coupling* - your instance of `Academy500Roadster` can **only ever have** a 1600CC engine and normal headlights. You cannot change the engine or the headlights.

If we wanted a 2000CC roadster we'd have to create another class (maybe Academy500Roadster2000) with different constructor code:

```c#
public Academy500Roadster2000()
{
    Engine = new Engine2000();
    Headlights = new NormalHeadlights();
}
```

And then implement all the code again we have in our original class - all because we changed the engine size. Whereas if we provide it in the constructor, the `Academy500Roadster` class represents only the concept of the model, not of the exact implementation detail.

Without getting into all the ins and outs, this violates the Single Responsibility Principle, or SRP. We want the `Academy500Roadster` to be our single class with the responsibility of being the car model, and the only place we have to Accelerate or Brake.

## Dependency Inversion Principle & Dependency Injection

What we've actually been doing up until now, is known as the Dependency Inversion Principle, or DIP, and it's one of the fundamental concepts in what's known as SOLID with OO programming.

>  DIP says: high-level modules should not depend on low-level modules but both should depend on abstractions.

We have been *injecting* instances into our class, that it *depends* on to be able to function, rather than letting the class decide what it needs to function. So we have achieved DIP by using the pattern known as **Dependency Injection**, or DI.

### What does DIP mean about depending on abstractions?

Interfaces - and abstract classes - are *abstractions*. They allow us to talk about any concrete implementations without ever needing to know about the *exact* implementation.

So we don't care if the engine is 1600CC, 2000CC, a hamster in a wheel, or a rubber band. We just care that there *is* an engine, and it has the methods and properties defined by the interface.

We don't care if headlights are neon, halogen, candles, or a small mob with burning torches. 

It allows us *not to care about the actual implementation*, only about the interface - what we can call on **any** headlight or engine object.

Trust us, this is going to be very, very important.

### So what's the difference between DIP and DI?

DIP is what we want to achieve, and is a **Design Principle**. It doesn't say *how* you should achieve it technically. To solve the problem we have used constructor injection on our dependencies - Dependency Injection, which is a **Design Pattern**.

So DIP is the *theory*, DI is the *practice*.

## My Head Hurts

Me too, me too. Nonetheless, without having to mention any of the above previously, this is what you *have* been doing, and it's vastly easier to start with good practices rather than try and unpick them later.

We've previously worked by knowing the functions we need to call. We've changed our paradigm to a more object-oriented approach - telling the object what to do for us rather than checking the object's state and deciding what to do next.

This is also known as the "Tell, Don't Ask" principle:

- Instead of asking an object for information about its state and then making decisions externally, code should *tell* objects what to do, allowing them to *encapsulate* their behaviour and make decisions based on their internal state or the context in which they operate.

- It promotes *encapsulation*, *reduces coupling*, and *improves the maintainability* of the code by putting behaviour within objects and minimizing external dependencies.

> That's...a lot of new words. Yep, and we've been doing it all along without knowing or using these (for the most part). We've done it by doing. It's much more fun.

This was a massive shift in software engineering in the 1990s, and it takes some time to understand why it's good and why people sometimes find it very hard to understand.

## SOLID

We've actually been doing some of Bob Martin's 5 SOLID principles:

- **S**ingle Responsibility Principle (SRP)

- **O**pen/Closed Principle (OCP)

- **L**iskov Substitution Principle (LSP)

- **I**nterface Segregation Principle (ISP)

- **D**ependency Inversion Principle (DIP)

This sounds really technical and quite intimidating. Let's look as some examples that ChatGPT helpfully created for me:


**Single Responsibility Principle (SRP):**

Imagine you have a toy robot. Its single responsibility is to walk forward. It shouldn't also be responsible for making sounds or shooting lasers. Each part of the robot should have one job.

> Our Academy500Roadster class has a single responsibility - it's the model of the car

**Open/Closed Principle (OCP):**

Think of a toy box. You can add new toys to the box without changing the box itself. The box is closed for changes, but open for new toys. So, you can add a new toy car or a new doll without needing to change the box.

> Our Academy500Roadster is a bit like this - we can't change the car itself, but we can change what it contains (different types of engines, headlights)

**Liskov Substitution Principle (LSP):**

Suppose you have a remote-controlled car. If you have two cars, a small one and a big one, you should be able to use the remote for the small car to control the big car as well. Both cars behave like cars when using the remote.

> ICar is the abstraction that allows us to know how all cars operate without having to known or care about the exact model

**Interface Segregation Principle (ISP):**

Imagine you have a remote control with many buttons. But you only need three buttons to control your toy car: forward, backward, and stop. You don't need all the other buttons, so they are unnecessary for your specific use.

> Not quite composition over inheritance, but it helps. This is breaking interfaces down and not trying to create massive, catch all interfaces. Imagine creating a ICar interface that had all the specifics for engine, headlights, brakes, and also for things that were specific to only a few cars, like ejector seats...

**Dependency Inversion Principle (DIP):**

Picture a robot factory. Instead of directly building robots inside the factory, the factory gets parts (like arms, legs, and heads) from different suppliers. This way, the factory can easily switch suppliers if needed without changing the way it builds robots.

> Oh look - just exactly how we build our car!

SOLID can look very, very complex when you first start out, and quite daunting. Don't worry, we're just glad you are here and learning about it now.