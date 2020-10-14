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
