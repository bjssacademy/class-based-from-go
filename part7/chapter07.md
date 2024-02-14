# Composition

We looked at interfaces in a [previous chapter](/part4/chapter04.md).

Interfaces are things that are implemented. When a class implements an interface, we can say it "is a" type.

So our `MP3Media` class "is a" `IMedia` type. In this way we can use anything that implements that interface exactly the same without knowing exactly what *concrete* type it is.

Composition is different again - composition is "has a". We build the object by providing parts.

> Interfaces and composition can be used together - it's not one or the other.

## An Example

Let's look at the concept of a car. There are many different makes and models, but every one of them "is a" car. They share common abilities:
- Accelerate()
- Brake()

They also share common properties, they "have a":
- Engine
- Wheels

And so on.

So we know if I have an "Academy 500 Roadster" (the car of choice for mid-life crises), then I know that like every other car in existence it will be able to accelerate and I will be able to apply the brakes.

I also know it will have properties, such as wheels, seats, engine, and many others.

Where it differs is that whilst there are things that are common to *all* cars (the contract, or interface), there are things that are common yet specific (what that car's specification is - eg. 1.6L engine, 25 inch chrome wheels, leather seats).

### My head hurst, can I see some code?

Let's try that with code. First create a new folder named `cars`, and add a new interface named `IEngine`.

```c#
public interface IEngine
{
    public Double EngineSize { get; }

}
```

> This is the first time we've had a *property* in an interface. You'll notice we only have the getter `get` in the accessor. This is because we cannot have our setter as `private set` in an interface, and we don't want it to be public. We have to do this in the implementation, which we will see in a bit.

Now create another interface called `ICar` :

```c#
public interface ICar
{
    public IEngine Engine { get; }

    public void Accelerate();
    public void Brake();
}
```

> Note that the property Engine has the data type IEngine. This means we can use *any* engine in this car, not just a specific engine...

Now create a class called `Academy500Roadster` that implements `ICar`:

```c#
public class Academy500Roadster : ICar
{
    public IEngine Engine { get; private set; }

    public Academy500Roadster(IEngine engine)
    {
        Engine = engine;
    }

    public void Accelerate()
    {
        throw new NotImplementedException();
    }

    public void Brake()
    {
        throw new NotImplementedException();
    }
}
```

> If you implemented the interface through the IDE, you'll have to replace the NotImplementedException. By convention the IDE will moan at you about undeclared properties and suggest you make them nullable. There is almost always a better way!

So here we have our specific type of car, and it has an Engine property which can only be set via the constructor. Imagine getting a new car and having to install the engine yourself. That's what we mean by the object being ready to use when it is first instantiated.

A reminder about constructors:

- special method that is called once
- when you create an object
- it has one job: get the object ready to be used by your app

Now let's create our concrete Engine instance. Create a class called `Engine1600` and make it implement `IEngine`:

```c#
public class Engine1600 : IEngine
{
    public double EngineSize { 
        get { return 1600.0D; } 
    }
}
```

> **Wait! What are you doing here?** Okay, I've cheated a bit, I know more C# than you, and I haven't fully explained getters and setters other than they are "magic".
>
> LINK TO GETTERS AND SETTERS

Right, so now we have our getter that provides a "canned" response every time it is called of 1600 as a double. There's no setter specified, so the Engine1600 class is ready to be used when it is instantiated. Good object, like it.

Now, finally, we can go back to our `Program.cs` file and build our car!

```c#
using academy_demo.cars;

IEngine engine = new Engine1600();

Academy500Roadster roadster = new Academy500Roadster(engine);

Console.WriteLine(roadster.Engine.EngineSize);
```

- `IEngine engine = new Engine1600();` Create a new instance of the Engine1600 object
- `Academy500Roadster roadster = new Academy500Roadster(engine);` Create a new instance of the Academy500Roadster with the engine provided
- `Console.WriteLine(roadster.Engine.EngineSize);` on the roadster object, access its Engine object property, and get the EngineSize property of the Engine object!

We now have a car, with a 1600CC engine in it! We have composed our car - we created the base, and then gave it the engine size we wanted for this particular model.

## Lab

The Academy500Roadster has two engine options, with an engine size of 1600 or 2000. 

Create a new concrete class for the new engine size, and create two Academy500Roadsters - one with a 1600 engine and one with a 2000 engine.

