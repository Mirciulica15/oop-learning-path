# Singleton Pattern

- A **Creational** design pattern
- It ensures **only one instance** of its kind exists
- Provides a **single point of access** to it

## Analogy

- The president of the goverment of a country is a **global identifier**

![SingletonAnalogy](https://github.com/Mirciulica15/oop-learning-path/assets/36898665/11a8c058-9663-4b64-900f-f0e55cf87e22)

## How Singleton works

- Singleton lets you access your object from **anywhere** in your application (like a global variable)
- It encapsulates the attributes of this object in **one single class**
- Guarantee that **only one instance** of this class will be available at any point in time

## Why use Singleton?

![DatabaseSingleton](https://github.com/Mirciulica15/oop-learning-path/assets/36898665/5fd01cf9-5afa-4788-a156-208161be5d29)

- Only one instance of a database is required
- **Reuse** the same instance every time you access the tables
- You **avoid** possible errors and **conflicts**

## Implementation

- Thread-unsafe

![ThreadUnsafeImplementation](https://github.com/Mirciulica15/oop-learning-path/assets/36898665/07ea1909-86ff-40df-af29-02a70fbc02bb)

- Thread-safe, but every thread has to wait for the lock

![ThreadSafe](https://github.com/Mirciulica15/oop-learning-path/assets/36898665/bfec95a0-af02-48fa-a20f-2622487da9e2)

- Thread-safe, non-blocking, but can still fail

![ThreadSafeImproved](https://github.com/Mirciulica15/oop-learning-path/assets/36898665/a7af6f0a-dcfb-4fad-964d-586e78a91437)

- Why can the previous implementation fail? Because of **partially constructured object references**

![ThreadSafePartialObjectSafe](https://github.com/Mirciulica15/oop-learning-path/assets/36898665/d1ac0a70-85f9-4db8-a0fd-8325418262d2)

- Thread-safe, non-blocking and deals with **partially constructured object references**

![VolatileVariableAccess](https://github.com/Mirciulica15/oop-learning-path/assets/36898665/bc67418e-2b87-4c1d-affe-63bf15d331d0)

- Still, one important aspect regarding **volatile** variables it that they aren't cached, but retrieved directly from main memory (a costly operation)
- In the previous code, we access the volatile variable **twice**, once in the first ```if``` statement, and then in the ```return``` statement:
- Therefore, we can further **improve** the implementation by using a local variable ```result``` and only accessing the volatile variable from memory **once**

![FinalOptimizedVersion](https://github.com/Mirciulica15/oop-learning-path/assets/36898665/10b2910b-e121-4467-9c89-cb9f8eda2773)

## Summary

- Singleton should be used when you must have a **single instance** available
- It disables all means of creating objects of a class except for the **special static creation method**
- The method returns the **existing instance** if it has already been created
- The code needs to be adapted to handle **multiple threads**

## Code

```java

public class Singleton {

    private static volatile Singleton instance;
    private String data;
  
    private Singleton(String data) {
        this.data = data;
    }
  
    public static Singleton getInstance(String data) {
        Singleton result = instance;
        if(result == null) {
            synchronized(Singleton.class) {
                result = instance;
                if(result == null) {
                    instance = result = new Singleton(data);
                }
            }
        }
        return result;
    }
}

```
