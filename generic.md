# Generic functions

C++

```c++
template<typename T>
void mySwap(T& aArg1, T& aArg2)
{
    T temp(aArg1);
    aArg1 = aArg2;
    aArg2 = temp;
}
// /////////////////////////////
int aaa = 1;
int bbb = 2;
mySwap(aaa, bbb);
```

Objective-C

```

```

Swift

```swift
func mySwap<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var aaa = 1
var bbb = 2

mySwap(&aaa, &bbb)
```

# Generic class

C++

```c++
template<typename T>
class ClassABC{
public:
    T mField;
    
    ClassABC(const T& aArg)
    : mField(aArg){
    }
    
    T justAssignAndReturn(const T& aArg) {
        mField = aArg;
        return mField;
    }
};
// ///////////////////////
ClassABC<std::string> obj("123");
std::string res = obj.justAssignAndReturn("abc");
std::cout<<res<<std::endl;

```

Objective-C

```objective-c
@interface ClassABC<T> : NSObject {
@private T _field;
}

- (instancetype)initWith:(T)aArg;
- (T)justAssignAndReturn:(T)aArg;
@end

@implementation ClassABC

- (instancetype)initWith:(id)aArg // here is id instead of T like in interface...
{
    if ((self = [super init])){
        _field = aArg;
    }
    return self;
}

- (id)justAssignAndReturn:(id)aArg {
    _field = aArg; // shallow copy
    return _field;
}

@end
// /////////////////////////
ClassABC<NSString*>* obj = [[ClassABC alloc] initWith: @"123"];
NSString* res = [obj justAssignAndReturn:@"abc"];
NSLog(@"res=%@", res);
```

Swift

```swift
class ClassABC<T>{
    var field:T
    
    init(_ arg:T) {
        field = arg
    }
    
    func justAssignAndReturn(_ arg:T) ->  T {
        field = arg
        return field
    }
}

var obj = ClassABC<String>("123")
var res = obj.justAssignAndReturn("abc")
print("res=\(res)")
```

# Generic type restrictions

C++11

```c++
template<typename T, std::enable_if_t<std::is_integral<T>::value, bool> = true> // tooooo complex
void function(const T& aValue) {
    std::cout<<"Integer: "<<aValue<<std::endl;
}

function(1); 
function("abc"); // ERROR: constraints not satisfied
```

C++20 (Constraints) - type related concept 

```c++
template <class T>
concept IntegerConcept = std::is_integral<T>::value;

template<IntegerConcept T>
void function(const T& aValue) {
    std::cout<<"Integer: "<<aValue<<std::endl;
}

function(1); 
function("abc"); // ERROR: constraints not satisfied
```

C++20 (Constraints) - function related concept

```c++
template<class T>
concept SthWithSize = requires(T aArg){
    { aArg.size() } -> std::convertible_to<std::size_t>;
};

template<SthWithSize T>
void functionCollection(const T& aArg){
    std::cout<<"size: "<<aArg.size()<<std::endl;
}

std::vector<int> v{1, 2, 3};
functionCollection(v);
std::string str;
functionCollection(str);
const char* ptr = "abc";
functionCollection(ptr); // ERROR: doesn't have size()
```

Objective-C

```

```

