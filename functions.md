# Simplest function format

C++

```c++
void fun() {
    printf("abc\n");
}

fun();
```

Swift

```swift
func fun(){
    print("abc")
}

fun()
```

Kotlin

```kotlin
fun funABC(){ // Kotlin: fun; Swift: func... ehh
    print("abc")
}

funABC()
```

# Function with arguments

C++

```c++
void fun(const char* arg1, int arg2) {
    printf("arg1=%s arg2=%d", arg1, arg2);
}

fun("A", 1);
```

Swift

```swift
func fun(arg1: String, arg2: Int){
    print("arg1=\(arg1) arg2=\(arg2)")
}

fun(arg1: "A", arg2: 1)
```

In Swift argument names are treated as label while calling function. Therefore, the function written as above requires to put labels. However, Swift allows to write a custom labels. Labels don't have to be written for all arguments.

```swift
func fun(thisIsLabel1 arg1: String, thisIsLabel2 arg2: Int){
    print("arg1=\(arg1) arg2=\(arg2)") // argument names are used
}

fun(thisIsLabel1: "A", thisIsLabel2: 1) // labels are used
```

There is also option to omit label completely.

```swift
func fun(_ arg1: String, _ arg2: Int){
    print("arg1=\(arg1) arg2=\(arg2)")
}

fun("A", 1)
```

Kotlin

```kotlin
fun funABC(arg1: String, arg2: Int){
    print("arg1=$arg1 arg2=$arg2")
}

funABC("A", 1)
funABC(arg1 = "A", arg2 = 1)
funABC(arg2 = 1, arg1 = "A")
```

# Return value

C++

```c++
float fun(int arg1) {
    return 1.5f * static_cast<float>(arg1);
}

float res = fun(1);
```

Swift

```swift
func fun(arg1: Int) -> Float{
    return 1.5 * Float(arg1)
}

var res:Float = fun(arg1: 1) // can be also: var res = fun(arg1: 1)
```

C++ from version 17 has [[nodiscard]] attribute to mark function that its return should be captured.

```c++
[[nodiscard]] int fun() {
    return 1;
}

fun(); // compilation warning becuase return value wasn't captured;
```

Swift works in exactly opposite way. By default, the compiler would show warning if returned value wasn't assigned. To clear this warnig following approach shall be used:

```swift
func fun() -> Int{
    return 1
}
let _ = fun()
```

or

```swift
@discardableResult
func fun() -> Int{
    return 1
}
fun()
```

Return without *return* keyword... syntax sugar

```swift
func fun() -> Int{
    123 // the same as: return 123
}

print("res=\(fun())")
```

Kotlin

```kotlin
fun funABC(arg1: Int) : Float{
    return 1.5F * arg1
}

var res = funABC(1)
```



## Return multiple values

C++

```c++
#include <tuple>

std::tuple<int, int> fun() // can be written: auto fun()
{
    return std::make_tuple(1, 2); // C++20: automatic type deduction
}

int res1 = 0, res2 = 0;
std::tie(res1, res2) = fun();
    
printf("res1=%d res2=%d\n", res1, res2);
```

Swift

```swift
func fun() -> (res1 : Int, res2 : Int){
    return (1, 2)
}

var res = fun()
print("res1=\(res.res1) res2=\(res.res2)")
```

# Return optional value

C++ 

```c++
#include <optional>

std::optional<int> fun(int arg){
    return arg > 0 ? arg : std::optional<int>{};
}

const auto res = fun(0);
printf("res=%s", (res ? std::to_string(*res).c_str() : "null"));
```

Swift

```swift
func fun(_ arg: Int) -> Int?{
    return arg > 0 ? arg : nil
}

let res = fun(0) // returned type is Optional<Int>
print("res=\(String(describing:res))")
```

Kotlin

```kotlin
fun funABC(arg: Int) : Int?{
    return if (arg > 0)  arg else null
}

val res = funABC(0) 
print("res=$res")
```

# Default argument value

C++

```c++
void fun(int arg1 = 1, int arg2 = 2){
  print("arg1=%d arg2=%d", arg1, arg2);
}

fun(); // arg1=1 arg2=2
fun(10); // arg1=10 arg2=2
fun(10, 20); // arg1=10 arg2=20
```

Default valus can be only in last arguments, so following code wouldn't compile:

```c++
void fun(int arg1 = 1, int arg2) {} // compile error
```

Swift

```swift
func fun(arg1: Int = 1, arg2: Int = 2){
    print("arg1=\(arg1) arg2=\(arg2)")
}

fun() // arg1=1 arg2=2
fun(arg1: 10) // arg1=10 arg2=2
fun(arg2: 20) // arg1=1 arg2=10 - this type of call is not possible in C++
fun(arg1: 10, arg2: 20) // arg1=10 arg2=20
```

In Swift, a default value can be set for any argument, so following code is OK:

```swift
func fun(arg1: Int = 1, arg2: Int){
    print("arg1=\(arg1) arg2=\(arg2)")
}
```

Kotlin

```kotlin
fun funABC(arg1: Int = 1, arg2: Int = 2) {
    println("arg1=$arg1 arg2=$arg2")
}

funABC() // arg1 = 1, arg2 = 2
funABC(3) // arg1 = 3, arg2 = 2
funABC(arg1 = 3, arg2 = 4) // arg1=3 arg2=4
funABC(arg2 = 5) //arg1=1 arg2=5 , so similar to Swift
```

# Return via argument

C++

```c++
void fun(int& arg1, int* arg2) {
    arg1 = 10;
    *arg2 = 20;
}

int res1 = 0;
int res2 = 0;
fun(res1, &res2);
printf("res1=%d res2=%d\n", res1, res2);

```

C++ allows return value using reference or pointer. 

Swift

```swift
func fun(_ arg1 : inout Int, arg2 : inout Int) {
    arg1 = 10
    arg2 = 20
}

var res1 = 0
var res2 = 0
fun(&res1, arg2: &res2)
print("res1=\(res1) res2=\(res2)")
```

It is required to put keyword *inout* return value by argument also & shall be put before variable while calling function. It works with label and nonlabel arguments.

# Variadic number of arguments



1) C-style 

```c++
double arithmeticMean(int count, ...) {
    double result = 0.0;
    std::va_list args;
    va_start(args, count);
    for (int i = 0; i < count; ++i) {
        result += va_arg(args, double); // no type validation, developer specifies type
    }
    va_end(args);
    return result / count;
}
double res = arithmeticMean(5, 1.0, 2.0, 3.0, 4.0, 5.0); // 5 - defines number of inputs
printf("res=%f\n", res);
```

2) C++ style

```c++
template<typename T1, typename... T>
T1 arithmeticMeanGeneric(T1 arg1, T... args)
{
    T1 sumRes = arg1 + (args + ...); // C++17 feature; sum of all arguments
    return sumRes / (sizeof...(args) + 1); // +1 because the 1st argument is arg1
}
double res=arithmeticMeanGeneric(1.0, 2.0, 3.0, 4.0, 5.0); // it will not work if 0 arguments provided
printf("res=%f\n", res);
```

Swift

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}

var res = arithmeticMean(1, 2, 3, 4, 5.0)
print("res=\(res)")
```

Kotlin

```kotlin
fun arithmeticMean(vararg numbers: Double) : Double {
    var total: Double = 0.0
    for (number in numbers) {
        total += number
    }
    return total / numbers.size
}

var res = arithmeticMean(1.0, 2.0, 3.0, 4.0, 5.0) // only double type
print("res=$res")
```

# Pointer to function & Function types 

C++

```c++
double fun(int arg1, double& arg2){
    arg2 = 0.5 * arg1;
    return arg2;
}

double (*f1)(int, double&) = fun; // it could be also used std::function; auto f1 = fun is OK

double res1 = 0;
double res2 = 0;
res1 = f1(10, res2);
printf("res1=%f res2=%f\n", res1, res2);
```

Swift

```swift
func fun(arg1 : Int, arg2 : inout Double) -> Double {
    arg2 =  0.5 * Double(arg1)
    return arg2
}

var f1: (Int, inout Double) -> Double = fun // inout - must appear if fun has it; var f1 = fun is OK

var res1 = 0.0
var res2 = 0.0
res1 = f1(10, &res2)
print("res1=\(res1) res2=\(res2)")
```

Kotlin

```kotlin
fun funABC(arg1 : Int) : Double {
    val tmp =  0.5 * arg1
    return tmp
}

var fp:(Int)->Double = ::funABC
var res = fp(1)
```

