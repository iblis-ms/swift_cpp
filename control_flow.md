# If

C++ and Swift have similar structure of if command. I wrote several cases of ifs. C++ allows also short version of ifs (without brackets), but it decreases readability.

```c++
int variable1 = 0;
int variable2 = 1;
if (variable1 > 0) { // also: >, <, >=, <=, ==, !=
	// ...
}

if (variable2 < 5) {
	// ...
}
else if (variable1 == 8) {
	// ...
}
else {
	// ...
}

if (variable1 == 4 || variable1 == 5) {// || is logical or, && is logical and
	// ...
}

if (auto abc = variable1; variable1 >= 0) { // C++20 feature
	// ...
}
```

Swift

```swift
let variable1 = 0
let variable2 = 1
if variable1 > 0 { // brackets are optional: if (variable1 > 0) is also OK
	// ...
}

if variable2 < 5 {
	// ...
}
else if variable1 == 8 {
	// ...
}
else {
	// ...
}

if variable1 == 4 || variable1 == 5 {// || is logical or, && is logical and
    // ...
    print("111")
}

```

C++ & Swift have ternary operator: 

```
condition ? value_if_true : value_if_false
```

C++

```c++
int input = 0;
int res = input > 0 ? 1 : 0;
```

Swift

```swift
var input = 0
var res = input > 0 ? 1 : 0
```

 

# For - loop

Range based for is very similar in C++ and Swift except that the difference in end condition.

C++:

```c++
for (int index = 0; index < 10; ++index) {
  // ... index is not constant
}

const int beg = 0;
const int end = 10;
for (int index = beg; index < end; ++index){ 
  // ..
}
```

Swift:

```swift
for index in 0...10 {
    // ... index is constant,m like: let index
}

let beg = 0
let end = 10
for index in beg..<end { // ..< means lower than end; beg...end means lower or equal
    // ...
}
```

Iteration through array is also similar to C++11 foreach.

C++:

```c++
int array[] = {0, 1, 2, 3};
for (const auto& var : array) {
  // ...
}
```

Swift:

```swift
let array = [0, 1, 2, 3]
for item in array{
    //...
}
```

Loops for maps/dics

C++17

```c++
#include <map>

std::map<std::string, int> map{
	{"a", 1}, {"b", 2}, {"c", 3}
};

for (const auto& [key, value] : map)
{
	// ...
}
```

Swift

```swift
let map = ["a": 1, "b": 2, "c": 3]
for (key, value) in map {
    // ...
}
```

# While - loop

C++

```c++
int variable = 0;
while (variable < 10) {
	// ...
}
```

Swift:

```swift
var variable = 0
while variable < 10 {
	// ...
}
```

# Do/while & repeat/while - loop

C++

```c++
int variable = 0;
do {
	// ...
} while (variable < 5);
```

Swift

```swift
var variable = 0
repeat {
	// ...
} variable < 5
```

# Switch

The switch command has the biggest differences between C++ and Swift. C++ version requires break keyword to executing a case. If there isn't break, the next case will be processed. In C++ 17, there was added attribute *[[falltrough]]* to informat a compiler that missing *break* is not an issue. Swift version works in opposite way. By default there isn't fall through, but to fall through *fallthrough* is required.

C++

```c++
int variable = 0;
switch(variable){
  case 0:
    // ...
    break;
  case 1:
    [[fallthrough]]; // C++17 attribute. 
  case 2:
    // ... - code here will be executed by case 1 & 2
    break;
  case 3:
    // ... // fall through
  case 4:
    // ...
    break;
  default:
    // ...
    break;

}
```

Swift

```swift
var variable = 0
switch variable {
  case 0:
    // ...
  case 1, 2: // case for 1 & 2
    // ...
  case 3:
    // ...
    fallthrough
  case 4:
    // ...
  default:
    // ...
}
```

Moreover, Swift also alows range for cases.

```swift
var variable = 5
switch variable {
  case 0:
    // ...
  case 1...5: // case for 1, 2, 3, 4, 5
    // ...
  default:
    // ...
}
```

In C++ switch can work only with integer types. However, Swift allows String, Double, etc.

```swift
var variable = "a"
switch variable {
  case "a":
    // ...
  case "b"..."d", "f":
    // ...
  default:
    // ...
}
```

```swift
var variable = 1.2
switch variable {
  case 1.2:
    // ...
  case 2.3...3.5, 4.6: 
    // ...
  default:
    // ...
}
```

