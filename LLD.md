# 1.SOLID Principle
- S = Single Responsibility Principle
- O = Open/Closed Principle
- L = Liskov Substitution Principle
- I = Interface Segmented Principle
- D = Dependency Inversion Principle

## S = Single Responsibility Principle
"A class should have only 1 reason to change" OR "It should have only one responsibilty"
Example: Lets say we have a class called Invoice, which has 3 methods, calculateTotal, printInvoice, saveToDB. Now, calculateTotal's logic could be changed if there are taxes introduced. printInvoice could be changed if I change the printing logic, saveToDB logic can be changed if now its supposed to be saved in files. So here many things are susceptibile to change. So the Invoice class is not following the S principle of SOLID. 
![image](https://github.com/user-attachments/assets/800bc346-da51-4206-b3f9-554fe5cfd853)

So what can instead be done is, we can split all these 3 different methods into 3 different classes: 
![image](https://github.com/user-attachments/assets/fb061298-a23a-4eff-9e78-ecbee1f91600)
![image](https://github.com/user-attachments/assets/fcaf798b-22bc-487d-8fb9-1e517582ffeb)
![image](https://github.com/user-attachments/assets/2ee2e515-1840-48c3-b356-decd6d162e1a)
With this, it is now easy to understand and maintain. 

## O = Open/Closed Principle
"Open for Extension but closed for Modification"
The class which has already been tested and is live, we should not modify them. Instead extend them if needed.
Wrong way: 
![image](https://github.com/user-attachments/assets/e00fa20e-c9f8-4220-9274-8d47ea988d58)
Correct way:
![image](https://github.com/user-attachments/assets/5ba68a43-3c82-48b6-ac93-1bac77a9666e)

## L = Liskov Substitution Principle
"If class B is a subtype of class A, then we should be able to replace object of A with B without breaking the behaviour of the program."
- Subclass should extend the capability of parent class not narrow it down.
- A ka child B hai. We should be able to replace A with B without breaking the behaviour of program.

Correct:
![image](https://github.com/user-attachments/assets/733855f1-93da-4718-b33b-c51ae6fba74e)
Wrong: This is wrong because here we are throwing exception (breaking the code) for already existing method of the parent class. 
![image](https://github.com/user-attachments/assets/70f978bc-7d49-4938-bbf6-b6e345bd48da)

## I = Interface Segmented Principle
"Interfaces should be such, that client should not implement unnecessary functions they do not need."
![image](https://github.com/user-attachments/assets/ba03a370-69ae-48b5-a793-2a8a341db6b0)
here, waiter class will have to implement all the methods of RestaurantEmployee such as washDishes(), serveCustomers(), and cookFood() even though washing dishes and cookFood is unnecessary for waiter. So rather, break the interfaces into many different parts so that we don't need to override every single useless method of the interface while implementing it.
![image](https://github.com/user-attachments/assets/b3ed6475-1ad4-406b-800e-4fbb66928cab)

## D = Dependency Inversion Principle
"Class should depend on interfaces rather than concrete classes."  
Wrong:
![image](https://github.com/user-attachments/assets/30ee4ba5-384f-4be9-b45f-326ee08c2298)
Correct:
![image](https://github.com/user-attachments/assets/f54c97be-7f3b-403a-ba48-adec765cd4fc)

# 1.1 Liskov Substitution Principle
If we have a parent class called Dad and it has three children c1, c2, c3, then for the following codes, the code should not break:  
```
Dad d1 = new c1();
Dad d2 = new c2();
Dad d3 = new c3();
```
Correct method se we will be able to utilise polymorphism.  
![Screenshot 2025-03-05 113609](https://github.com/user-attachments/assets/13c3f42f-95d2-4099-b4e5-d64587190213)
Here in the above picture, for the Bicycle class, because we are returning null for hasEngine() method, the code will break.  
Solution: In the parent class, put only very generic methods which every subclass would have. 

# 2. Strategy Design Pattern
![image](https://github.com/user-attachments/assets/9ad03c1b-c676-4ffa-bc45-6b6f02647205)  
is-a = Inheritance.  
has-a = Attribute in the class.

![image](https://github.com/user-attachments/assets/4378cea2-c956-4ae9-873d-2f883df24e37)
Here, Sporty Vehicle "is-a" Vehicle. Passenger Vehicle "is-a" Vehicle. Off Road Vehicle "is-a" Vehicle.  
Here, Off Road Vehicle and Sporty Vehicle have overriden their drive() method because they want some special capability.
- Now here, if the overriden drive() method of Sporty Vehicle and Off Road Vehicle is same, that would violate DRY principle.

To solve this problem:  
![image](https://github.com/user-attachments/assets/ab86d6c3-7d6e-424d-9911-44c9453692b1)  
Here, we did "Vehicle 'has-a' DriveStrategy".  
So now Vehicle will have an attribute: DriveStrategy obj; The type of this DriveStrategy will be decided by the child of the Vehicle.  
![image](https://github.com/user-attachments/assets/0292d3a9-b9ed-494a-9e71-c1a04b8f215c)  

```public class Main {
    public static void main(String[] args) {
        Vehicle bicycleObj = new Bicycle();
        bicycleObj.drive();
        Vehicle carObj = new Car();
        carObj.drive();
    }
}

class Vehicle {
    DriveStrategy driveStrat;
    public void greeting(){
        System.out.println("Nice to meet you!");
    }
    
    public Vehicle(){}
    
    public Vehicle(DriveStrategy obj){
        this.driveStrat = obj;
    }
    
    public void drive(){
        this.driveStrat.drive();
    }
}

class Bicycle extends Vehicle {
    public Bicycle (){
        super(new NormalDrive());
    }
}

class Bike extends Vehicle{
    public Bike (){
        super(new SpecialDrive());
    }
}

class Car extends Vehicle{
    public Car (){
        super(new SpecialDrive());
    }
}

interface DriveStrategy {
    void drive();
}

class NormalDrive implements DriveStrategy{
    public void drive(){
        System.out.println("This is normal drive");
    }
}

class SpecialDrive implements DriveStrategy{
    public void drive(){
        System.out.println("This is special drive");
    }
}
```

![image](https://github.com/user-attachments/assets/6180a661-b80d-4bc2-8132-cac6e64d9186)


# 3.Observer Design Pattern - Walmart Interview Question
Question: Implement "Notify Me" under a product (say iPhone) in Amazon website. We have to send a notification to all those that have clicked Notify Me whenever that product comes back for sale.
![image](https://github.com/user-attachments/assets/be4976c6-adc8-47f6-9062-d598f208a2a5)  

In this, we have two objects: Observable, and Observer.  
Whenever Observable's state changes, it will update all the Observers (there can be multiple observers)  
![image](https://github.com/user-attachments/assets/0a90354c-1b10-453c-b3e1-40b3cc853cdd)  

To implement this, we have Observable Interface.  
It has add(), remove(), notify(). And Observer Interface has update().  
- add() takes in an object of Observer and implies that ye object ko add karo. Hence this add is also called registration.

![image](https://github.com/user-attachments/assets/324c77b4-58e3-437c-b52a-0559fb77c145)

Now, there will be a Observable concrete class.  
![image](https://github.com/user-attachments/assets/45eef1ce-2cee-4a30-b2b3-0ff9c520f97a)
![image](https://github.com/user-attachments/assets/e26558b2-b077-4ff5-adfc-e66bc2090619)

Structure:  
![image](https://github.com/user-attachments/assets/6dcb7a3e-f7fa-4f7c-9505-31ed9d298f66)

Similarly, now we will create a concrete class for Observer. This will include update(){}
Now, there is a nuance here. There could be multiple Observable concrete classes; so how will our specific Observer Concrete Class know which Observable is trying to change it? In order to tackle this, we create a "has-a" relationship between that specific Observer Concrete Class and the Observable Concrete class which is trying to change it.  

![image](https://github.com/user-attachments/assets/70d3ccad-a165-4316-8de7-e020363818fd)  

Structure now:       
![image](https://github.com/user-attachments/assets/472e1480-97fc-4e0b-8e35-97e3cb42f9f6)

Example:  
There is a weather station who's job is to set the current temperature every 5 minutes. This current temperature (Observable) is being observed by:  
- TV Display (Observer)
- Mobile Display (Observer)

For this, let us first create a WeatherStation interface: WSObservable
```
public class Main {
    public static void main(String[] args) {
    }
}

interface WSObservable{
    public void add(DisplayObserver obj);
    public void remove(DisplayObserver obj);
    public void notifyObservers();
    public void setTemp(int newTemp);
    public int getTemp();
}

interface DisplayObserver{
    public void update();
}

class MobileDisplayObserver implements DisplayObserver{
    WSObservable ws;
    MobileDisplayObserver(WSObservable obj){
        this.ws = obj;
    }
    public void update(){
        
    }
}


class TVDisplayObserver implements DisplayObserver{
    WSObservable ws;
    TVDisplayObserver(WSObservable obj){
        this.ws = obj;
    }
    public void update(){
        
    }
}
class WSObservableImpl implements WSObservable{
    int temp;
    List<DisplayObserver> displayList = new ArrayList<>();
    public void add(DisplayObserver obj){
        displayList.add(obj);
    }
    public void remove(DisplayObserver obj){
        displayList.remove(obj);
    }
    public void notifyObservers(){
        for(DisplayObserver obj : displayList){
            obj.update();
        }
    }
    public void setTemp(int newTemp){
        if(newTemp!=this.temp){
            this.temp = newTemp;
            notifyObservers();        
        }
    }
    public int getTemp(){
        return this.temp;
    }
}

```
Now, let us attempt the "Notify Me" question:  
```
public class Main {
    public static void main(String[] args) {
        StocksObservable iPhones = new IphoneObservableImpl();
        NotificationAlertObserver observer1 = new EmailAlertObserverImpl("abc1@gmail.com", iPhones);
        NotificationAlertObserver observer2 = new EmailAlertObserverImpl("abc2@gmail.com", iPhones);
        NotificationAlertObserver observer3 = new EmailAlertObserverImpl("abc3@gmail.com", iPhones);
        NotificationAlertObserver observer4 = new MobileAlertObserverImpl("adityasangani21", iPhones);
        iPhones.add(observer1);
        iPhones.add(observer2);
        iPhones.add(observer3);
        iPhones.add(observer4);
        iPhones.setStockCount(20);
    }
}

interface StocksObservable{
    public void add(NotificationAlertObserver ea);
    public void remove(NotificationAlertObserver ea);
    public void notifyObservers();
    public void setStockCount(int stockCount);
    public int getStockCount();
}

interface NotificationAlertObserver{
    public void update();
}

class EmailAlertObserverImpl implements NotificationAlertObserver{
    String emailId;
    StocksObservable so;
    EmailAlertObserverImpl(String emailId, StocksObservable obj){
        this.so = obj;
        this.emailId = emailId;
    }
    public void update(){
        sendEmail(emailId, "product is in stock, hurry up!");
    }
    
    void sendEmail(String emailId, String msg){
        System.out.println("mail sent to " + emailId);
    }
}

class MobileAlertObserverImpl implements NotificationAlertObserver{
    String username;
    StocksObservable so;
    MobileAlertObserverImpl(String username, StocksObservable obj){
        this.so = obj;
        this.username = username;
    }
    public void update(){
        sendMsgOnMobile(username, "product is in stock, hurry up!");
    }
    
    void sendMsgOnMobile(String username, String msg){
        System.out.println("mail sent to " + username);
    }
}

class IphoneObservableImpl implements StocksObservable{
    int stockCount = 0;
    List<NotificationAlertObserver> emailList = new ArrayList<>();
    public void add(NotificationAlertObserver ea){
        this.emailList.add(ea);
    }
    public void remove(NotificationAlertObserver ea){
        this.emailList.remove(ea);
    }
    public void notifyObservers(){
        for(NotificationAlertObserver ea : emailList){
            ea.update();
        }
    }
    public void setStockCount(int newStockCount){
        if(stockCount==0){
            notifyObservers();
        }
        stockCount= this.stockCount+newStockCount;
    }
    public int getStockCount(){
        return stockCount;
    }
}
```
