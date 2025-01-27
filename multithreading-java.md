# CPU
- CPU is responsible for executing instructions from programs. It performs basic arithmetic, logic, control and input/output operations specified by the instructions.
Eg. A modern CPU like Intel Core i7 or AMD Ryzen 7.

## Core
- A core is an individual processing unit within a CPU. Modern CPUs can have multiple cores, allowing them to perform multiple tasks simulataneously.
- CPU ke andar jo core hota hai, uske andar hi processing hoti hai.
- A quad-core processor has 4 cores, allowing it to perform four tasks simulataneously. For instance, one core could handle your web browser, another your music player, another a download manager, and another a background system update.

## Program
- A program is a set of instructions written in a programming language that tells the computer how to perform a specific task.

## Process
- A process is an **instance** of a program that is being executed. When a program runs, the OS creates a process to manage its execution. Eg. Microsoft Word is a program. When we open it, it becomes a process in the OS.

## Thread
- A thread is the smallest unit of execution within a process. A process can have multiple threads, which share the same resources but can run independently.

## Multitasking
- Multitasking allows an OS to run multiple processes simulateneously.
- On single-core CPUs, this is done through time-sharing, rapidly switching between tasks.
- On multi-core CPUs, true parallel execution occurs, with tasks distributed across cores. The OS scheduler balances the load, ensuring efficient and responsive system performance.

## Multithreading
- It refers to the ability to execute multiple threads within a single process concurrently.
- A web browser can use multithreading by having separate threads for rendering the page, running JS, and managing user inputs. This makes the browser more responsive and efficient.
- Multithreading enhances the efficiency of multitasking by breaking down individual tasks into smaller sub-tasks or threads. These threads can be processed simulataneously, making better use of the CPU's capabilities.

## Time Slicing 
- Time slicing divides CPU time into small intervals called time slices or quanta.
- Function: The OS scheduler allocates these time slices to different processes and threads, ensuring each gets a fair share of CPU time.
- Purpose: This prevents any single process or thread from monopolizing the CPU, improving responsiveness and enabling concurrent execution. 

## Context Switching
- It is the process of saving the state of a currently running process or thread and loading the state of the next one to be executed.
- Function: When a process or thread's time slice expires, the OS scheduler performs a context switch to move the CPU's focus to another process or thread.
- Purpose: This allows multiple processes and threads to share the CPU, giving the appearance of simultaneous execution on a single-core CPU or improving parallelism on multi-core CPUs.

Multitasking can be achieved through multithreading where each task is divided into threads that are managed concurrently.
While multitasking typically refers to the running of multiple applications, multithreading is more granular, dealing with multiple threads within the same application or process.

# How JVM handles multithreading?
- Java provides robust support for multithreading.
- Java's multithreading capabilities are part of the java.lang package, making it easy to implement concurrent execution.

- When a Java program starts, one thread begins running immediately, which is called the **main** thread. This thread is responsible for executing the main method of a program.

# Ways to Create Threads
2 ways:
1. Extend the Thread class
2. Implement the Runnable interface

## 1. Extend the Thread class
Lets say we want to make our thread say "World.
Then in App.java:
```
public class App {
    public static void main(String[] args) throws Exception {
        World world = new World();
        world.start();
        for(int i=0; i<100000; i++){
            System.out.println("Hello");
        }
    }
}
```

Then in World.java
```
public class World extends Thread {
    @Override
    public void run() {
        for(int i=0; i<100000; i++){
            System.out.println("World");
        }
    }
}
```
Now execution will occur in random order.
- Here, the **run** method is overriden to define the code that constitutes the new thread.
- **start** method is called to initate the new thread.

## 2. Implement the Runnable Interface

In World Class:
```
public class World implements Runnable {
    @Override
    public void run() {
        for(int i=0; i<100000; i++){
            System.out.println("World");
        }
    }
}
```

And in App.java:
```
public class App {
    public static void main(String[] args) throws Exception { 
        World world = new World();
        Thread t1 = new Thread(world);
        t1.start();
        for(int i=0; i<100000; i++){
            System.out.println("Hello");
        }
    }
}
```
Here, first we have to make an instance of World class, create an instance of Thread class, and then pass the world object into the Thread class. 
Then the start() method is called on the Thread object which will initiate the new thread.

- In both cases, the run method contains the code that will be executed in the new thread.

# Thread Lifecycle
1. New: A thread is in this state when it is created but not yet started.
2. Runnable: After the start method is called, the thread becomes runnable. It is ready to run and is waiting for CPU time.
3. Running: The thread is in this state when it is executing.
4. Blocked/Waiting: A thread is in this state when it is waiting for a resource or for another thread to perform an action.
5. Terminated: A thread is in this state when it has finished executing.

In main thread if we do t2.join(), then main thread will wait for t2 to get terminated, and only then it will execute the next line.

# When to use Extend Thread vs Runnable 
If a class A already extends class B, and we want to create a thread of A -> Then we will use Runnable because we can't extend A to B AS WELL AS Thread (multiple inheritance isn't allowed in Java).
