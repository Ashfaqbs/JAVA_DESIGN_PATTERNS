## Behavioral design patterns are design patterns that deal with the communication between objects and the delegation of responsibilities among them. They focus on how objects interact with one another and how they behave in different situations.

There are several behavioral design patterns, including:

Observer pattern: This pattern defines a one-to-many relationship between objects, where one object (the subject) notifies other objects (the observers) when it changes its state.

Strategy pattern: This pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. The strategy pattern lets the algorithm vary independently from clients that use it.

Template method pattern: This pattern defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

Chain of responsibility pattern: This pattern lets you pass requests along a chain of handlers. Each handler decides whether to handle the request or pass it on to the next handler in the chain.

Command pattern: This pattern encapsulates a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

Interpreter pattern: This pattern defines a representation for a grammar of a given language along with an interpreter that uses the representation to interpret sentences in the language.

Iterator pattern: This pattern provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

Mediator pattern: This pattern defines an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly and it lets you vary their interaction independently.

Memento pattern: This pattern lets you save and restore the previous state of an object without revealing the details of its implementation.

State pattern: This pattern allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

Visitor pattern: This pattern lets you define a new operation without changing the classes of the elements on which it operates.

Each of these patterns addresses a different aspect of object behavior and can be applied in different scenarios to improve the design and functionality of the software system.

## Observer pattern implemented in Java:


     import java.util.ArrayList;
     import java.util.List;

     interface Observer {
    void update(String message);
    }

     interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers(String message);
     }

    class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String state;

    public void attach(Observer observer) {
        observers.add(observer);
    }

    public void detach(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void setState(String state) {
        this.state = state;
        notifyObservers(state);
    }
      }

     class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    public void update(String message) {
        System.out.println(name + " received the message: " + message);
    }
      }

    public class Main {
    public static void main(String[] args) {
        ConcreteSubject subject = new ConcreteSubject();
        ConcreteObserver observer1 = new ConcreteObserver("Observer 1");
        ConcreteObserver observer2 = new ConcreteObserver("Observer 2");
        ConcreteObserver observer3 = new ConcreteObserver("Observer 3");

        subject.attach(observer1);
        subject.attach(observer2);
        subject.attach(observer3);

        subject.setState("New state");
        subject.detach(observer2);

        subject.setState("Another state");
    }
      }
 



In this example, we have a Subject interface and a ConcreteSubject class that implements it. The ConcreteSubject maintains a list of Observer objects and notifies them whenever its state changes.

The Observer interface is implemented by the ConcreteObserver class, which receives messages from the ConcreteSubject and prints them to the console.

In the Main class, we create a ConcreteSubject object and three ConcreteObserver objects. We attach all three observers to the subject and then set the subject's state to "New state", which triggers the notifyObservers() method and sends the message to all observers. We then detach the second observer and set the state to "Another state", which triggers the update() method for the remaining observers.

The output of the program will be:

Observer 1 received the message: New state
Observer 2 received the message: New state
Observer 3 received the message: New state
Observer 1 received the message: Another state
Observer 3 received the message: Another state



## Strategy pattern implemented in Java:

     interface SortingStrategy {
    int[] sort(int[] array);
     }

     class BubbleSort implements SortingStrategy {
    public int[] sort(int[] array) {
        int n = array.length;
        for (int i = 0; i < n-1; i++) {
            for (int j = 0; j < n-i-1; j++) {
                if (array[j] > array[j+1]) {
                    int temp = array[j];
                    array[j] = array[j+1];
                    array[j+1] = temp;
                }
            }
        }
        return array;
    }
    }

     class QuickSort implements SortingStrategy {
    public int[] sort(int[] array) {
        quickSort(array, 0, array.length - 1);
        return array;
    }

    private void quickSort(int[] array, int left, int right) {
        if (left < right) {
            int pivotIndex = partition(array, left, right);
            quickSort(array, left, pivotIndex - 1);
            quickSort(array, pivotIndex + 1, right);
        }
    }

    private int partition(int[] array, int left, int right) {
        int pivot = array[right];
        int i = left - 1;
        for (int j = left; j < right; j++) {
            if (array[j] <= pivot) {
                i++;
                int temp = array[i];
                array[i] = array[j];
                array[j] = temp;
            }
        }
        int temp = array[i+1];
        array[i+1] = array[right];
        array[right] = temp;
        return i+1;
    }
       }

     class SortingAlgorithm {
    private SortingStrategy strategy;

    public SortingAlgorithm(SortingStrategy strategy) {
        this.strategy = strategy;
    }

    public void setSortingStrategy(SortingStrategy strategy) {
        this.strategy = strategy;
    }

    public int[] sort(int[] array) {
        return strategy.sort(array);
    }
     }

     public class Main {
    public static void main(String[] args) {
        SortingAlgorithm algorithm = new SortingAlgorithm(new BubbleSort());
        int[] array1 = {5, 2, 7, 1, 9};
        int[] array2 = {8, 4, 6, 3, 0};

        System.out.println("Before sorting:");
        System.out.println(Arrays.toString(array1));
        System.out.println(Arrays.toString(array2));

        algorithm.sort(array1);
        algorithm.setSortingStrategy(new QuickSort());
        algorithm.sort(array2);

        System.out.println("After sorting:");
        System.out.println(Arrays.toString(array1));
        System.out.println(Arrays.toString(array2));
    }
     }
     
     
     
     
 In this example, we have a SortingStrategy interface with a sort() method that takes an array of integers and returns a sorted version of the array. The BubbleSort and QuickSort classes implement this interface to provide different sorting algorithms.

The SortingAlgorithm class uses a SortingStrategy object to perform the actual sorting. The setSortingStrategy() method allows the strategy to be changed at runtime if needed.

In the Main class, we create a SortingAlgorithm object with a BubbleSort strategy and use it to sort two different arrays. We then switch the strategy to QuickSort and sort the second array again.

The output of the program will be:

Before sorting:
[5, 2, 7, 1, 9]
[8, 4, 6, 3, 0]
After sorting:
[1, 2, 5, 7, 9]
[0, 3, 4, 6, 8]







## Template Method design pattern:



    abstract class DataProcessor {
    // This is the template method that defines the algorithm.
    public void process() {
        readData();
        processData();
        writeData();
    }

    // These methods are implementation-specific and must be overridden by concrete classes.
    protected abstract void readData();
    protected abstract void processData();
    protected abstract void writeData();
   }

    class CSVDataProcessor extends DataProcessor {
    @Override
    protected void readData() {
        System.out.println("Reading data from CSV file");
    }

    @Override
    protected void processData() {
        System.out.println("Processing data from CSV file");
    }

    @Override
    protected void writeData() {
        System.out.println("Writing data to CSV file");
    }
      }

     class ExcelDataProcessor extends DataProcessor {
    @Override
    protected void readData() {
        System.out.println("Reading data from Excel file");
    }

    @Override
    protected void processData() {
        System.out.println("Processing data from Excel file");
    }

    @Override
    protected void writeData() {
        System.out.println("Writing data to Excel file");
    }
    }

      public class Main {
       public static void main(String[] args) {
        DataProcessor csvProcessor = new CSVDataProcessor();
        csvProcessor.process();

        DataProcessor excelProcessor = new ExcelDataProcessor();
        excelProcessor.process();
    }
    }

    
    
 In this example, DataProcessor is an abstract class that defines the overall algorithm for processing data. The process() method is the template method that calls the implementation-specific methods readData(), processData(), and writeData(). These methods must be implemented by concrete classes that extend DataProcessor.

CSVDataProcessor and ExcelDataProcessor are concrete classes that extend DataProcessor and provide the implementation for the abstract methods. When process() is called on these objects, the template method is executed, and the implementation-specific methods are called in the correct order based on the object type.

When we run this code, we get the following output:


Reading data from CSV file
Processing data from CSV file
Writing data to CSV file
Reading data from Excel file
Processing data from Excel file
Writing data to Excel file




As we can see, the process() method is called on the csvProcessor and excelProcessor objects, which in turn call the implementation-specific methods in the correct order based on the object type. This allows us to define a common algorithm in the abstract class, while still allowing for specific implementations in the concrete classes.



## Chain of Responsibility design pattern:


    abstract class Logger {
    private Logger nextLogger;

    public Logger setNextLogger(Logger nextLogger) {
        this.nextLogger = nextLogger;
        return nextLogger;
    }

    public void logMessage(int level, String message) {
        if (level >= getLogLevel()) {
            write(message);
        }
        if (nextLogger != null) {
            nextLogger.logMessage(level, message);
        }
    }

    protected abstract int getLogLevel();

    protected abstract void write(String message);
}

     class ConsoleLogger extends Logger {
    @Override
    protected int getLogLevel() {
        return 1;
    }

    @Override
    protected void write(String message) {
        System.out.println("Console logger: " + message);
    }
     }

      class FileLogger extends Logger {
    @Override
    protected int getLogLevel() {
        return 2;
    }

    @Override
    protected void write(String message) {
        System.out.println("File logger: " + message);
    }
     }

     class EmailLogger extends Logger {
    @Override
    protected int getLogLevel() {
        return 3;
    }

    @Override
    protected void write(String message) {
        System.out.println("Email logger: " + message);
    }
      }

     public class Main {
    public static void main(String[] args) {
        Logger consoleLogger = new ConsoleLogger();
        Logger fileLogger = new FileLogger();
        Logger emailLogger = new EmailLogger();

        // Set up the chain of responsibility
        consoleLogger.setNextLogger(fileLogger).setNextLogger(emailLogger);

        // Test the chain of responsibility
        consoleLogger.logMessage(1, "Message to be logged at console logger");
        consoleLogger.logMessage(2, "Message to be logged at console logger and file logger");
        consoleLogger.logMessage(3, "Message to be logged at console logger, file logger, and email logger");
    }
     }
     
     
     
 In this example, we have an abstract class Logger that defines the interface for handling log messages. Each concrete implementation of Logger can handle messages at a different log level. The setNextLogger() method is used to set up the chain of responsibility by linking one logger to the next. When a message is passed to a logger, it checks if it can handle the message based on its log level. If it can handle the message, it writes it to the appropriate output. If it cannot handle the message, it passes it on to the next logger in the chain.

ConsoleLogger, FileLogger, and EmailLogger are concrete implementations of Logger that handle messages at different log levels. ConsoleLogger handles messages with a log level of 1, FileLogger handles messages with a log level of 2, and EmailLogger handles messages with a log level of 3.

When we run this code, we get the following output:



Console logger: Message to be logged at console logger
Console logger: Message to be logged at console logger and file logger
Console logger: Message to be logged at console logger, file logger, and email logger
File logger: Message to be logged at console logger and file logger
File logger: Message to be logged at console logger, file logger, and email logger
Email logger: Message to be logged at console logger, file logger, and email logger


As we can see, when a message is passed to the consoleLogger, it checks if it can handle the message based on its log level. If it can handle the message, it writes it to the console. If it cannot handle the message, it passes it on to the next logger




## Command design pattern:


     interface Command {
    void execute();
    }

    class Light {
    public void on() {
        System.out.println("Light is on");
    }

    public void off() {
        System.out.println("Light is off");
    }
     }

    class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.on();
    }
      }

    class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.off();
    }
     }

    class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }
      }

    public class Main {
    public static void main(String[] args) {
        Light light = new Light();
        RemoteControl remoteControl = new RemoteControl();

        // Turn on the light
        Command lightOnCommand = new LightOnCommand(light);
        remoteControl.setCommand(lightOnCommand);
        remoteControl.pressButton();

        // Turn off the light
        Command lightOffCommand = new LightOffCommand(light);
        remoteControl.setCommand(lightOffCommand);
        remoteControl.pressButton();
    }
     }

     
  In this example, we have an interface Command that defines the interface for executing a command. The Light class represents the receiver of the command, i.e., the object that actually performs the action. LightOnCommand and LightOffCommand are concrete implementations of Command that encapsulate the on() and off() methods of Light, respectively.

RemoteControl represents the invoker of the command, i.e., the object that initiates the execution of a command. It has a setCommand() method that sets the command to be executed and a pressButton() method that executes the command.

When we run this code, we get the following output:

Light is on
Light is off
As we can see, when we set the lightOnCommand to be executed and press the button on the remoteControl, the light.on() method is executed. Similarly, when we set the lightOffCommand to be executed and press the button on the remoteControl, the light.off() method is executed.



## The Interpreter pattern is used to define a grammar for interpreting a language and provides an interpreter to interpret sentences in the language. It falls under the behavioral design pattern category.



Here's an example of how to implement the Interpreter pattern in Java:



     interface Expression {
    boolean interpret(String context);
     }
 
     class TerminalExpression implements Expression {
             private String data;

    public TerminalExpression(String data) {
        this.data = data;
    }

    @Override
    public boolean interpret(String context) {
        if (context.contains(data)) {
            return true;
        }
        return false;
    }
       }

    class OrExpression implements Expression {
    private Expression expr1;
    private Expression expr2;

    public OrExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    @Override
    public boolean interpret(String context) {
        return expr1.interpret(context) || expr2.interpret(context);
    }
        }

         class AndExpression implements Expression {
    private Expression expr1;
    private Expression expr2;

    public AndExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    @Override
    public boolean interpret(String context) {
        return expr1.interpret(context) && expr2.interpret(context);
    }
     }
 
    public class Main {
    public static void main(String[] args) {
        Expression john = new TerminalExpression("John");
        Expression married = new TerminalExpression("married");
        Expression and = new AndExpression(john, married);

        Expression julie = new TerminalExpression("Julie");
        Expression divorced = new TerminalExpression("divorced");
        Expression or = new OrExpression(julie, divorced);

        String context1 = "John is married";
        String context2 = "Julie is divorced";
        String context3 = "Jack is single";

        System.out.println(and.interpret(context1));
        System.out.println(or.interpret(context2));
        System.out.println(and.interpret(context3));
    } 
            }
            
            
            
            
  In this example, we have an interface Expression that defines the interpret() method. The TerminalExpression class represents the terminal expression in the grammar, i.e., the basic building blocks of the language. OrExpression and AndExpression represent the non-terminal expressions in the grammar, i.e., the expressions that combine terminal expressions to form more complex expressions.

We then define the context in which the language is interpreted, represented by strings context1, context2, and context3. We create expressions by combining the terminal expressions using the non-terminal expressions to form more complex expressions. Finally, we call the interpret() method on these expressions with the respective contexts to check if the sentence is valid in the language.

When we run this code, we get the following output:

true
true
false
  

## The Iterator pattern is used to provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation. It falls under the behavioral design pattern category.

Here's an example of how to implement the Iterator pattern in Java:


    import java.util.ArrayList;
      import java.util.Iterator;
       import java.util.List;

      interface IteratorInterface {
    boolean hasNext();
    Object next();
     }

        class NameIterator implements IteratorInterface {
    private int index = 0;
    private List<String> names;

    public NameIterator(List<String> names) {
        this.names = names;
    }

    @Override
    public boolean hasNext() {
        if (index < names.size()) {
            return true;
        }
        return false;
    }

    @Override
    public Object next() {
        if (this.hasNext()) {
            return names.get(index++);
        }
        return null;
    }
      }

        interface Container {
    IteratorInterface getIterator();
     }

        class NameRepository implements Container {
    private List<String> names;

    public NameRepository() {
        names = new ArrayList<String>();
    }

    public void addName(String name) {
        names.add(name);
    }

    @Override
    public IteratorInterface getIterator() {
        return new NameIterator(names);
    }
      }

       public class Main {
    public static void main(String[] args) {
        NameRepository nameRepository = new NameRepository();
        nameRepository.addName("John");
        nameRepository.addName("Jane");
        nameRepository.addName("Joe");

        IteratorInterface iterator = nameRepository.getIterator();

        while (iterator.hasNext()) {
            String name = (String) iterator.next();
            System.out.println("Name: " + name);
        }
          }
        }  

        
        
        
        
  In this example, we have an interface IteratorInterface that defines the hasNext() and next() methods. The NameIterator class implements this interface and provides an implementation for iterating over a list of names. The NameRepository class implements the Container interface and provides a way to get an iterator for the list of names.

We then create a NameRepository object, add some names to it, and get an iterator for the list of names using the getIterator() method. We then iterate over the list of names using the hasNext() and next() methods on the iterator.

When we run this code, we get the following output:


Name: John
Name: Jane
Name: Joe
As we can see, the iterator correctly iterates over the list of names and prints them to the console.


## The Mediator pattern is used to define an object that encapsulates the communication between a set of objects. It falls under the behavioral design pattern category.

Here's an example of how to implement the Mediator pattern in Java:



    interface Mediator {
    void sendMessage(String message, Colleague colleague);
    }

        abstract class Colleague {
    private Mediator mediator;

    public Colleague(Mediator mediator) {
        this.mediator = mediator;
    }

    public void sendMessage(String message) {
        mediator.sendMessage(message, this);
    }

    public abstract void receiveMessage(String message);
     }

        class ConcreteColleague1 extends Colleague {
    public ConcreteColleague1(Mediator mediator) {
        super(mediator);
    }

    @Override
    public void receiveMessage(String message) {
        System.out.println("ConcreteColleague1 received message: " + message);
    }
       }

         class ConcreteColleague2 extends Colleague {
    public ConcreteColleague2(Mediator mediator) {
        super(mediator);
    }

    @Override
    public void receiveMessage(String message) {
        System.out.println("ConcreteColleague2 received message: " + message);
    }
     }

    class ConcreteMediator implements Mediator {
    private ConcreteColleague1 colleague1;
    private ConcreteColleague2 colleague2;

    public void setColleague1(ConcreteColleague1 colleague1) {
        this.colleague1 = colleague1;
    }

    public void setColleague2(ConcreteColleague2 colleague2) {
        this.colleague2 = colleague2;
    }

    @Override
    public void sendMessage(String message, Colleague colleague) {
        if (colleague == colleague1) {
            colleague2.receiveMessage(message);
        } else {
            colleague1.receiveMessage(message);
        }
    }
   }

       public class Main {
    public static void main(String[] args) {
        ConcreteMediator mediator = new ConcreteMediator();

        ConcreteColleague1 colleague1 = new ConcreteColleague1(mediator);
        ConcreteColleague2 colleague2 = new ConcreteColleague2(mediator);

        mediator.setColleague1(colleague1);
        mediator.setColleague2(colleague2);

        colleague1.sendMessage("Hello from colleague1");
        colleague2.sendMessage("Hello from colleague2");
    }
    }
    
    
    
 In this example, we have an interface Mediator that defines the sendMessage() method. The Colleague abstract class has a reference to the Mediator object and provides a way to send messages using the sendMessage() method. The ConcreteColleague1 and ConcreteColleague2 classes implement the Colleague class and provide an implementation for receiving messages.

The ConcreteMediator class implements the Mediator interface and provides an implementation for sending messages between the two colleagues. The Main class creates a ConcreteMediator object and two ConcreteColleague objects. It sets the two colleagues as the Colleague objects for the ConcreteMediator object and sends messages between them using the sendMessage() method.

When we run this code, we get the following output:
ConcreteColleague2 received message: Hello from colleague1
ConcreteColleague1 received message: Hello from colleague2



As we can see, the messages are correctly sent between the two colleagues using the ConcreteMediator object.

## The Memento pattern is used to capture and externalize the state of an object without violating encapsulation, so that the object can be restored to that state later. It falls under the behavioral design pattern category.

Here's an example of how to implement the Memento pattern in Java:



     class Memento {
    private final String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
   }
  
     class Originator {
      private String state;

    public void setState(String state) {
        System.out.println("Setting state to " + state);
        this.state = state;
    }

    public Memento save() {
        System.out.println("Saving state " + state);
        return new Memento(state);
    }

    public void restore(Memento memento) {
        state = memento.getState();
        System.out.println("Restoring state to " + state);
    }
    }

      class Caretaker {
    private Memento memento;

    public void saveState(Originator originator) {
        memento = originator.save();
    }

    public void restoreState(Originator originator) {
        originator.restore(memento);
    }
     }

      public class Main {
    public static void main(String[] args) {
        Originator originator = new Originator();
        Caretaker caretaker = new Caretaker();

        originator.setState("State 1");
        caretaker.saveState(originator);

        originator.setState("State 2");
        caretaker.saveState(originator);

        originator.setState("State 3");
        caretaker.restoreState(originator);

        originator.setState("State 4");
        caretaker.restoreState(originator);
    }
      }
      
  
      
      
   In this example, we have a Memento class that represents the saved state of the Originator object. The Originator class has a setState() method to set its internal state and a save() method to create a new Memento object to represent its current state. It also has a restore() method to restore the state of the Originator object to a previous state represented by a Memento object.

The Caretaker class is responsible for storing and retrieving Memento objects. It has a saveState() method that saves the current state of the Originator object and a restoreState() method that restores the state of the Originator object to a previously saved state.

The Main class creates an Originator object and a Caretaker object. It sets the state of the Originator object to different values and saves the state using the Caretaker object. It then restores the state of the Originator object to previous states using the Caretaker object.

When we run this code, we get the following output:
Setting state to State 1
Saving state State 1
Setting state to State 2
Saving state State 2
Setting state to State 3
Restoring state to State 2
Setting state to State 4
Restoring state to State 3

As we can see, the Originator object is able to save and restore its state using the Memento objects created by the save() method, and the Caretaker object is able to store and retrieve Memento objects to save and restore the state of the Originator object.





## State pattern in Java:


First, let's define an interface for our state classes:

      public interface State {
    void handleState(Context context);
     }
     
     
  Next, let's define our context class, which will hold the current state and delegate requests to it:
  
  
     public class Context {
    private State state;

    public void setState(State state) {
        this.state = state;
    }

    public void request() {
        state.handleState(this);
    }
   }
   
   
 Now, let's define some concrete state classes:

      public class StateA implements State {
    @Override
    public void handleState(Context context) {
        System.out.println("Handling state A");
        context.setState(new StateB());
    }
        }

          public class StateB implements State {
    @Override
    public void handleState(Context context) {
        System.out.println("Handling state B");
        context.setState(new StateC());
    }
         }

           public class StateC implements State {
    @Override
    public void handleState(Context context) {
        System.out.println("Handling state C");
        context.setState(new StateA());
    }
        }
 
  Finally, let's see the State pattern in action:

           public class Main {
    public static void main(String[] args) {
        Context context = new Context();
        context.setState(new StateA());

        for (int i = 0; i < 3; i++) {
            context.request();
        }
    }
         } 
  When we run this code, we'll see the following output:

  Handling state A
Handling state B
Handling state C

## Visitor pattern in Java:.

Let's start by defining an interface for our Visitor:

    public interface Visitor {
    void visit(ElementA element);
    void visit(ElementB element);
     }
     
     
   Next, let's define some concrete elements that can be visited:

      public interface Element {
    void accept(Visitor visitor);
   }

    public class ElementA implements Element {
     @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }

    public void operationA() {
        System.out.println("ElementA operationA");
    }
    }

     public class ElementB implements Element {
    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }

    public void operationB() {
        System.out.println("ElementB operationB");
    }
    }
   

Notice that each element has an accept method that takes a Visitor and calls the appropriate visit method.

Now, let's define some concrete Visitor classes:

       public class VisitorA implements Visitor {
    @Override
    public void visit(ElementA element) {
        element.operationA();
    }

    @Override
    public void visit(ElementB element) {
        element.operationB();
    }
     }

       public class VisitorB implements Visitor {
    @Override
    public void visit(ElementA element) {
        System.out.println("VisitorB visiting ElementA");
    }

    @Override
    public void visit(ElementB element) {
        System.out.println("VisitorB visiting ElementB");
    }
       }
       
       
       
       
       
 Finally, let's see the Visitor pattern in action:

       public class Main {
    public static void main(String[] args) {
        List<Element> elements = new ArrayList<>();
        elements.add(new ElementA());
        elements.add(new ElementB());

        Visitor visitorA = new VisitorA();
        Visitor visitorB = new VisitorB();

        for (Element element : elements) {
            element.accept(visitorA);
            element.accept(visitorB);
        }
    }
     }
     
     
 When we run this code, we'll see the following output:

 ElementA operationA
VisitorB visiting ElementA
ElementB operationB
VisitorB visiting ElementB
 
 
 
 In this example, we create a list of Element objects and two Visitor objects, visitorA and visitorB. We then iterate over each element in the list and call the accept method with both visitors. Each visitor performs a different operation on each element. The Visitor pattern allows us to add new operations to the elements without modifying the element classes themselves.
 

