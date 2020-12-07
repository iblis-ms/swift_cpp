

```swift
class ClassABC{
    var fieldA = 0
    var fieldB = 0
    // all fields need to be initialized if no 'init' function
    
}

var item1 = ClassABC()
var item2 = ClassABC()

item1.fieldA = 1
item2.fieldA = 2

print("item1.fieldA=\(item1.fieldA)") // 1
print("item2.fieldA=\(item2.fieldA)") // 2

var item3 = item1 // copy
item3.fieldA = 3
print("item2.fieldA=\(item2.fieldA)") // 2
print("item3.fieldA=\(item3.fieldA)") // 3

```



```swift
class ClassABC{
    var fieldA = 0
    var fieldB = 0
    // all fields need to be initialized if no 'init' function
    
    func add(_ addA:Int, _ addB:Int){
        self.fieldA += addA // 'self' is like 'this' in C++
        fieldB += addB // without 'self' but not a problem because there isn't any other fieldB in this scope
    }
    
    func print(){
        Swift.print("fieldA=\(fieldA)") // required Swift to call function from global scope, like :: in C++
        Swift.print("fieldB=\(fieldB)")
    }
}


var item4 = ClassABC()
item4.print()
item4.add(10, 20)
item4.print()
```



# Struct 

```swift
struct ClassABC { // works only for struct
    var fieldA = 0
    var fieldB:Int
    let fieldC:Int
}

var item1 = ClassABC(fieldA: 10, fieldB: 20, fieldC: 30)

item1.fieldA = 1
item1.fieldB = 2
  
let item2 = ClassABC(fieldA: 10, fieldB: 20, fieldC: 30)
// item2.fieldA = 100 // ERROR: because item2 is const

```

# Lazy initialization

```swift
import Foundation

class Item{
    var itemA:Int = 0
    init() { // must be without args to usage with 'lazy'
        print("Item")
    }
}

class Class{
    lazy var classItemA = Item()
}

var cl1 = Class()// classItemA is not initilized now
print("--------")
print("\(cl1.classItemA.itemA)") // first usage, so classItemA is initialized and value returned

```

# Calculate property on-fly

```swift
import Foundation

class Square{
    var size = 0.0
}

class SquareMath {
    var square = Square()
    var area:Double {
        get{ // get that calculates on fly
            return square.size * square.size
        }
        set(newArea){ // set square's size via 'fake' setter; if empty name: newValue name is avaible
            square.size = newArea.squareRoot()
        }
    }
    var perimeter:Double{ // read only property
        return 4.0 * square.size
    }
}

var item = SquareMath()
item.area = 9.0
print("size: \(item.square.size)") // size: 3.0
print("area: \(item.area)") // area: 9.0
print("perimeter: \(item.perimeter)")
// item.perimeter = 30 // ERROR: read only property
```

Property - observers

```swift
class ClassABC {
    var item = 1 {
        willSet(newValue) { // observer called before change; newValue is constant
            print("willSet newValue: \(newValue) oldValue: \(item)")
            item = 10 * newValue // WARN: it doesn't make sense, it will be overwritten
        }
        didSet { // observer called after change
            print("didSet newValue: \(item) oldValue: \(oldValue)")
            item = 100 * oldValue
        }
    }
}

var obj = ClassABC()
obj.item = 2
// willSet newValue: 2 oldValue: 1
// didSet newValue: 2 oldValue: 1
print("obj.item=\(obj.item)")
// obj.item=100
```

# Restrictions

```swift
import Foundation

@propertyWrapper // constraint class
struct ColorConstraint {
    private var value: UInt // value itself
    var projectedValue: String = "no"  // projectedValue (user defined type) gives additional info see $red
    
    init() {
        self.value = 0 // init is called as in normal class
    }
    
    init(wrappedValue: UInt) {
        self.value = wrappedValue // init is called as in normal class, but arg. wrappedValue is required
    }
    
    var wrappedValue: UInt { // property with restriction
        get {
            return value
        }
        set {
            value = min(newValue, 255) // just limit value to 255
            projectedValue = (value != 0 ? "True" : "False") // if non-zer, mark that property is OK
        }
    }
}

struct RGB {
    @ColorConstraint var red: UInt // @name of constraint defined above
    @ColorConstraint var green: UInt
    @ColorConstraint var blue: UInt = 0
}

var rgb = RGB()
print("Red value: \(rgb.red) Ready: \(rgb.$red)")
rgb.red = 100
print("Red value: \(rgb.red) Ready: \(rgb.$red)")
```

Static 

```swift

struct MyStruct {
    static var staticProperty = "Struct."
    static var staticCalcProperty: Int {
        return 1
    }
}

enum MyEnum {
    static var staticProperty = "Enum."
    static var staticCalcProperty: Int {
        return 6
    }
}

class MyClass {
    static var staticProperty = "Class."
    static var staticCalcProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}

print("staticProperty=\(MyStruct.staticProperty) staticCalcProperty=\(MyStruct.staticCalcProperty)")
print("staticProperty=\(MyEnum.staticProperty) staticCalcProperty=\(MyEnum.staticCalcProperty)")
print("staticProperty=\(MyClass.staticProperty) staticCalcProperty=\(MyClass.staticCalcProperty)")
print("staticProperty=\(MyClass.staticProperty) overrideableComputedTypeProperty=\(MyClass.overrideableComputedTypeProperty)")
MyStruct.staticProperty = "new Struct value"
print("staticProperty=\(MyStruct.staticProperty) staticCalcProperty=\(MyStruct.staticCalcProperty)")
```

# Struct vs Class

`struct

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) { // struct requires mutating keyword for function that changes fields
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0) // availble for struct
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"

```

class - that does the same

```swift
class Point {
    var x = 0.0, y = 0.0
    init(x:Double, y:Double){
        self.x = x
        self.y = y
    }
     func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"

```

Order of init call

Opposite of C++ constructor call order

```swift

class ItemBase{
    init(){
        print("ItemBase init")
    }
}

class Base{
    var itemBase = ItemBase()
    init() {
        print("Base init")
    }
}

class ItemDerivative{
    init(){
        print("ItemDerivative init")
    }
}

class Derivative : Base{
    var item = ItemDerivative()
    override init(){
        print("Derivative init")
    }
}

var obj = Derivative()
/*
 Completely different approach:
ItemDerivative init
Derivative init
ItemBase init
Base init
*/

```

If initialiation of fileds is done in init:

```swift
import Foundation

class ItemBase{
    init(){
        print("ItemBase init")
    }
}

class Base{
    var itemBase:ItemBase
    init() {
        print("Base init")
        itemBase = ItemBase()
    }
}

class ItemDerivative{
    init(){
        print("ItemDerivative init")
    }
}

class Derivative : Base{
    var item:ItemDerivative
    override init(){
        print("Derivative init")
        item = ItemDerivative()
    }
}

var obj = Derivative()
/*
 Completely different approach:
 Derivative init
 ItemDerivative init
 Base init
 ItemBase init
*/

```

Calling init from init

```swift
class ClassABC {
    var field1:Int
    var field2:Int
    
    init(arg1:Int, arg2:Int){
        field1 = arg1
        field2 = arg2
    }
    
    convenience init(arg1:Int){ // convenience is required for class; not for struct
        self.init(arg1:arg1, arg2: arg1)
    }
}

var obj = ClassABC(arg1: 1)
print("field1=\(obj.field1) field2=\(obj.field2)")

```

