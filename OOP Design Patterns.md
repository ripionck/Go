## 23 classic design patterns

---

### **Creational Patterns**  
- [Singleton Pattern](#singleton-pattern)  
- [Factory Method Pattern](#factory-method-pattern)  
- [Abstract Factory Pattern](#abstract-factory-pattern)  
- [Builder Pattern](#builder-pattern)  
- [Prototype Pattern](#prototype-pattern)  
- [Object Pool Pattern](#object-pool-pattern)  

---

### **Structural Patterns**  
- [Adapter Pattern](#adapter-pattern)  
- [Decorator Pattern](#decorator-pattern)  
- [Proxy Pattern](#proxy-pattern)  
- [Composite Pattern](#composite-pattern)  
- [Bridge Pattern](#bridge-pattern)  
- [Facade Pattern](#facade-pattern)  
- [Flyweight Pattern](#flyweight-pattern)  

---

### **Behavioral Patterns**  
- [Observer Pattern](#observer-pattern)  
- [Strategy Pattern](#strategy-pattern)  
- [Command Pattern](#command-pattern)  
- [State Pattern](#state-pattern)  
- [Chain of Responsibility Pattern](#chain-of-responsibility-pattern)  
- [Iterator Pattern](#iterator-pattern)  
- [Mediator Pattern](#mediator-pattern)  
- [Memento Pattern](#memento-pattern)  
- [Template Method Pattern](#template-method-pattern)  
- [Visitor Pattern](#visitor-pattern)  

---

## **1. Creational Patterns**

### **Singleton Pattern**
- **Definition**: Ensures a class has only one instance globally accessible.
- **Use Case**: Configuration managers, database connections.
- **Python Example**:
  ```python
  class Singleton:
      _instance = None

      def __new__(cls):
          if not cls._instance:
              cls._instance = super().__new__(cls)
          return cls._instance

  config = Singleton()
  config.settings = {"theme": "dark"}
  ```

### **Factory Method Pattern**
- **Definition**: Subclasses decide which class to instantiate.
- **Use Case**: Logistics app (trucks vs. ships).
- **Python Example**:
  ```python
  class Transport:
      def deliver(self): pass

  class Truck(Transport):
      def deliver(self): return "Deliver by road"

  class Ship(Transport):
      def deliver(self): return "Deliver by sea"

  class Logistics:
      def create_transport(self): pass

  class RoadLogistics(Logistics):
      def create_transport(self): return Truck()

  class SeaLogistics(Logistics):
      def create_transport(self): return Ship()
  ```

### **Abstract Factory Pattern**
- **Definition**: Creates families of related objects.
- **Use Case**: Cross-platform UI toolkits.
- **Python Example**:
  ```python
  class Button:
      def render(self): pass

  class WindowsButton(Button):
      def render(self): return "Windows style button"

  class MacButton(Button):
      def render(self): return "Mac style button"

  class UIFactory:
      def create_button(self): pass

  class WindowsFactory(UIFactory):
      def create_button(self): return WindowsButton()

  class MacFactory(UIFactory):
      def create_button(self): return MacButton()
  ```

### **Builder Pattern**
- **Definition**: Constructs complex objects step-by-step.
- **Use Case**: Meal combos in a restaurant app.
- **Python Example**:
  ```python
  class Meal:
      def __init__(self):
          self.items = []

      def add_item(self, item): self.items.append(item)

  class MealBuilder:
      def build_burger(self): pass
      def build_drink(self): pass

  class VegMealBuilder(MealBuilder):
      def build_burger(self): return "Veg Burger"
      def build_drink(self): return "Coke"
  ```

### **Prototype Pattern**
- **Definition**: Clones objects to avoid costly creation.
- **Use Case**: Game graphics with reusable sprites.
- **Python Example**:
  ```python
  import copy

  class Sprite:
      def clone(self): return copy.deepcopy(self)

  ghost = Sprite()
  cloned_ghost = ghost.clone()
  ```

### **Object Pool Pattern**
- **Definition**: Manages reusable object pools.
- **Use Case**: Database connection pooling.
- **Python Example**:
  ```python
  class ConnectionPool:
      def __init__(self, size):
          self._pool = [self._create_conn() for _ in range(size)]

      def _create_conn(self): return "New Connection"

      def acquire(self): return self._pool.pop()

      def release(self, conn): self._pool.append(conn)
  ```

---

## **2. Structural Patterns**

### **Adapter Pattern**
- **Definition**: Converts incompatible interfaces.
- **Use Case**: Legacy system integration.
- **Python Example**:
  ```python
  class OldSystem:
      def legacy_request(self): return "Legacy data"

  class Adapter:
      def __init__(self, old_system):
          self.old_system = old_system

      def request(self): return self.old_system.legacy_request().upper()
  ```

### **Decorator Pattern**
- **Definition**: Adds responsibilities dynamically.
- **Use Case**: Adding logging/encryption to data streams.
- **Python Example**:
  ```python
  class DataStream:
      def write(self, data): pass

  class LoggingDecorator(DataStream):
      def __init__(self, stream):
          self.stream = stream

      def write(self, data):
          print(f"Log: {data}")
          self.stream.write(data)
  ```

### **Proxy Pattern**
- **Definition**: Controls access to an object.
- **Use Case**: Lazy-loading images.
- **Python Example**:
  ```python
  class Image:
      def display(self): pass

  class RealImage(Image):
      def display(self): print("Displaying image")

  class ProxyImage(Image):
      def __init__(self):
          self._real_image = None

      def display(self):
          if not self._real_image:
              self._real_image = RealImage()
          self._real_image.display()
  ```

### **Composite Pattern**
- **Definition**: Represents part-whole hierarchies.
- **Use Case**: File system (files and folders).
- **Python Example**:
  ```python
  class FileSystemComponent:
      def ls(self): pass

  class File(FileSystemComponent):
      def ls(self): print("File")

  class Folder(FileSystemComponent):
      def __init__(self):
          self.children = []

      def add(self, child): self.children.append(child)

      def ls(self):
          for child in self.children:
              child.ls()
  ```

### **Bridge Pattern**
- **Definition**: Separates abstraction from implementation.
- **Use Case**: Remote controls for different devices.
- **Python Example**:
  ```python
  class Device:
      def turn_on(self): pass

  class TV(Device):
      def turn_on(self): return "TV on"

  class Remote:
      def __init__(self, device):
          self.device = device

      def press_button(self): print(self.device.turn_on())
  ```

### **Facade Pattern**
- **Definition**: Simplifies complex subsystems.
- **Use Case**: Home theater system control.
- **Python Example**:
  ```python
  class HomeTheaterFacade:
      def __init__(self):
          self.dvd = DVDPlayer()
          self.sound = SoundSystem()

      def watch_movie(self):
          self.dvd.on()
          self.sound.set_volume(10)
  ```

### **Flyweight Pattern**
- **Definition**: Shares objects to reduce memory.
- **Use Case**: Text editor character formatting.
- **Python Example**:
  ```python
  class CharacterStyle:
      _styles = {}

      def __new__(cls, font):
          if font not in cls._styles:
              cls._styles[font] = super().__new__(cls)
          return cls._styles[font]
  ```

---

## **3. Behavioral Patterns**

### **Observer Pattern**
- **Definition**: Notifies dependents of state changes.
- **Use Case**: Stock market alerts.
- **Python Example**:
  ```python
  class Stock:
      def __init__(self):
          self._observers = []

      def add_observer(self, observer):
          self._observers.append(observer)

      def notify(self, price):
          for observer in self._observers:
              observer.update(price)
  ```

### **Strategy Pattern**
- **Definition**: Encapsulates interchangeable algorithms.
- **Use Case**: Payment methods (credit card, PayPal).
- **Python Example**:
  ```python
  class PaymentStrategy:
      def pay(self, amount): pass

  class CreditCard(PaymentStrategy):
      def pay(self, amount): print(f"Paid ${amount} via Credit Card")

  class PayPal(PaymentStrategy):
      def pay(self, amount): print(f"Paid ${amount} via PayPal")
  ```

### **Command Pattern**
- **Definition**: Encapsulates requests as objects.
- **Use Case**: Undo/redo in text editors.
- **Python Example**:
  ```python
  class Command:
      def execute(self): pass

  class CopyCommand(Command):
      def execute(self): print("Copied text")
  ```

### **State Pattern**
- **Definition**: Changes behavior with internal state.
- **Use Case**: Vending machine states (idle, processing).
- **Python Example**:
  ```python
  class VendingMachine:
      def __init__(self):
          self.state = IdleState()

      def insert_coin(self):
          self.state = ProcessingState()
  ```

### **Chain of Responsibility**
- **Definition**: Passes requests through a handler chain.
- **Use Case**: Middleware in web frameworks.
- **Python Example**:
  ```python
  class Handler:
      def __init__(self, successor=None):
          self.successor = successor

      def handle(self, request):
          if self.successor:
              self.successor.handle(request)
  ```

### **Iterator Pattern**
- **Definition**: Traverses collections uniformly.
- **Use Case**: Custom collection traversal.
- **Python Example**:
  ```python
  class BookCollection:
      def __init__(self):
          self.books = []

      def __iter__(self):
          return iter(self.books)
  ```

### **Mediator Pattern**
- **Definition**: Centralizes object communication.
- **Use Case**: Chat rooms.
- **Python Example**:
  ```python
  class ChatRoom:
      def send_message(self, user, message):
          print(f"[{user}] {message}")
  ```

### **Memento Pattern**
- **Definition**: Captures/restores object state.
- **Use Case**: Game save points.
- **Python Example**:
  ```python
  class GameMemento:
      def __init__(self, state):
          self.state = state
  ```

### **Template Method Pattern**
- **Definition**: Defines algorithm skeleton.
- **Use Case**: Data export steps (format, save).
- **Python Example**:
  ```python
  class DataExporter:
      def export(self):
          self.format()
          self.save()

      def format(self): pass
      def save(self): pass
  ```

### **Visitor Pattern**
- **Definition**: Adds operations to objects.
- **Use Case**: Tax calculation for products.
- **Python Example**:
  ```python
  class ProductVisitor:
      def visit_book(self, book): pass
      def visit_electronics(self, electronics): pass
  ```

---

## **Key Differences**
- **Factory Method vs. Abstract Factory**: Factory Method creates one product, while Abstract Factory creates product families.
- **Strategy vs. State**: Strategy changes behavior via external algorithms, while State changes via internal state transitions.
