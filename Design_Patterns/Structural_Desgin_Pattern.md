# Structural design patterns

Structural design patterns are design patterns that deal with object composition and class relationships. These patterns are used to create relationships between classes and objects to form larger structures that are more complex and versatile than the individual components.

There are several types of structural design patterns, including:

Adapter Pattern - This pattern allows the interface of an existing class to be used as another interface. It is used when the client expects a different interface than what the object can provide.

Bridge Pattern - This pattern separates the abstraction from its implementation so that the two can be varied independently. It is used when we want to decouple an abstraction from its implementation so that both can be modified independently.

Composite Pattern - This pattern composes objects into tree structures to represent part-whole hierarchies. It is used when we want to create a structure of objects where each object can have its own children.

Decorator Pattern - This pattern adds behavior to an individual object, dynamically, without affecting other objects. It is used when we want to add functionality to an object without changing its interface.

Facade Pattern - This pattern provides a simple interface to a complex system. It is used when we want to simplify the interaction with a complex system by providing a simpler interface.

Flyweight Pattern - This pattern reduces the number of objects created by sharing objects between multiple contexts. It is used when we want to reduce memory usage by sharing common parts of objects.

Proxy Pattern - This pattern provides a surrogate or placeholder object to control access to another object. It is used when we want to control access to an object or to add additional functionality to it.

Each of these patterns has its own specific use cases and benefits, and they can be combined to create more complex and versatile software architectures.



## Adapter pattern is a structural design pattern that allows the interface of an existing class to be used as another interface. This pattern converts the interface of one class into another interface that the client expects. This allows classes with incompatible interfaces to work together.

Here's an example of how to implement the adapter pattern in Java:

Let's say we have an existing class Rectangle with its own interface:




    public class Rectangle {
    public void drawRectangle(int x, int y, int width, int height) {
        System.out.println("Rectangle drawn at (" + x + ", " + y + ") with width " + width + " and         height " + height);
    }
     }
     
     
 We also have a new client code that expects a different interface for drawing shapes:

     public interface Shape {
    void draw(int x, int y, int width, int height);
   }
   
   
   
   
 We can create an adapter class RectangleAdapter that implements the Shape interface and adapts the draw() method to call the drawRectangle() method of the Rectangle class:
 
 
     public class RectangleAdapter implements Shape {
     private Rectangle rectangle;

    public RectangleAdapter(Rectangle rectangle) {
        this.rectangle = rectangle;
    }

    @Override
    public void draw(int x, int y, int width, int height) {
        rectangle.drawRectangle(x, y, width, height);
    }
   }
   
   
   
   Now, we can use the RectangleAdapter to draw a rectangle using the Shape interface:

     public class Main {
    public static void main(String[] args) {
        Shape shape = new RectangleAdapter(new Rectangle());
        shape.draw(10, 20, 30, 40);
    }
    }
    
    
    
    
    Output:

    Rectangle drawn at (10, 20) with width 30 and height 40
 
 
 
## The Bridge pattern is a structural design pattern that decouples an abstraction from its implementation so that the two can vary independently. This pattern involves an abstraction (an interface or abstract class) and an implementation that can be changed at runtime without affecting the client code.

Here's an example of how to implement the Bridge pattern in Java:

Let's say we have an abstraction interface Shape:


    public interface Shape {
    void draw();
    }
    
    
  We also have two concrete implementations of Shape, CircleShape and SquareShape:

     public class CircleShape implements Shape {
        @Override
        public void draw() {
        System.out.println("Drawing Circle");
       }
    }

    public class SquareShape implements Shape {
       @Override
      public void draw() {
        System.out.println("Drawing Square");
       }
     }
  
  
  
  We also have an interface Renderer that defines the implementation methods:

     public interface Renderer {
    void renderShape();
     }


We can create two concrete implementations of Renderer, VectorRenderer and RasterRenderer:
 
 
    public class VectorRenderer implements Renderer {
    @Override
    public void renderShape() {
        System.out.println("Rendering shape in vector format");
    }
   }

     public class RasterRenderer implements Renderer {
    @Override
    public void renderShape() {
        System.out.println("Rendering shape in raster format");
    }
   }
   
   
   
Now, we can create a Shape abstraction that takes a Renderer implementation as a constructor argument:


    public abstract class Shape {
    protected Renderer renderer;

    public Shape(Renderer renderer) {
        this.renderer = renderer;
    }

    public abstract void draw();
     }
     
     
We can create two concrete implementations of Shape, Circle and Square, that use the Renderer implementation passed in their constructor:

     public class Circle extends Shape {
    public Circle(Renderer renderer) {
        super(renderer);
    }

    @Override
    public void draw() {
        System.out.print("Circle: ");
        renderer.renderShape();
    }
     }

     public class Square extends Shape {
    public Square(Renderer renderer) {
        super(renderer);
    }

    @Override
    public void draw() {
        System.out.print("Square: ");
        renderer.renderShape();
    }
     }
Finally, we can use the Shape abstraction and the Renderer implementations to draw shapes in different formats:


       public class Main {
        public static void main(String[] args) {
        Renderer vectorRenderer = new VectorRenderer();
        Renderer rasterRenderer = new RasterRenderer();

        Shape circle = new Circle(vectorRenderer);
        Shape square = new Square(rasterRenderer);

        circle.draw();
        square.draw();
    }
    }

  
 
   
 
  Output:

     Circle: Rendering shape in vector format
     Square: Rendering shape in raster format
     
     
## The Composite pattern is a structural design pattern that allows you to compose objects into tree structures to represent part-whole hierarchies. This pattern allows you to treat individual objects and compositions of objects uniformly.

Here's an example of how to implement the Composite pattern in Java:

Let's say we have a base component interface Employee:



     public interface Employee {
    void showDetails();
    }
    
    
  We also have two concrete implementations of Employee, Developer and Manager:

     public class Developer implements Employee {
    private String name;
    private String position;

    public Developer(String name, String position) {
        this.name = name;
        this.position = position;
    }

    @Override
    public void showDetails() {
        System.out.println("Name: " + name + ", Position: " + position);
    }
    }

     public class Manager implements Employee {
    private String name;
    private String position;

    public Manager(String name, String position) {
        this.name = name;
        this.position = position;
    }

    @Override
    public void showDetails() {
        System.out.println("Name: " + name + ", Position: " + position);
    }
   }
   
   
 We can create a composite class Team that implements the Employee interface and contains a list of Employee objects:
 
 
     import java.util.ArrayList;
     import java.util.List;
 
     public class Team implements Employee {
      private List<Employee> employees = new ArrayList<>();

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    public void removeEmployee(Employee employee) {
        employees.remove(employee);
    }

    @Override
    public void showDetails() {
        for (Employee employee : employees) {
            employee.showDetails();
        }
    }
     }
     
     
 Now, we can create a team of developers and managers using the Team class:

     public class Main {
    public static void main(String[] args) {
        Developer dev1 = new Developer("John Doe", "Java Developer");
        Developer dev2 = new Developer("Jane Doe", "Python Developer");

        Manager manager1 = new Manager("Jack Smith", "IT Manager");

        Team team = new Team();
        team.addEmployee(dev1);
        team.addEmployee(dev2);
        team.addEmployee(manager1);

        team.showDetails();
    }
     }
   
   
   
   Output:

     Name: John Doe, Position: Java Developer
     Name: Jane Doe, Position: Python Developer
     Name: Jack Smith, Position: IT Manager
     
     
     
   In this example, we have used the Composite pattern to create a tree structure of employees, where a Team can contain other Employee objects. This allows us to treat individual employees and teams of employees uniformly. We can easily add or remove employees from the team and show their details without knowing whether they are individual employees or part of a team.
   
   
   
   
## The Decorator pattern is a structural design pattern that allows you to add functionality to an object dynamically at runtime by wrapping it in an object of a decorator class that has the same interface as the original object.

Here's an example of how to implement the Decorator pattern in Java:

Let's say we have a base interface Pizza:
 
 
    public interface Pizza {
    String getDescription();
    double getCost();
     }
     
     
     
We also have a concrete implementation of Pizza, PlainPizza:


     public class PlainPizza implements Pizza {
    @Override
    public String getDescription() {
        return "Thin dough";
    }

    @Override
    public double getCost() {
        return 4.00;
    }
     }
     
     
   Now, we can create decorator classes that wrap the Pizza object and add additional functionality. For example, let's create a Mozzarella decorator:
   
   
   
     public class Mozzarella extends PizzaDecorator {
    public Mozzarella(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + ", mozzarella";
    }

    @Override
    public double getCost() {
        return pizza.getCost() + 0.50;
    }
    }
    
    
    
    
 The Mozzarella decorator wraps a Pizza object and adds mozzarella cheese to it. It also overrides the getDescription() and getCost() methods to return the description and cost of the pizza with the added mozzarella cheese.

We can create more decorator classes, like TomatoSauce and Mushrooms, that wrap a Pizza object and add tomato sauce and mushrooms to it, respectively.

Now, we can create a PlainPizza object and wrap it in decorator objects to create a pizza with mozzarella cheese, tomato sauce, and mushrooms:
   
      
    public class Main {
    public static void main(String[] args) {
        Pizza pizza = new Mozzarella(new TomatoSauce(new Mushrooms(new PlainPizza())));
        System.out.println(pizza.getDescription() + " - $" + pizza.getCost());
    }
   }


     Output:  Thin dough, tomato sauce, mushrooms, mozzarella - $5.50
 
 
    
In this example, we have used the Decorator pattern to add functionality to a Pizza object dynamically at runtime. We have created decorator classes that wrap a Pizza object and add additional functionality, like cheese, tomato sauce, and mushrooms, to it. This allows us to create a pizza with different toppings by wrapping a PlainPizza object in different decorator objects.




## The Facade pattern is a structural design pattern that provides a simplified interface to a complex system of classes, making it easier to use and understand.

Here's an example of how to implement the Facade pattern in Java:

Let's say we have a complex system of classes that we want to simplify using the Facade pattern. The system consists of three classes: CPU, Memory, and HardDrive. These classes work together to execute computer programs, but using them directly can be complicated.
  
  
     public class CPU {
    public void freeze() { /* ... */ }
    public void jump(long position) { /* ... */ }
    public void execute() { /* ... */ }
    }

     public class Memory {
    public void load(long position, byte[] data) { /* ... */ }
      }
  
     public class HardDrive {
    public byte[] read(long lba, int size) { /* ... */ }
       }
  
  
  
 We can create a Facade class, Computer, that simplifies the use of the system by providing a high-level interface to the classes in the system:
 
 
     public class Computer {
    private final CPU cpu;
    private final Memory memory;
    private final HardDrive hardDrive;

    public Computer() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }

    public void startComputer() {
        cpu.freeze();
        memory.load(BOOT_ADDRESS, hardDrive.read(BOOT_SECTOR, SECTOR_SIZE));
        cpu.jump(BOOT_ADDRESS);
        cpu.execute();
    }
      }
      
      
      
      
 The Computer class wraps the CPU, Memory, and HardDrive classes and provides a simplified interface to them. In this example, the startComputer() method simplifies the process of starting the computer by executing a sequence of instructions. These instructions are implemented using the methods of the CPU, Memory, and HardDrive classes.

Now, instead of using the CPU, Memory, and HardDrive classes directly, we can use the Computer class to start the computer:
 
 
 
     public class Main {
    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.startComputer();
    }
    }
    
    
    
  In this example, we have used the Facade pattern to simplify the use of a complex system of classes by providing a high-level interface to them. The Computer class acts as a facade for the CPU, Memory, and HardDrive classes, making it easier to use and understand the system
 
 
 
 
 
 
## The Flyweight pattern is a structural design pattern that allows sharing objects to support large numbers of fine-grained objects efficiently.

Here's an example of how to implement the Flyweight pattern in Java:

Let's say we have a drawing application that allows the user to create shapes. Each shape has a color, size, and position. Instead of creating a new object for each shape, we can reuse the same object for shapes with the same color.

First, we create a Shape interface that defines the properties and methods that all shapes should have:



    public interface Shape {
    void draw(int x, int y, String color);
     }
     
  Next, we create a Circle class that implements the Shape interface:

     public class Circle implements Shape {
    private final String color;
    private int radius;

    public Circle(String color) {
        this.color = color;
    }

    public void setRadius(int radius) {
        this.radius = radius;
    }

    @Override
    public void draw(int x, int y, String color) {
        System.out.printf("Drawing a %s circle with radius %d at (%d, %d)%n", color, radius, x, y);
    }
       }
       
       
       
       
       
The Circle class has a color field that is set in the constructor and a radius field that can be set using a setter method. The draw() method takes the position and color of the circle as arguments and prints a message to the console.

Next, we create a ShapeFactory class that creates and manages Circle objects:



    import java.util.HashMap;
     import java.util.Map;

     public class ShapeFactory {
    private final Map<String, Circle> circles = new HashMap<>();

    public Circle getCircle(String color) {
        Circle circle = circles.get(color);
        if (circle == null) {
            circle = new Circle(color);
            circles.put(color, circle);
        }
        return circle;
    }
     }
     
     
     
The ShapeFactory class has a circles field that is a map of Circle objects, keyed by their color. The getCircle() method takes a color as an argument and returns a Circle object with that color. If a Circle object with the given color doesn't exist in the map, a new one is created and added to the map.

Finally, we create a Client class that uses the ShapeFactory to create and draw Circle objects:


      public class Client {
        public static void main(String[] args) {
        ShapeFactory shapeFactory = new ShapeFactory();

        Circle circle1 = shapeFactory.getCircle("red");
        circle1.setRadius(10);
        circle1.draw(0, 0, "red");

        Circle circle2 = shapeFactory.getCircle("red");
        circle2.setRadius(20);
        circle2.draw(100, 100, "red");

        Circle circle3 = shapeFactory.getCircle("blue");
        circle3.setRadius(30);
        circle3.draw(200, 200, "blue");

        Circle circle4 = shapeFactory.getCircle("blue");
        circle4.setRadius(40);
        circle4.draw(300, 300, "blue");
    }
      }
      
      
In this example, we have used the Flyweight pattern to efficiently reuse Circle objects with the same color. The ShapeFactory class acts as a flyweight factory that creates and manages Circle objects, and the Client class uses the ShapeFactory to create and draw circles. By reusing Circle objects with the same color, we can reduce the memory footprint of the application and improve its performance.



## The Proxy pattern is a structural design pattern that provides a surrogate or placeholder for another object to control access to it.

Here's an example of how to implement the Proxy pattern in Java:

Suppose we have an interface Internet that has a method connectTo(String serverHost) that connects to a remote server:
           
           
            public interface Internet {
    void connectTo(String serverHost) throws Exception;
     }
 
 
 We create a RealInternet class that implements the Internet interface and connects to the remote server:
 
 
     public class RealInternet implements Internet {
    @Override
    public void connectTo(String serverHost) throws Exception {
        System.out.printf("Connecting to %s%n", serverHost);
    }
       }
       
       
Next, we create a ProxyInternet class that also implements the Internet interface but acts as a proxy for the RealInternet class:



    public class ProxyInternet implements Internet {
    private final Internet internet = new RealInternet();
    private final Set<String> bannedSites = new HashSet<>();

    public ProxyInternet() {
        bannedSites.add("example.com");
        bannedSites.add("test.com");
    }

    @Override
    public void connectTo(String serverHost) throws Exception {
        if (bannedSites.contains(serverHost)) {
            throw new Exception("Access Denied");
        }
        internet.connectTo(serverHost);
    }
   }
   
   
   
 The ProxyInternet class has a RealInternet object as a field, which it uses to connect to the remote server. It also has a bannedSites set that contains the list of banned server hosts. In the connectTo() method, the ProxyInternet checks if the server host is in the banned list, and if so, throws an exception. Otherwise, it delegates the connection to the RealInternet object.

Finally, we create a Client class that uses the ProxyInternet to connect to remote servers:



    public class Client {
    public static void main(String[] args) {
        Internet internet = new ProxyInternet();
        try {
            internet.connectTo("example.com");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
        try {
            internet.connectTo("google.com");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
        try {
            internet.connectTo("test.com");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
      }
      
      
 In this example, we have used the Proxy pattern to provide a proxy for the RealInternet class that controls access to the remote server. The ProxyInternet class checks if the server host is in the banned list and throws an exception if so. Otherwise, it delegates the connection to the RealInternet object. By using the Proxy pattern, we can add extra functionality to the Internet interface without modifying the RealInternet class.
 
 
 
 


 

     
  
