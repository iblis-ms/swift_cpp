# Numeric types

There is a different approach for a variable declaration/definition:

C++:

```c++
type name = initial_value
```

Swift:

```
let name:type = initial_value
```

Both languages have architecture depended types (i.e. Swift's Int can be 32bit or 64bit).

C++ has more built-in types. Swift has built-in optional, tuple types, but it will descirbed later. Let's focus on numeric data types.

C++

```c++
float singlePrecisonVariable = 1.1F;
double doulbePrecisonVariable = 2.3;

short signedShortVariable = -1;
unsigned short unsignedShortVariable = 1U;
int signedIntegerVariable = -2;
unsigned int unsignedIntegerVariable = 2U;
long signedLongVariable = 3L;
unsigned long unsignedLongVariable = 4UL;
long long signedLongLongVariable = 5LL;
unsigned long long unsignedLongLongVariable = 6ULL;

char probablySignedChar = 'a';
signed char signedChar = 'b';
unsigned char Char = 'c';

```

Swift

```swift
var intVariable:Int = -10
var uintVariable:UInt = 20
var floatVariable:Float = 2.3
var doubleVariable:Double = 3.3
var strVariable:String = "abc"
var boolVariable:Bool = true
var charVariable:Character = "C" // "" instead of C style 'C'
```

# Architecture independent integers

C++ provides aliases to easily work with architecture independent integer. Therefore, it is required to include <cstdint> header file, where architecture independent types are defined.

```c++
#include <cstding>

int8_t signedInteger8Bit;
uint32_t unsignedInteger32Bit;
```

In Swift, there a language support for architecture independent types:

```swift
var int8Variable:Int8
var uint32Variable:UInt32
```

There are also float types with defined number of bits, which are not present in C++ standard library

```swift
var float32Variable:Float32
var float32Variable:Float32
```

## Type information

### C based style

There are defines in <climits> header for integer types:

```c
#include <climits>

uint8_t max8bitUnsignedInteger = UINT8_MAX;
int32_t min32bitSignedInteger = UINT32_MIN;
```

for float & double you need to include <cfloat>

```c
#include <cfloat>

float floatMax = FLT_MAX;
float floatMinPositiveValue = FLT_MIN; // this is the lowest positive value that can be stored in float
float floatMin = -FLT_MAX; // nagative of FLT_MAX is the lowest value of float
```

### C++ style

C++11 introduced <limits> header file with template based approach for limits, etc. There is no distinguishing between float and integer types .

```c++
#include <limits>

float floatMax = std::numeric_limits<float>::max();
float floatMinPositiveValue = std::numeric_limits<float>::min();
float floatMin = -std::numeric_limits<float>::max();

uint8_t max8bitUnsignedInteger = std::numeric_limits<uint8_t>::max();
int32_t min32bitSignedInteger = std::numeric_limits<int32_t>::min();
```

### Swift style

```swift
var max8bitUnsignedInteger:UInt8 = UInt8.max
var floatMax:Float32  = Float32.greatestFiniteMagnitude
var floatMinPositiveValue:Float32  = Float32.leastNormalMagnitude
```

# String data type

C++ has C-style string and std::string (and std::string_view from C++17) for string operations. 

### C-style string

```
const char* cStyleString = "abc";
```

To do any action with C-style string you need to include header file with declaration of function:

```c++
#include <cstring>
const char* cStyleString = "abc";
size_t length = strlen(cStyleString);
```

The code above shows compile time constant string pointed by cStyleString. If a developer wants to any manipulation of string, their have to use dynamic memory allocation for most cases or have a buffer with required size. Unfortunately, C-style approach leaves all stuff required with  memory allocation and deallocation on developers, example:

```c++
char* cStyleString = new char[4]; // allocate memory for 4 chars
strcpy(cStyleString, "abc"); // copy "abc" to a dynamic allocated memory
size_t length = strlen(cStyleString); // get the length
cStyleString[1] = 'D'; // change "abc" to "aDc"
delete[] cStyleString; // deallocate memory
```

### C++ std::string

In string header, std::string class is defined. It use dynamic memory allocation to store characters. It exposes API to easily manipulate 

```c++
#include <string>

std::string str("abc"); // creates a string object, put "abc" to dynamically allocated memory inside.
size_t length = str.length(); // get the length
str[1] = "D"; // change "abc" to "aDc"
```

In this case, std::string allocates and deallocates memory if it required. Functions to manipulate on string are parts of std::string. It is easy to use.

### Swift string

Because String in Swift is done in completely different way. It has fully support for unicode, it covers NSString from Objective-C. Operating on String is quite painful for beginners. In Swift, characters are not located one after one in continous memory where each byte represents one character - it makes operation on String quite counterintuitive. 

```swift
var strTest = "abc"
var length = strTest.count
let start = strTest.index(strTest.startIndex, offsetBy: 1); // convert index to the real index in String implementation
let end = strTest.index(strTest.startIndex, offsetBy: 2); // as above
strTest.replaceSubrange(start..<end, with: "D") // now strTest is aDc

```

# Constants

C++ is has const and constexp keywords. There are 2 options of costantness: compile time constant and non-compile time constant.

Compile time constant - It means that a compile can calculate the value while compilation. Therefore, it can put the calculated value directly in object code, so there is completely zero overhead in runtime. 

Non-compile time constant - The 'const' keyword in this case means that variable cannot be changed after initialization. 

```c++
const int a = 2; // compile time constant
constexp int b = 3; // as above

const std::string str("abc"); // constant str variable, but not compile time.
str = "def"; /// ERROR: cannot change 'str'
```

In Swift, there is a special keyword 'let'.

```swift
let constVar:Int = 2
constVar = 3 // ERROR
```

# Automatic type deduction

C++ from C++11 supports automatic type deduction. Swift has also this functionality.

```c++
auto thisIsInt = 2;
auto thisIsFloat = 2.3f;
auto thisIsDouble = 2.3;
```

Swift version:

```swift
var thisIsInt = 2
var thisIsDouble = 3.0 // however.. "Swift always chooses Double (rather than Float)" - rather?
```

