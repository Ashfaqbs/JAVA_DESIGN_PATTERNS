# Creational design patterns
Creational design patterns are concerned with object creation mechanisms,
trying to create objects in a way that's suitable for a given situation. They deal with object creation mechanisms that can be used to create objects in a controlled and efficient manner. Here are some common creational design patterns in Java:

Singleton: Ensures that only one instance of a class is created and provides a global point of access to it.

Factory: Provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

Abstract Factory: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

Builder: Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

Prototype: Allows new objects to be created by copying existing objects, without the need for creating new objects from scratch.

These patterns are useful for creating objects in a flexible, efficient, and reusable manner. 
hey help to reduce code duplication, improve code maintainability, and make your code more scalable.
By choosing the appropriate creational design pattern for your project, you can optimize object creation and make your code more robust.




## Singleton is a creational design pattern that ensures that only one instance of a class is created and provides a global point of access to it. This pattern is commonly used in situations where it is necessary to limit the number of instances of a class to one and provide a global point of access to that instance.

Here is an example of a Singleton in Java:




    public class Singleton {
     private static Singleton instance = null;
    private String message;

    private Singleton() {
        message = "Hello, I am a Singleton!";
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    public void showMessage() {
        System.out.println(message);
    }
    }
    
    
   In this example, the Singleton class has a private constructor and a private static variable "instance" that is initialized to null. The getInstance() method is public and static and provides global access to the Singleton instance. If the instance has not been created yet, it creates a new instance and returns it. If the instance has already been created, it simply returns the existing instance.

The showMessage() method is an example of a method that can be called on the Singleton instance to perform some operation.

Using the Singleton pattern ensures that there is only one instance of the Singleton class in the system and that it can be accessed from anywhere in the code. This pattern is useful in situations where a single instance of a class needs to be shared across multiple objects or subsystems, such as logging or database access.



## The Factory pattern is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. This pattern provides a way to encapsulate object creation so that the implementation of the creation process can be changed later without affecting the client code.

Here's an example of the Factory pattern in Java:


     public interface Animal {
    void makeSound();
     }

     public class Dog implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
        }
     }

     public class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
    }

     public class AnimalFactory {
       public static Animal createAnimal(String animalType) {
        Animal animal = null;

        switch (animalType) {
            case "dog":
                animal = new Dog();
                break;
            case "cat":
                animal = new Cat();
                break;
            default:
                System.out.println("Invalid animal type.");
                break;
        }

        return animal;
    }
  }

    public class Main {
    public static void main(String[] args) {
        Animal dog = AnimalFactory.createAnimal("dog");
        Animal cat = AnimalFactory.createAnimal("cat");

        dog.makeSound(); // Output: Woof!
        cat.makeSound(); // Output: Meow!
    }
     }
     
     
     
In this example, we have an interface called Animal that defines a makeSound() method. We have two concrete classes that implement the Animal interface: Dog and Cat.

We also have a AnimalFactory class that provides a static method createAnimal() that takes an animalType parameter and returns an instance of the corresponding animal class.

Finally, we have a Main class that uses the AnimalFactory to create Dog and Cat objects and call their makeSound() methods.

Using the Factory pattern, we have encapsulated the creation process of Animal objects in the AnimalFactory class. If we want to add a new animal class, we can simply add a new case to the createAnimal() method without affecting the client code.


## Abstract Factory is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. This pattern is useful when there are multiple families of related objects that need to be created and used together.

Here is an example of the Abstract Factory pattern in Java:


    public interface Shape {
    void draw();
    }

    public class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Rectangle");
        }
     }

     public class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Circle");
         }
     }

     public interface ShapeFactory {
    Shape createShape();
     }

          public class RectangleFactory implements ShapeFactory {
    @Override
    public Shape createShape() {
        return new Rectangle();
              }
          }

          public class CircleFactory implements ShapeFactory {
    @Override
    public Shape createShape() {
        return new Circle();
              }
          }

     public class ShapeProducer {
         public static ShapeFactory getFactory(boolean rounded) {
             if (rounded) {
            return new RoundedShapeFactory();
             } else {
            return new RegularShapeFactory();
        }
         }
     }

    public class RegularShapeFactory implements ShapeFactory {
    @Override
    public Shape createShape() {
        return new Rectangle();
    }
    }
  
     public class RoundedShapeFactory implements ShapeFactory {
    @Override
    public Shape createShape() {
        return new RoundedRectangle();
    }
    }

     public class RoundedRectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing RoundedRectangle");
         }
      }
      
      
   In this example, we have an interface called Shape that defines a draw() method. We have two concrete classes that implement the Shape interface: Rectangle and Circle.

We also have an interface called ShapeFactory that defines a method createShape() which returns a Shape object. We have two concrete factories that implement the ShapeFactory interface: RectangleFactory and CircleFactory.

We also have two additional factories, RegularShapeFactory and RoundedShapeFactory, which provide concrete implementations of the ShapeFactory interface for creating different types of shapes.

Finally, we have a ShapeProducer class that provides a static method getFactory() that returns an instance of the appropriate ShapeFactory depending on whether or not the rounded flag is set to true.

Using the Abstract Factory pattern, we can create families of related objects (shapes) without specifying their concrete classes. If we want to add a new family of related objects, we can simply add a new ShapeFactory implementation and update the ShapeProducer to return the new factory.



## The Builder pattern is a creational design pattern that separates the construction of a complex object from its representation, allowing the same construction process to create various representations of the object.

Here's an example of the Builder pattern in Java:

    public class Person {
    private final String firstName;
    private final String lastName;
    private final int age;
    private final String email;
    private final String phone;

    private Person(Builder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.age = builder.age;
        this.email = builder.email;
        this.phone = builder.phone;
    }

    public static class Builder {
        private String firstName;
        private String lastName;
        private int age;
        private String email;
        private String phone;

        public Builder(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }

        public Person build() {
            return new Person(this);
        }
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public int getAge() {
        return age;
    }

    public String getEmail() {
        return email;
    }

    public String getPhone() {
        return phone;
    }
     }
     
     
   In this example, we have a Person class that has a private constructor and a static inner class called Builder. The Builder class has methods for setting various attributes of a Person object and a build() method that returns the final Person object.

To create a Person object using the Builder class, we can use the following code:


     Person person = new Person.Builder("John", "Doe")
                    .age(30)
                    .email("john.doe@example.com")
                    .phone("555-555-5555")
                    .build();


This code creates a new Person object with the specified attributes using the Builder class. The build() method returns the final Person object with all the attributes set.

Using the Builder pattern, we can easily create complex objects by setting only the attributes that we need, without having to set all the attributes in the constructor. This makes the code more readable and maintainable.



## The Prototype pattern is a creational design pattern that allows cloning of objects without coupling the code to the specific classes of those objects. The Prototype pattern is useful when creating objects is expensive or when we need to create a large number of similar objects.

Here's an example of the Prototype pattern in Java:
 
 
 
     public abstract class Shape implements Cloneable {
    private String id;
    protected String type;

    public abstract void draw();

    public String getType() {
        return type;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    @Override
    public Object clone() {
        Object clone = null;

        try {
            clone = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }

        return clone;
    }
     }

    public class Rectangle extends Shape {
    public Rectangle() {
        type = "Rectangle";
    }

    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
      }
     }
  
     public class Circle extends Shape {
      public Circle() {
          type = "Circle";
    }

    @Override
    public void draw() {
        System.out.println("Inside Circle::draw() method.");
    }
}

     public class ShapeCache {
      private static Map<String, Shape> shapeMap = new HashMap<>();

    public static Shape getShape(String shapeId) {
        Shape cachedShape = shapeMap.get(shapeId);
        return (Shape) cachedShape.clone();
    }

    public static void loadCache() {
        Circle circle = new Circle();
        circle.setId("1");
        shapeMap.put(circle.getId(), circle);

        Rectangle rectangle = new Rectangle();
        rectangle.setId("2");
        shapeMap.put(rectangle.getId(), rectangle);
    }
    }
    
    
    
   In this example, we have an abstract Shape class that has a type and an id field, as well as a draw() method that is implemented by the concrete shape classes. The Shape class also implements the Cloneable interface, which allows us to create a clone of a Shape object.

We have two concrete shape classes, Circle and Rectangle, that implement the draw() method.

We also have a ShapeCache class that maintains a map of Shape objects with their id as the key. The ShapeCache class has a getShape() method that returns a clone of a Shape object with the specified shapeId, and a loadCache() method that pre-populates the map with some Circle and Rectangle objects.

To use the Prototype pattern, we can call the loadCache() method to load the cache with some Shape objects, and then use the getShape() method to get a clone of a Shape object with the specified shapeId, like this:




    ShapeCache.loadCache();

    Shape clonedShape = ShapeCache.getShape("1");
    System.out.println("Shape : " + clonedShape.getType());

    Shape clonedShape2 = ShapeCache.getShape("2");
    System.out.println("Shape : " + clonedShape2.getType());
    
    
    
This code loads the ShapeCache with some Circle and Rectangle objects, and then gets clones of these objects using the getShape() method. The clone() method creates a new object with the same attributes as the original object, so we can modify the cloned object without affecting the original object.






 