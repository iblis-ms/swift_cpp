# Exception

#### C++

```c++
void funWithException(int aArg) {
    if (aArg < 0){
        throw std::string("abc");
    }
    else {
        throw 1;
    }
}
void funWithoutException() noexcept {} // no exception -> better optimization
///
try {
	funWithException(-1);
} catch(const std::string& exception){ // captures: throw 	std::string("abc")
	std::cout<<"String exception: "<<exception<<std::endl;
} catch(int exception){ // captures: throw 1
	std::cout<<"Int exception: "<<exception<<std::endl;
}
```

#### Objective-C

```objective-c
@interface ClassABC : NSObject

- (void)funWithException:(int)aArg;
- (void)fun;

@end

@implementation ClassABC

- (void)funWithException:(int)aArg {
    if (aArg < 0) {
        @throw @"abc";
    }
    else {
        @throw [NSNumber numberWithInt:1];
    }
}

- (void)fun {
    @try {
        [self funWithException:1];
        
    } @catch (NSString* exception) { // captures: @throw @"abc";
        NSLog(@"Exception: %@", exception);
    } @catch(NSNumber* exception) { // [NSNumber numberWithInt:1];
        NSLog(@"Exception: %@", exception);
    } @finally { // called if there was or wasn't any exception
        NSLog(@"Finally");
    }
}
```

#### Swift

```kotlin
enum MyException: Error {
    case exceptionCase1
    case exceptionCase2
    case exceptionCase3
}

func funWithException(aArg : Int) throws {
    if aArg < 0 {
        throw MyException.exceptionCase1
    }
    else {
        throw MyException.exceptionCase2
    }
}

func fun() {
    do {
        try funWithException(aArg: 0)
    } catch MyException.exceptionCase1 {
        print("....")
    } catch MyException.exceptionCase2 {
        print("....")
    } catch { // must be empty catch to catch everything
        print("....")
    }
}
```

Exception with values:

```swift
enum MyException: Error {
    case exceptionCase1(value : String)
    case exceptionCase2(value1 : Int, value2 : Double)
    case exceptionCase3
}

func funWithException(aArg : Int) throws {
    if aArg < 0 {
        throw MyException.exceptionCase1(value: "abc")
    }
    else {
        throw MyException.exceptionCase2(value1: 1, value2: 1.2)
    }
}

func fun() {
    do {
        try funWithException(aArg: -2)
    } catch MyException.exceptionCase1(let value) {
        print("Case1: \(value)")
    } catch MyException.exceptionCase2(let value1, let value2) {
        print("Case2: \(value1) & \(value2)")
    } catch { // must be empty catch to catch everything
        print("something else")
    }
}
```

#### Kotlin

```

```

