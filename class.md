# Class

### C++

In C++ struct & class are almost the same. The difference is that struct has default public access and default public inheritance, but class private.

```c++
class ClassABC{
public:
    int fieldA;
    int fieldB;
};

ClassABC item1;
ClassABC item2;

item1.fieldA = 1;
item2.fieldA = 2;

printf("item1.fieldA=%d\n", item1.fieldA);
printf("item2.fieldA=%d\n", item2.fieldA);

ClassABC item3(item1);
item3.fieldA = 3;

printf("item1.fieldA=%d\n", item1.fieldA); // 1
printf("item3.fieldA=%d\n", item3.fieldA); // 3

```

### Objective-C

```objective-c
#import <Foundation/Foundation.h>
@interface ClassABC : NSObject<NSCopying> // protocol NSCopying for copy object

@property int fieldA;
@property int fieldB;

@end

@implementation ClassABC

- (nonnull instancetype)copyWithZone:(nullable NSZone *)zone { // NSCopying's method
    ClassABC *newObj = [ClassABC new];
    newObj.fieldA = self.fieldA;
    newObj.fieldB = self.fieldB;
    return newObj;
}

@end
  
ClassABC* item1 = [[ClassABC alloc] init];
ClassABC* item2 = [ClassABC new]; // or in shorter way

item1.fieldA = 1; // like property
[item2 setFieldA: 2]; // by setter

NSLog(@"item1.fieldA=%d\n", [item1 fieldA]); // 1
NSLog(@"item2.fieldA=%d\n", item2.fieldA); // 2

ClassABC* item3 = [item1 copy]; // ClassABC* item3 = item1 // changing pointer only
item3.fieldA = 3; // like property

NSLog(@"item1.fieldA=%d\n", item1.fieldA); // 1
NSLog(@"item3.fieldA=%d\n", item3.fieldA); // 3
```

### Swift

In Swift struct != class. 

Struct:

```swift
struct ClassABC{
    var fieldA = 0
    var fieldB = 0
    // all fields need to be initialized if no 'init' function
    
}

// ///////////////////////////////////////////////

var item1 = ClassABC()
var item2 = ClassABC()

item1.fieldA = 1
item2.fieldA = 2

print("item1.fieldA=\(item1.fieldA)") // 1
print("item2.fieldA=\(item2.fieldA)") // 2

var item3 = item1 // copy
item3.fieldA = 3
print("item1.fieldA=\(item1.fieldA)") // 1
print("item3.fieldA=\(item3.fieldA)") // 3

```

Class:

```swift
class ClassABC : NSCopying { // NSCopying protocol
    var fieldA = 0
    var fieldB = 0
    // all fields need to be initialized if no 'init' function
    
    func copy(with zone: NSZone? = nil) -> Any { // required by NSCopying
        let ret = ClassABC()
        ret.fieldA = self.fieldA
        ret.fieldB = self.fieldB
        return ret
    }
}

// ///////////////////////////////////////////////

var item1 = ClassABC()
var item2 = ClassABC()

item1.fieldA = 1
item2.fieldA = 2

print("item1.fieldA=\(item1.fieldA)") // 1
print("item2.fieldA=\(item2.fieldA)") // 2

var item3 = item1.copy() as! ClassABC // copy: as! required because copy return Any
item3.fieldA = 3
print("item1.fieldA=\(item1.fieldA)") // 1
print("item3.fieldA=\(item3.fieldA)") // 3
```

The copying approach for class in Swift is similar to Objective-C way.

#### Kotlin

```kotlin
class ClassABC{
    var fieldA:Int
    var fieldB:Int

    constructor(fieldA:Int, fieldB:Int) {
        this.fieldA = fieldA
        this.fieldB = fieldB
    }
}
// ///////////////////
var obj = ClassABC(1,2)
println("obj.fieldA=${obj.fieldA} obj.fieldB=${obj.fieldB}")

```

Or ....

```kotlin
class ClassABC(var fieldA:Int, var fieldB:Int){ // if there is one constructor, everything can be in header
}

var obj = ClassABC(1,2)
println("obj.fieldA=${obj.fieldA} obj.fieldB=${obj.fieldB}")
```

constructors' hell

```kotlin
class ClasABC { // no 'primary' constructor
    var field:Int

    constructor(arg1:Int, arg2:Int) { // 'secondary' constructor
        field = arg1 + arg2
        println("This is constructor")
    }

    constructor(arg1:Int, arg2:Int, arg3:Int){ // 'secondary' constructor
        field = arg1 + arg2 + arg3
    }
}
```

with primary constructor

```kotlin
class ClasABC(var arg:Int) { // primary constructor
    var field:Int = arg

    constructor(arg1:Int, arg2:Int) : this(arg1 + arg2){ // primary must be called
    }

    constructor(arg1:Int, arg2:Int, arg3:Int) : this(arg1 + arg2 + arg3){// as above
    }
}

val obj1 = ClasABC(1)
val obj2 = ClasABC(1, 2)
val obj3 = ClasABC(1, 2, 3)
```



# Class methods

### C++

```c++
class ClassABC{
public:
    int fieldA;
    int fieldB;
    
    void add(int addA, int addB){
        this->fieldA = addA;
        fieldB = addB;
    }
    
    void print(){
        printf("fieldA=%d\n", fieldA);
        printf("fieldA=%d\n", fieldB);
    }
};

// ///////////////////////////////////////////////

ClassABC item4;
item4.print();
item4.add(10, 20);
item4.print();
```

### Objective-C

```objective-c

@interface ClassABC : NSObject

@property int fieldA;
@property int fieldB;

- (void) add: (int)addA and: (int)addB;
- (void)print;

@end

@implementation ClassABC

- (void)add: (int)addA and: (int)addB {
    self.fieldA += addA;
    self.fieldB += addB;
}

- (void)print {
    printf("fieldA=%d\n", self.fieldA);
    printf("fieldA=%d\n", self.fieldB);
}

@end

// ///////////////////////////////////////////////

ClassABC* item1 = [[ClassABC alloc] init];
[item1 print];
[item1 add:10 and:20];
[item1 print];
```

### Swift

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

// ///////////////////////////////////////////////

var item4 = ClassABC()
item4.print()
item4.add(10, 20)
item4.print()
```

#### Kotlin

```kotlin
class ClassABC{
    private var fieldA:Int
    private var fieldB:Int

    constructor(fieldA:Int, fieldB:Int) {
        this.fieldA = fieldA
        this.fieldB = fieldB
    }

    fun add(addA:Int, addB:Int) {
        fieldA += addA
        fieldB += addB
    }

    fun print() {
        println("fieldA=$fieldA")
        println("fieldB=$fieldB")
    }
}
// ///////////////////
var obj = ClassABC(0,0)
obj.add(10, 20)
obj.print()
```

or 

```kotlin
class ClassABC(var fieldA:Int, var fieldB:Int){ // if there is one constructor, everything can be in header

    fun add(addA:Int, addB:Int) {
        fieldA += addA
        fieldB += addB
    }

    fun print() {
        println("fieldA=$fieldA")
        println("fieldB=$fieldB")
    }
}
// ///////////////////
var obj = ClassABC(0,0)
obj.add(10, 20)
obj.print()
```

# Swift Struct 

```swift
import Foundation

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

item1 = item2 // deep copy because of structure
item1.fieldA = 113
print("item1.fieldA=\(item1.fieldA)")
print("item2.fieldA=\(item2.fieldA)")

```

# Lazy initialization

#### C++

C++ doesn't have language support for lazy initialization, but it could done as below (getter example):

```c++
class Item{
public:
    int itemA = 0;
};

class Class {
    Item* classItemAInternal = nullptr;
public:
    
    Item* classItemA()
    {
        if (classItemAInternal == nullptr) {
            classItemAInternal = new Item();
        }
        return classItemAInternal;
    }
};
```

Objective-C

```objc

@interface Item : NSObject

@property int itemA;

@end

@implementation Item
@end


@interface ClassA : NSObject

@property(nonatomic) Item *classItemA;

@end

@implementation ClassA

- (Item *)classItemA {
    if (_classItemA == nil) {
        _classItemA = [Item new];
    }
    return _classItemA;
}

@end
```

Swift

```swift
import Foundation

class Item{
    var itemA:Int = 0
    init() { // must be without args to usage with 'lazy'
        print("Item")
    }
}

class Class{
    lazy var classItemA = Item() // lazy can be used only with var - not with let
}

var cl1 = Class()// classItemA is not initilized now
print("--------")
print("\(cl1.classItemA.itemA)") // first usage, so classItemA is initialized and value returned

```

#### Kotlin

```kotlin
class Item{
    var itemA:Int = 0
    constructor(itemA:Int) {
        println("Item")
        this.itemA = itemA
    }
}

class Class{
    val classItemA:Item by lazy {
        println("calculate")
        Item(12) // last statement is initialization
    }
}
// ////////////////////
val obj = Class();
println("${obj.classItemA.itemA}")
```

# Calculate property on-fly

C++ & Objective-C implementation should follow approach as was written in 'lazy' initialization chapter. Swift has its own languages features for this.

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

#### Kotlin

```kotlin
class ClassABC {
    var item:Int  by Delegates.observable(1) { // 1 is initial value
            prop, old, new ->
        println("$old -> $new")
    }
}
// /////////////
val obj = ClassABC();
obj.item = 2
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

# Static 

C++

```c++
class Class {
public:
    //declaration of static variables
    static const std::string staticConstVar;
    static std::string staticVar;
    // static function
    static void fun(){}
    
};
// definition of static variables
std::string Class::staticVar = "static";
const std::string Class::staticConstVar = "const static";

// ///////////////////////
const std::string& constVar = Class::staticConstVar;
std::string& var = Class::staticVar;
Class::staticVar = "new value";

Class::fun();
```

#### Objective-C

```objc

@interface ClassA : NSObject // Class is used in Foundation framework, so ClassA

// attribute class 
@property (class) NSString* staticVar;
@property (class, readonly) NSString* staticConstVar; // however, NSString is nonmutable

+ (void)fun;
@end

@implementation ClassA


static NSString* _staticVar = @"static";
static const NSString* _staticConstVar = @"const static";

+ (void)fun {
}

// attribute class requires writing getters.
+ (const NSString*)staticConstVar {
  return _staticConstVar;
}

+ (NSString*)staticVar {
  return _staticVar;
}

+ (void)setStaticVar:(const NSString *)newValue {
    _staticVar = [newValue copy];
}
@end

// /////////////////////////
const NSString* constVar = ClassA.staticConstVar;
NSString* var = ClassA.staticVar;
ClassA.staticVar = @"new value";
```

#### Swift

```swift
struct Class { // or class 
    static var staticProperty = "static"
    static let staticConstProperty = "static const"
    
    static func fun(){
    }
}

var var1 = Class.staticProperty
var var2 = Class.staticConstProperty

Class.staticProperty = "new value"

Class.fun()
```

static also for enum

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

Kotlin

```kotlin
class ClassABC{
    companion object { // 
        var thisIsStaticVar:Int = 0

        fun staticFun(){
            println("staticFun: $thisIsStaticVar")
        }
    }
}
ClassABC.thisIsStaticVar++;
ClassABC.staticFun() // print: staticFun: 1
```



# Struct vs Class

#### struct

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

# Inheritance

#### C++

```c++
class CBase {
private:
    int mBasePrivate;
protected:
    int mBaseProtected;
public:
    int mBasePublic;
};
// ////////////////////////
class CClass : public CBase {
private:
    int mInheritPrivate;
protected:
    int mInheritProtected;
public:
    int mInheritPublic;
    
    void fun(){
        // mBasePrivate = 0; // ERROR: 
        mBaseProtected = 1;
        mBasePublic = 2;
        
        mInheritPrivate = 3;
        mInheritProtected = 4;
        mInheritPublic = 5;
    }
};
// ////////////////////////
CClass obj;
obj.mBasePublic = 6;
obj.mInheritPublic = 7
```

Multiple base inheritance

```c++
class CBaseA {
public:
    int mBasePublic;
};
// ////////////////////////
class CBaseB {
public:
    int mBasePublic;
};
// ////////////////////////
class CClass: public CBaseA, public CBaseB {
public:
};
// ////////////////////////
CClass obj;
```

#### Objective-C

```c++
@interface CBase : NSObject{
    @private
    int _basePrivate;
    
    @protected
    int _baseProtected;
    
    @public
    int _basePublic;
}

@property int baseProperty;

@end

@implementation CBase

@end
// ///////////////////////
@interface CClass : CBase {
    @private
    int _classPrivate;

    @protected
    int _classProtected;

    @public
    int _classPublic;
}

@property int classProperty;

- (void)fun;

@end

@implementation CClass

- (void)fun{
    super.baseProperty = 0;
    _baseProtected = 1;
    _basePublic = 2;
    // _classPrivate = 3; // ERROR: it is not visible
    
    _classPrivate = 4;
    _classProtected = 5;
    _classPublic = 6;
    self.classProperty = 7;
}
@end
// /////////////////////
CClass* ptr = [CClass new];
ptr.classProperty = 0;
ptr.baseProperty = 1;
```

#### Swift

```swift
class CBase {
    public var basePublic1 = 0
    var basePublic2 = 1
    fileprivate var baseFilePrivate = 2
    private var basePrivate = 3
}
// /////////////////////
class CClass : CBase {
    public var classPublic1 = 0
    var classPublic2 = 1
    fileprivate var classFilePrivate = 2
    private var classPrivate = 3
    
    func fun() {
        self.basePublic1 = 10
        self.basePublic2 = 20
        self.baseFilePrivate = 30
        // self.basePrivate = 40 // ERROR because not visible in CClass
        self.classPublic1 = 11
        self.classPublic2 = 12
        self.classFilePrivate = 13
        self.classPrivate = 14
    }
}
// /////////////////////
var obj = CClass()
var a = 0
a = obj.basePublic1
a = obj.basePublic2
a = obj.baseFilePrivate
// a = obj.basePrivate // ERROR: because of private
a = obj.classPublic1
a = obj.classPublic2
a = obj.classFilePrivate
a = obj.classPrivate
```

#### Kotlin

```kotlin
open class Base { // in Kotlin classes are 'final' by default
    public var basePublic = 0
    protected var baseProtected = 0
    private var basePrivate = 0
}
// //////////////////////////////
class Class : Base() {
    public var classPublic = 0
    protected var classProtected = 0
    private var classPrivate = 0

    fun funABC(){
        basePublic = 1
        baseProtected = 2
        // basePrivate = 3 // ERROR: not visible
        classPublic = 4
        classProtected = 5
        classPrivate = 6

    }
}
//////////////////////////////
val obj = Class()
obj.classPublic = 10
obj.basePublic = 11
```

# Order of init call

Opposite of C++ constructor/init call order

#### C++

```c++
class ItemBase{
public:
    ItemBase(){
        std::cout<<"ItemBase init\n";
    }
};

class Base{
    ItemBase itemBase;
public:
    Base(){
        std::cout<<"Base init\n";
    }
};

class ItemDerivative{
public:
    ItemDerivative(){
        std::cout<<"ItemDerivative init\n";
    }
};

class Derivative : public Base{
    ItemDerivative item;
public:
    Derivative(){
        std::cout<<"Derivative init\n";
    }
};
// ///////////////////
Derivative obj;
/*
ItemBase init
Base init
ItemDerivative init
Derivative init
*/
```

#### Objective-C

```objc
@interface ItemBase : NSObject
- (instancetype)init;
@end

@implementation ItemBase

- (instancetype)init{
    NSLog(@"ItemBase init");
    self = [super init];
    if (self) {
    }
    return self;
}

@end

@interface Base : NSObject

@property(nonatomic) ItemBase *itemBase;

- (instancetype)init;

@end

@implementation Base

- (instancetype)init{
    NSLog(@"Base init");
    self = [super init];
    if (self) {
        _itemBase = [ItemBase new];
    }
    return self;
}

@end


@interface ItemDerivative : NSObject
- (instancetype)init;
@end

@implementation ItemDerivative

- (instancetype)init{
    NSLog(@"ItemDerivative init");
    self = [super init];
    if (self) {
    }
    return self;
}

@end


@interface Derivative : Base

@property(nonatomic) ItemDerivative *item;

- (instancetype)init;

@end

@implementation Derivative

- (instancetype)init{
    NSLog(@"Derivative init");
    self = [super init]; // developer needs to call base class init  - is not done automatically as in C++
    if (self) {
        _item = [ItemDerivative new];
    }
    return self;
}

@end

// ///////////////////////////////////
Derivative *obj = [Derivative new];
/*
2020-12-09 18:57:23.689746+0100 ObjectiveC_Test[14497:2345444] Derivative init
2020-12-09 18:57:23.689791+0100 ObjectiveC_Test[14497:2345444] Base init
2020-12-09 18:57:23.689823+0100 ObjectiveC_Test[14497:2345444] ItemBase init
2020-12-09 18:57:23.689852+0100 ObjectiveC_Test[14497:2345444] ItemDerivative init
*/
```

#### Swift

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
        super.init() // if written, it must be called after this class properties initialization
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

Kotlin

```kotlin
// ///////////////////////
open class Base { // in Kotlin classes are 'final' by default
    var baseFieldA:ItemBase = ItemBase()

    constructor(){
        println("Base constructor")
    }
}
// ///////////////////////
class ItemDerivative{
    constructor(){
        println("ItemDerivative constructor")
    }
}
// ///////////////////////
class Derivative : Base {
    var item:ItemDerivative = ItemDerivative()
    constructor(){
        println("Derivative constructor")
    }
}
// ///////////////////////
val obj1 = Derivative()
/* C++/Java order
ItemBase constructor
Base constructor
ItemDerivative constructor
Derivative constructor
 */
```

# Final - prevent inheritance and override

C++ (C++ 11 feature)

```c++
struct Base {
    virtual ~Base() = default;
    
    virtual void fun() = 0;
};

struct DerLevel1 : Base {
    
    virtual void fun() final {
        
    }
};

struct DerLevel2 : DerLevel1 {
    
    virtual void fun() {} // ERROR: because DerLevel1::fun is final

};

struct DerLevel2_final final : DerLevel1 {
};

struct DerLevel3 : DerLevel2_final { // ERROR: because DerLevel2_final is final
};
```

#### Objective-C

```

```

#### Swift

```swift
class Base {
    
    func fun() {
        print("fun")
    }
}

class Der : Base {
    
    override final func fun()  {
        print("Der fun")
    }
}

final class Der2 : Der{
    override func fun() { // ERROR: fun is final in Der
        print("Der2 fun")
    }
}

class Der3 : Der2 { // ERROR: Der2 is final
}
```

#### Kotlin

Classes in Kotlin are by default final.

# Class Interface required by function

#### C++

```c++
struct MyInterface{
    virtual ~MyInterface() = default;
    virtual void onCall(const std::string& aMessage) = 0;
};
// //////////////////
struct ClassABC : MyInterface {
    void onCall(const std::string& aMessage){
    }
};
// //////////////////
void fun(MyInterface& aInterface){
    aInterface.onCall("abc");
}
// //////////////////
ClassABC obj;
```

#### Objective-C - protocols

```objective-c
@protocol MyInterface <NSObject>

- (void)onCall:(NSString*) aMessage;

@end
// //////////////////
@interface ClassABC : NSObject<MyInterface>
- (void)onCall:(NSString*) aMessage;
@end
// //////////////////
@implementation ClassABC

- (void)onCall:(NSString*) aMessage {
    NSLog(@"OnCall");
}

@end
// //////////////////

void fun(NSObject<MyInterface>* aInterface)  // NSObject
{
    [aInterface onCall:@"message"];
}
// //////////////////

ClassABC* ptr = [ClassABC new];
fun(ptr);
```

Using id with protocol.

```objective-c
@protocol MyInterface

- (void)onCall:(NSString*) aMessage;

@end
// //////////////////
@interface ClassABC : NSObject<MyInterface>
- (void)onCall:(NSString*) aMessage;
@end
// //////////////////
@implementation ClassABC

- (void)onCall:(NSString*) aMessage {
    NSLog(@"OnCall");
}

@end
// //////////////////

void fun(id<MyInterface> aInterface)
{
    [aInterface onCall:@"message"];
}
// //////////////////

ClassABC* ptr = [ClassABC new];
fun(ptr);
```

#### Swift

Class:

```swift
protocol MyInterface
{
    func onCall(aMessage : String)
}
// //////////////////////
class ClassABC : MyInterface {
    func onCall(aMessage: String) {
        print(aMessage)
    }
}
// //////////////////////
func fun(_ aInterface : MyInterface){
    aInterface.onCall(aMessage: "message")
}
// //////////////////////
var obj = ClassABC()
fun(obj)
```

Struct:

```swift
protocol MyInterface
{
    mutating func onCall(aMessage : String) // mutating required for struct; However, protocol would work with class
}
// //////////////////////
struct ClassABC : MyInterface {
    var field = 0
    mutating func onCall(aMessage: String) { // mutating because of struct
        print(aMessage)
        field = aMessage.count // changing state
    }
}
// //////////////////////
func fun(_ aInterface :  inout MyInterface){
    aInterface.onCall(aMessage: "message")
}
// //////////////////////
var obj: MyInterface = ClassABC() // type of protocol required
fun(&obj)
```

Kotlin

```kotlin
interface MyInterface{
    fun onCall(aMessage : String)
}
// //////////////////////
class ClassABC : MyInterface{
    override fun onCall(aMessage: String) {
        print(aMessage)
    }

}
// //////////////////////
fun funABC(aInterface : MyInterface){
    aInterface.onCall("abc")
}
// //////////////////////
val obj = ClassABC()
funABC(obj)
```

