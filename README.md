
# SOLID principles

SOLID stands for five software design principles, which seek to improve quality and maintainability in your code.

*some examples are just illustrative, they wouldn't really work*

## S -> Single Responsibility Principle
This principle states that a class needs to have only one responsibility in your system. This helps your system become easier to understand and maintain.

- Does not follow the principles:
```java
public class Board {
    public Piece[][] boardMatrix = new Piece[8][8];

    public void createPieces {/*...*/}
    
    public void updateBoards {/*...*/}

    public void changePiecePosition {/*..*/}
}
```
- Follow the principles:
```java
public class Board {
    public Piece[][] boardMatrix = new Piece[8][8];

    public void createPieces {/*...*/}
    
    public void updateBoards {/*...*/}
}

public class Piece {
    public Position position;

    public void changePiecePosition {/*..*/}
}

public class Position {
    public int positionX;
    public int positionY;

    public Position(int positionX, int positionY) {
        this.positionX = positionX;
        this.positionY = positionY;
    }
}
```

## O -> Open/Closed Principle
This principle states that software entities (classes, modules, etc.) must be open for extension, but closed for modification, that is, in a class it is possible to add any new resource without having to modify existing resources

- Does not follow the principles:
```java
public class ContractClt {
    public double salary() {/*...*/}
}

public class Trainee {
    public double aux() {/*...*/}
}

public class FolhaDePagamento {
    private double balance;

    public void calcular(Object functionary) {
        if (functionary instanceof ContractClt) {
            ContractClt contractClt = (ContractClt) functionary;
            this.balance = contractClt.salary();
        } else if (functionary instanceof Trainee) {
            Trainee trainee = (Trainee) functionary;
            this.balance = trainee.aux();
        }
    }
}

```
- Follow the principles:
```java
public interface Payable {
    double remuneration();
}

public class ContractClt implements Payable {
    public double remuneration() {/*..*/}
}

public class Trainee implements Payable {
    public double remuneration() {/*..*/}
}

public class Payroll {
    private double balance;

    public void calculate(Payable functionary) {
        this.balance = functionary.remuneration();
    }
}


```

## L -> Liskov Substitution Principle
It states that objects of a subclass must be able to replace objects of its base class without affecting the integrity of the program. In other words, we can use both the base class and the classes that are derived from this first because all follow the same template.

Does not follow the principles:
- Override/implement a method that does nothing;
```java
public class A {
    public String getName() {
        return "My name is A";
    }
}

public class B extends A {
    @Override
    public String getName() {
        // Don't do nothing
    }
}

public class Main {
    public static void printName(A object) {
        System.out.println(object.getName());
    }

    public static void main(String[] args) {
        A object1 = new A();
        A object2 = new B();

        printName(object1)
        printName(object2)
    }
}
```


- Throw an unexpected exception;
```java
public class A {
    public String getName() {
        return "My name is A";
    }
}

public class B extends A throw Exception {
    @Override
    public String getName(boolean haveExeption) {
        if (haveExeption) {
            throw new Exception("Exception message");
        }
        return "My name is B
    }
}

public class Main {
    public static void printName(A object) {
        System.out.println(object.getName());
    }

    public static void main(String[] args) {
        A object1 = new A();
        A object2 = new B();

        printName(object1)
        printName(object2)
    }
}
```


- Return values ​​of types other than the base class;
```java
public class A {
    public String getName() {
        return "My name is A";
    }
}

public class B extends A {
    @Override
    public int getName() {
        return 2
    }
}

public class Main {
    public static void printName(A object) {
        System.out.println(object.getName());
    }

    public static void main(String[] args) {
        A object1 = new A();
        A object2 = new B();

        printName(object1)
        printName(object2)
    }
}
```


- Follow the principles:
```java
public class A {
    public String getName() {
        return "My name is A";
    }
}

public class B extends A {
    @Override
    public String getName() {
        return "My name is B";
    }
}

public class Main {
    public static void printName(A object) {
        System.out.println(object.getName());
    }

    public static void main(String[] args) {
        A object1 = new A();
        A object2 = new B();

        printName(object1)
        printName(object2)
    }
}
```

## I -> Interface Segregation Principle
This principle suggests that a class's interfaces should not force the implementation of methods that are not relevant to that class. It is preferable to have several specific interfaces than one large generic interface.

- Does not follow the principles:
```java
public interface Birds {
    public void setLocation(double longitude, double latitude);
    public void setAltitude(double altitude);
    public void render();
}

public class Parrot implements Birds {
    @Override
    public void setLocation(double longitude, double latitude) {/*...*/}

    @Override
    public void setAltitude(double altitude) {/*...*/}

    @Override
    public void render() {/*...*/}
}

public class Penguin implements Birds {
    @Override
    public void setLocation(double longitude, double latitude) {/*...*/}

    @Override
    public void setAltitude(double altitude) {
        // Doesn't do anything, penguins are flightless birds
    }

    @Override
    public void render() {/*...*/}
}

```
- Follow the principles:
```java
public interface Birds {
    public void setLocation(double longitude, double latitude);
    public void render();
}

public interface BirdsThatFly extends Birds {
    public void setAltitude(double altitude);
}

public class Parrot implements BirdsThatFly {
    @Override
    public void setLocation(double longitude, double latitude) {/*...*/}

    @Override
    public void setAltitude(double altitude) {/*...*/}

    @Override
    public void render() {/*...*/}
}

public class Penguin implements Birds {
    @Override
    public void setLocation(double longitude, double latitude) {/*...*/}

    @Override
    public void render() {/*...*/}
}

```

## D -> Dependency Inversion Principle
This principle emphasizes dependence on abstractions, not implementations. This means that high-level modules should not depend on low-level modules, they should both depend on abstractions. And abstractions haven't depend details, bacause details have depend abstractions.

- Does not follow the principles:
```java
import yourpackage.MySQLConnection; 

class PasswordReminder {
    private MySQLConnection dbConnection;

    public PasswordReminder(MySQLConnection dbConnection) {
        this.dbConnection = dbConnection;
    }

    // Do something
}

```
- Follow the principles:
```java
public interface DBConnectionInterface {
    public void connect();
}

public class MySQLConnection implements DBConnectionInterface {
    @Override
    public void connect() {
        // ...
    }
}

public class OracleConnection implements DBConnectionInterface {
    @Override
    public void connect() {
        // ...
    }
}

public class PasswordReminder {
    private DBConnectionInterface dbConnection;

    public PasswordReminder(DBConnectionInterface dbConnection) {
        this.dbConnection = dbConnection;
    }
}

```
 ## References
[VIDEO](https://www.youtube.com/watch?v=mkx0CdWiPRA)
[SITE1](https://www.devmedia.com.br/entendendo-interfaces-em-java/25502)
[SITE2](https://medium.com/desenvolvendo-com-paixao/o-que-%C3%A9-solid-o-guia-completo-para-voc%C3%AA-entender-os-5-princ%C3%ADpios-da-poo-2b937b3fc530)
[SITE3](https://www.freecodecamp.org/portuguese/news/os-principios-solid-da-programacao-orientada-a-objetos-explicados-em-bom-portugues/)
[SITE3](https://luby.com.br/desenvolvimento/software/o-que-e-solid/)
