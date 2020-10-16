# Design-Pattern-In-Swift

## Behavioral Design Pattern

### Observer 

```swift
protocol TestProtocol: class{
    func update(_ value: Int)
}

class Tasks {
    
    weak var observer: TestProtocol?
    
    var point: Int = 0{
        didSet{
            observer?.update(point)
        }
    }
}

class Observer: TestProtocol {
    func update(_ value: Int) {
        if value % 2 == 0 {
            print("Even")
        }else {
            print("Odd")
        }
    }
}

let observer = Observer()

let tasks = Tasks()

tasks.observer = observer

tasks.point = 6
```

### Iterator
```swift 
// Iterator Design Pattern

class Human{
    var firstname: String
    var lastname: String
    
    init(firstname: String, lastname: String) {
        self.firstname = firstname
        self.lastname = lastname
    }
}


protocol HumanIterator{
    func next() -> Human?
    func currentItem() -> Human?
    func isDone() -> Bool
    var count: Int{get}
}


protocol Aggregate{
    func getIterator() -> HumanIterator
    func getItem(_ index: Int) -> Human
}


class HumanAggregate: Aggregate {
    
    private var list: [Human] = []
    
    var count: Int {
        return list.count
    }
    
    func add(_ h: Human){
        list.append(h)
    }
    
    func pop() {
        if count > 0 {
            list.removeLast()
        }
    }
    
    func getIterator() -> HumanIterator {
        return Iterator(self)
    }
    
    func getItem(_ index: Int) -> Human {
        return list[index]
    }
}

class Iterator: HumanIterator {
    
    private var list: HumanAggregate
    
    var index: Int = 0
    
    var count: Int {
        return list.count
    }
    
    init(_ list: HumanAggregate) {
        self.list = list
    }
    
    func next() -> Human? {
        defer {
            index += 1
        }
        
        return isDone() ? list.getItem(index) : nil
    }
    
    func currentItem() -> Human? {
        return list.getItem(index)
    }
    
    func isDone() -> Bool {
        return index < count
    }
}

let hum = HumanAggregate()

hum.add(Human(firstname: "One", lastname: "last 1"))
hum.add(Human(firstname: "Tow", lastname: "last 2"))

let iterator = hum.getIterator()

while iterator.isDone() {
    print(iterator.currentItem()?.firstname)
    iterator.next()
}

```

### Strategy

```swift

protocol Strategy: class {
    func doAction(_ valueOne: Int, _ valueTwo: Int) -> Int
}

class AddOperation: Strategy {
    
    func doAction(_ valueOne: Int, _ valueTwo: Int) -> Int {
        valueOne + valueTwo
    }
   
}

class MultiplyOperation: Strategy {
    func doAction(_ valueOne: Int, _ valueTwo: Int) -> Int {
        valueOne * valueTwo
    }
}

class Action{
   private var action: Strategy
    
    init(action: Strategy) {
        self.action = action
    }
    
    func execute(valueOne: Int, valueTwo: Int) -> Int{
        action.doAction(valueOne, valueTwo)
    }
}

var actionA = Action(action: AddOperation())
actionA.execute(valueOne: 10, valueTwo: 10) // 20
actionA = Action(action: MultiplyOperation())
actionA.execute(valueOne: 10, valueTwo: 10) // 100
```

## Creational Design Pattern

### Facotory

```swift
enum Worker {
    case student
    case teacher
}

protocol School {
    var name: String? {get set}
    var id: Int? {get set}
    func say()
}

class Student: School{
    var name: String?
    
    var id: Int?
    
    func say() {
        print("I am a student. Name is \(name ?? "No Set"). Id is \(id ?? -1)")
    }
}


class Teacher: School {
    var name: String?
    
    var id: Int?
    
    func say() {
        print("I am a teacher. Name is \(name ?? "No Set"). Id is \(id ?? -1)")
    }
    
}

class Factory {
    var worker: Worker
    
    init(worker: Worker) {
        self.worker = worker
    }
    
    
    func create() -> School {
        switch worker {
        case .student:
           return Student()
        case .teacher:
           return Teacher()
        }
    }
}

var obj1 = Factory(worker: .student).create()
obj1.id = 123
obj1.name = "EA Rashel"
obj1.say()

var obj2 = Factory(worker: .teacher).create()
obj2.id = 321
obj2.name = "Teacher"
obj2.say()
```

> A factory pattern is a creational pattern. A strategy pattern is an operational pattern. Put another way, a factory pattern is used to create objects of a specific type. A strategy pattern is use to perform an operation (or set of operations) in a particular manner. In the classic example, a factory might create different types of Animals: Dog, Cat, Tiger, while a strategy pattern would perform particular actions, for example, Move; using Run, Walk, or Lope strategies.
