---
title: "The Singleton and Thread-Safe Implementations for Multithreaded Applications"
date: 2022-11-07
draft: false
tags: [
    'Software Design',
    'Design Patterns',
    '.NET',
    'C Sharp'
]
resources:
- name: "featured-image-preview"
  src: "class-diagram-flat.png"
---

During my time as an undergrad studying CS, I took an upper level course covering software design. It was here that I gained a passion for design patterns and principles while implementing them in Java. But design patterns aren't limited to a single language. They are, by definition, patterns of solutions to commonly recurring design problems in the neverending world of software development - and thus, apply to any programming language, paradigm, or framework. And so this post will be written in C# with some implementations that relate to thread-safety.

The course covered many different patterns and principles but I'll be covering only one in this post as the first of many.

# Intuition

When building robust software, sometimes it may be necessary (and cleanest) to only have a single instance of a class. Think of if you're running a poker game and you had two dealers that had to share information between eachother before dealing every card. 

- Dealer 1: "Hey, I want to give Player 1 a card, have you given him one yet?"
- Dealer 2: "No, go ahead please let me go home we don't need two people here."

Not only would a game like this run unbelievably slowly, but there could be a number of bugs and race conditions that arise if even one dealer miscommunicates, doesn't act in time, or performs some operation without the other dealer knowing.

# The Singleton

Enter, the Singleton. Probably, one of the most commonly used patterns when designing software systems. Unsurprisingly, the Singleton is a creational design pattern that ensures only a *single* instance of a class exists and provides a *single* point of access to this instance throughout the entirety of the code.

Below is the class diagram that shows the basic flow of a naive implementation.

<br>

![The Singleton](class-diagram-flat.png "The Singleton - Class Diagram (Original)")

I'll denote the caller as the `Client` to represent an arbitrary client's request for a Singleton instance. The Singleton class has a static member `_instance` that holds the reference to the Singleton, a private constructor to ensure no external creation of another Singleton (ie. Dealer 2), and a public method that returns the Singleton `_instance`.

The naive implementation (above class) is as follows:

```
namespace NaiveExample
{
    public sealed class Singleton 
    {
        private Singleton() {}
        private static Singleton _instance;

        public static Singleton GetInstance() 
        {
            if (_instance == null)
                _instance = new Singleton();

            return _instance;
        }

        public void SomeAction() 
        {
            // ...
        }
    }
}
```

# Thread-Safety & Thread-Safe Singletons

The above implementation should be good for most single-threaded applications. But how does it perform when using multiple threads?

Unfortunately, not so well. In a multithreaded environment you can have the same creation method `GetInstance()` called by multiple threads resulting in multiple instances of your Singleton (ie. Again having multiple dealers).

An easy safeguard would be to lock the thread that arrives first, initialize the instance of the Singleton, and unlock the thread for the others. 

Let's now define a static object called `_lock` that is acting as the shared object to check if the thread is unlocked. When retrieving the instance, we'll first acquire the mutual-exclusion lock on the object, go through our creation logic, and release the lock after we're done. As mentioned in the docs, any other thread is blocked from acquiring the lock and waits until the lock is release. Therefore, this implementation is thread-safe.

```
namespace ThreadSafeExample
{
    public sealed class SimpleThreadSafeSingleton
    {
        private SimpleThreadSafeSingleton() {}
        private static SimpleThreadSafeSingleton _instance;
        private static readonly object _lock = new object();

        public static SimpleThreadSafeSingleton GetInstance()
        {
            lock (_lock)
            {
                if (_instance == null) 
                    _instance = new SimpleThreadSafeSingleton()
                
                return _instance;
            }
        }

        public void SomeAction() 
        {
            // ...
        }
    }
}
```

Great! We have a thread-safe implementation of the Singleton, but what are the downsides?

Looking at the code, you'll see that we're acquiring a lock every time we request an instance of the Singleton which can result in performance losses.

This leads us to the next iteration, **double-check locking**.

```
namespace DoubleCheckLockingExample
{
    public sealed class DoubleCheckLockingSingleton
    {
        private DoubleCheckLockingSingleton() {}
        private static DoubleCheckLockingSingleton _instance;
        private static readonly object _lock = new object();

        public static DoubleCheckLockingSingleton GetInstance()
        {
            if (_instance == null) 
            {
                lock (_lock)
                {
                    if (_instance == null)
                        _instance = new DoubleCheckLockingSingleton()
                }
            }

            return _instance;
        }

        public void SomeAction() 
        {
            // ...
        }
    }
}
```

As the name suggests, with double-check locking we perform two checks. A check to see if the Singleton has been initialized, and then the logic from our thread-safe implementation. However, we no longer need to acquire a lock for every request of the instance - only upon initialization.

It's been noted however that this implementation is error-prone and can be open to performance/correctness issues when making significant changes.

This leads us to Jon Skeet's implementation which achieves thread-safety without using locks.

```
namespace NoLockExample
{
    public sealed class NoLockThreadSafeSingleton
    {
        private static readonly NoLockThreadSafeSingleton _instance = new NoLockThreadSafeSingleton();

        static NoLockThreadSafeSingleton() {}
        private NoLockThreadSafeSingleton() {}
        
        public static NoLockThreadSafeSingleton GetInstance()
        {
            return _instance;
        }

        public void DoAction()
        {
            // ...
        }
    }
}
```

#### How is this thread-safe?

Static constructors in C# are only executed when an instance of the class is created or a static member of the class is referenced. Importantly, they're also executed only **once** per AppDomain resulting in thread-safety with the above implementation.

The laziness of type initializers is only guaranteed by .NET when the type isn't marked with a special flag called `beforefieldinit`. (Skeet, 2019) However, the C# compiler applies this flag to all types that aren't given a static constructor.

# Conclusion

There are many ways to implement thread-safety along with the Singleton design pattern in C#, each with their distinct pros and cons. A large part of designing robust software systems is being able to balance these pros and cons effectively, while applying the most **pertinent** implementation for your needs.

Although I've used a poker game as a less technical example of when the Singleton might shine, in real-world applications the pattern is commonly used to implement Loggers / Log Managers, Configuration Managers, Database Connections, and so much more.

Despite speeding through some code I hope this brief explanation was of some value. Feel free to reach out if there's anything I've missed, anything that would be important to include, or even if you're having trouble understanding the post.

# Further Reading

For anyone interested in diving deeper into the C# language, I highly suggest taking a look at Skeet & Lippert's textbook [C# in Depth, Fourth Edition](https://www.manning.com/books/c-sharp-in-depth-fourth-edition?utm_source=jonskeet&utm_medium=affiliate&utm_campaign=book_skeet5_csharp_3_8_19&a_aid=jonskeet&a_bid=569211b4) to which I'll add a link to :)

# References

- BillWagner. (n.d.). Lock statement - C# reference. lock statement - C# reference | Microsoft Learn. Retrieved from https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/lock 
- Skeet, J., &amp; Lippert, E. (2019). C# in depth. Manning Publications Co.