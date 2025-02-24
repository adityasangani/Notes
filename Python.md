### Inheritance
Lets say you have a class called Person.
```
class Person:
    def __init__(self, fname, lname):
        self.firstname = fname
        self.lastname = lname
    def __str__(self):
        return f"{self.firstname} {self.lastname}"
    def printname(self):
        print(self.firstname, self.lastname)
p1 = Person("John", "Stokes")
p1.printname()
```
Now lets say you want to create a child class of Person, called Student.
Do it like this: 
```
class Student(Person):
    pass
```
Now, here we can also add __init__() function to Student class. However, note that he child's __init__() function overrides the inheritance of the parent's __init__() function.

#### super() function
The super() function explicitly calls a method from the parent class. This is useful when:
- You want to extend or modify the behavior of a parent method rather than completely overriding it. Example: 
```
class Parent:
    def greet(self):
        print("Hello from Parent!")

class Child(Parent):
    def greet(self):
        super().greet()  # Calls the parent class's greet method
        print("Hello from Child!")

obj = Child()
obj.greet()
```

### File Handling in Python
4 different modes:
- "r" - Read. Default value. Opens a file for reading, error if file does not exist.
- "a" - Append. Opens a file for appending, creates the file if it does not exist.
- "w" - Write. Opens a file for writing, creates the file if it does not exist. 
- "x" - Create. Creates the specified file, returns an error if the file exists.

Additionally, 
  - "t" - Text. Default value. Text mode
  - "b" - Binary. Binary mode (e.g. images)

  Now, to open a file for reading:
  f = open("demofile.txt")   ----> equivalent to f = open("demofile.txt", "rt")

  The open() function returns a file object, which has a read() method for reading the content of the file:
  print(f.read())

  #### Reading parts of a file
  1. Specific number of characters: print(f.read(5))
  2. Return one line: print(f.readline())
  3. Looping through the file: for x in f: print(x)
Always close your files: f.close()

#### Writing
Overwrites any existing content.
f.write("dfskf")

#### Delete a file
import os
os.remove("demofile.txt")

- Check if file exists:
  ```
  import os
  if os.path.exists("demofile.txt"):
    os.remove("demofile.txt")
  else:
    print("The file does not exist")
  ```
- Delete Folder
  import os
  os.rmdir("myfolder")
Note: You can only remove empty folders.

### Multithreading
- Doing multiple tasks at a time.
- Thread is a lightweight process.
```
import threading
import time
import random
import queue
 
buffer = queue.Queue(maxsize=10)
 
num_items = 20
 
def producer():
    for _ in range(num_items):  # for i in range(20):
        item = random.randint(50, 100) # will create an integer number from 50 to 100
        buffer.put(item)
        print(f"The Produced Item is: {item}")
        time.sleep(random.uniform(1, 2)) # floating numbers between 1 and 2. Thread will sleep for that many seconds
 
    for _ in range(5):
        buffer.put(None)
 
    print("Producer has finished producing")
 
def consumer():
    while True:
        item = buffer.get()
        if item is None:
            break
        print(f"The Consumed Item is: {item}")
        time.sleep(random.uniform(30, 60))
 
# Creating the threads
producer_thread = threading.Thread(target=producer)
 
consumer_thread1 = threading.Thread(target=consumer)
consumer_thread2 = threading.Thread(target=consumer)
consumer_thread3 = threading.Thread(target=consumer)
consumer_thread4 = threading.Thread(target=consumer)
 
# Starting the threads
producer_thread.start()
 
consumer_thread1.start()
consumer_thread2.start()
consumer_thread3.start()
consumer_thread4.start()
 
# Waiting for the threads to finish
producer_thread.join()
 
consumer_thread1.join()
consumer_thread2.join()
consumer_thread3.join()
consumer_thread4.join()
 
print("All threads have finished Execution")
    
```



