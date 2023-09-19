# Stack & Queue

## Stack

LIFO(Last-ln-First-0ut) -  후입선출

- `push()` : 스택의 하단에 요소를 추가
- `pop()` : 스택 상단의 요소를 꺼내서 (삭제한 뒤) 반환
- `peak()` : 스택 상단의 요소를 꺼내서 (삭제하지 않고) 반환
- `count` : 스택에 포함된 요소의 수를 반환
- `isEmpty()` : 스택에 포함된 요소가 없는 경우 true / 그렇지 않은 경우 false 반환
- `isFull()` : 스택에 포함될 요소의 수가 결정돼 있는 경우, 스택이 꽉 찼으면 true 반환 / 그렇지 않은 경우 false 반환

```swift
public struct Stack<T> {
    private var elements = [T]()

    public init() {}
    
    public mutating func pop() -> T? {
        return self.elements.popLast()
    }
    
    public mutating func push(_ element: T) {
        self.elements.append(element)
    }
    
    public func peak() -> T? {
        return self.elements.last
    }
    
    public var isEmpty: Bool {
        return self.elements.isEmpty
    }
    
    public var count: Int {
        return self.elements.count
    }
}

var myStack = Stack<Int>()
myStack.push(1) // [1]
myStack.push(2) // [1, 2]
myStack.push(3) // [1, 2, 3]

var x = myStack.pop() // x = 1
x = myStack.pop() // x = 2
x = myStack.pop() // x = 3
x = myStack.pop() // nil
myStack.isEmpty // true
```

## Queue

FIFO(First-ln-First-0ut) - 선입선출

- `enqueue()` : 큐의 맨 뒤에 새로운 요소를 추가
- `dequeue()` : 큐의 맨 첫 번째 요소를 제거한 뒤 반환
- `peak()` : 큐의 첫 번째 요소를 반환하되, 제거하지는 않음
- `clear()` : 큐를 재설정해 빈 상태가 되게 함
- `count` : 큐에 있는 요소의 수를 반환
- `isEmpty()` : 큐가 비어있으면 true 반환 / 그렇지 않으면 false 반환
- `isFull()` : 큐가 꽉 차있으면 true 반환 / 그렇지 않으면 false 반환
- `capacity` : 큐 용량을 가져오거나 설정하기 위한 read/write 프로퍼티
- `insert(_: atIndex:)` : 큐의 특정 인덱스 위치에 요소를 삽입
- `removeAtIndex(atIndex:)` : 큐의 특정 인덱스 위치에 있는 요소를 제거

```swift
public struct Queue<T> {
    private var data = [T]()
    public init() {}
    
    public mutating func enqueue(_ element: T) {
        data.append(element)
    }

    public mutating func dequeue() -> T? {
        return data.removeFirst()
    }

    public func peak() -> T? {
        return data.first
    }

    public mutating func clear() {
        data.removeAll()
    }

    public var count: Int {
        return data.count
    }

    public func isEmpty() -> Bool {
        return data.isEmpty
    }

    public func isFull() -> Bool {
        return count == data.capacity
    }
    
    public var capacity: Int {
        get {
            return data.capacity
        }
        set {
            data.reserveCapacity(newValue)
        }
    }

    public mutating func insert(_ element: T, atIndex: Int) {
        data.insert(element, at: atIndex)
    }

    public mutating func removeAtIndex(atIndex: Int) {
        data.remove(at: atIndex)
    }
}

var myQueue = Queue<Int>()
myQueue.enqueue(1) // myQueue : [1]
myQueue.enqueue(2) // myQueue : [1, 2]
myQueue.enqueue(3) // myQueue : [1, 2, 3]
myQueue.enqueue(4) // myQueue : [1, 2, 3, 4]

let x = myQueue.dequeue() // 1, myQueue : [2, 3, 4]
let y = myQueue.peak() // 2, myQueue : [2, 3, 4]
myQueue.insert(1, atIndex: 0) // 1, myQueue : [1, 2, 3, 4]
myQueue.removeAtIndex(atIndex: 0) // 1, myQueue : [2, 3, 4]
```
