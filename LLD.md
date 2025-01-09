## SOLID Principle
- S = Single Responsibility Principle
- O = Open/Closed Principle
- L = Liskov Substitution Principle
- I = Interface Segmented Principle
- D = Dependency Inversion Principle

### S = Single Responsibility Principle
"A class should have only 1 reason to change" OR "It should have only one responsibilty"
Example: Lets say we have a class called Invoice, which has 3 methods, calculateTotal, printInvoice, saveToDB. Now, calculateTotal's logic could be changed if there are taxes introduced. printInvoice could be changed if I change the printing logic, saveToDB logic can be changed if now its supposed to be saved in files. So here many things are susceptibile to change. So the Invoice class is not following the S principle of SOLID. 
![image](https://github.com/user-attachments/assets/800bc346-da51-4206-b3f9-554fe5cfd853)

So what can instead be done is: 
![image](https://github.com/user-attachments/assets/fb061298-a23a-4eff-9e78-ecbee1f91600)
![image](https://github.com/user-attachments/assets/fcaf798b-22bc-487d-8fb9-1e517582ffeb)
![image](https://github.com/user-attachments/assets/2ee2e515-1840-48c3-b356-decd6d162e1a)
With this, it is now easy to understand and maintain. 

### O = Open/Closed Principle
"Open for Extension but closed for Modification"
The class which has already been tested and is live, we should not modify them. Instead extend them if needed.
Wrong way:
![image](https://github.com/user-attachments/assets/e00fa20e-c9f8-4220-9274-8d47ea988d58)
Correct way:
![image](https://github.com/user-attachments/assets/5ba68a43-3c82-48b6-ac93-1bac77a9666e)

### L = Liskov Substitution Principle
"If class B is a subtype of class A, then we should be able to replace object of A with B without breaking the behaviour of the program."
- Subclass should extend the capability of parent class not narrow it down.
- A ka child B hai. We should be able to replace A with B without breaking the behaviour of program.

Correct:
![image](https://github.com/user-attachments/assets/733855f1-93da-4718-b33b-c51ae6fba74e)
Wrong:
![image](https://github.com/user-attachments/assets/70f978bc-7d49-4938-bbf6-b6e345bd48da)

### I = Interface Segmented Principle
"Interfaces should be such, that client should not implement unnecessary functions they do not need."
![image](https://github.com/user-attachments/assets/ba03a370-69ae-48b5-a793-2a8a341db6b0)
here washing dishes and cookFood is unnecessary for waiter. So rather, break them into small pieces.
![image](https://github.com/user-attachments/assets/b3ed6475-1ad4-406b-800e-4fbb66928cab)

### D = Dependency Inversion Principle
"Class should depend on interfaces rather than concrete classes."
Wrong:
![image](https://github.com/user-attachments/assets/30ee4ba5-384f-4be9-b45f-326ee08c2298)
Correct:
![image](https://github.com/user-attachments/assets/f54c97be-7f3b-403a-ba48-adec765cd4fc)

