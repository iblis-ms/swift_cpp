# Array

C++

C-style array

```c++
int array1[5]; // 5 items array without initialization

const int length = 5; // length of array needs to be compile time constant
int array2[length];

int array3[] = {1, 2, 3}; // 3 items array; elements: 1, 2, 3

int array4[3] = {1, 2}; // 3 items array: 1, 2, 0

array1[0] = 2;

for(const auto& item : array1){
  //...
}
```

C++11 array

```c++
std::array<int, 5> array1;

const int lenght = 5;
std::array<int, lenght> array2;

std::array<int, 3> array3{1, 2, 3};

std::array<int, 3> array4{1, 2};

std::array array5{1, 2}; // C++17 tempate type deduction; std::array<int, 2>

array1[0] = 2;

for(const auto& item : array1){
  //...
}
```

C++ - dynamic size

```c++
#include <vector>

std::vector<int> v1; // v1 = {} - empty array
v1.push_back(1); // v1 = {1} 
v1.push_back(2); // v1 = {1, 2}
int item = v1[1]; // item = 2
v1[0] = 3; // v1 = {3, 2}

int firstItem = v1.front(); // firstItem = 3
int lastItem = v1.back(); // lastItem = 2

std::vector<int> v2{1, 2, 3};

printf("length=%d", v1.size());
for(const auto& item : v2){
  //...
}
```

Swift

```swift
var array1:[Int] = [] // empty
var array2:Array<Int> = Array() // empty
var array3:[Int] = [1, 2, 3]
var array4 = [1, 2, 3]
array4.append(4) //array4: 1, 2, 3, 4

let item = array4[0] // item = 1
array4[0] = 30 // array4: 30, 2, 3, 4
array4[1...2] = [11, 12] // array4: 30, 11, 12, 4
array4 += [20, 21] // array4: 30, 11, 12, 4, 20, 21

var firstItem = array4.first
var lastItem = array4.last

print("length=\(array4.count)")
for item in array4{
    print("\(item)")
}

```

Swift array are like std::vector.



For performance reason in C++ and Swift it is highly recommended to set capacity of C++ std::vector and Swift Array before adding item.

C++

```c++
#include <vector>

std::vector<float> v;
v.reserve(1000); // now, the buffer is for 1000 object, so there is not required any reallocation for adding first 1000 object.
```

Swift

```swift
var v = Array<Int>()
v.reserveCapacity(1000) // now, the buffer is for 1000 object, so there is not required any reallocation for adding first 1000 object.
```

# Set

C++

```c++
std::set<int> set1{1, 2, 3, 4, 5, 6};

std::set<int>::iterator contains2_it = set1.find(2); //iterator is returned
if (contains2_it != set1.end()){ // returned iterator is 'end' operator means that 2 wasn't found.
    printf("contains 2");
}

bool contains3 = set1.contains(3); // C++20 added 'contains' 
if (contains3) {
  	printf("contains 3");
}

const std::pair<std::set<int>::iterator, bool> result1 = set1.insert(1);
const auto result2 = set1.insert(1); // better option
if (result2.second == true){
		printf("1 is already in the set");
}

for (const auto& item : set1){
		printf("%d\n", item);
}
```

Swift

```swift
var s1 = Set<Int>()
var s2: Set = [1, 2, 3]

var contains2 = s2.contains(2)
if contains2 {
    print("contains 2")
}

var result = s2.insert(1) // result has 'inserted' and 'memberAfterInsert' fields
if result.inserted == false { 
    print("1 is already in the set");
}

for item in s2 {
    print("\(item)")
}
```

# Map

C++

```c++
std::map<int, double> map1;
std::map<int, double> map2{{1, 1.1}, {2, 2.2}, {3, 3.3}};

double valueFor2 = map2[2];
map1[1] = 1.2;

std::map<int, double>::iterator it = map2.find(2);
if (it != map2.end()){
		printf("2 was find. Value: %f\n", it->second);
}

bool contains2 = map2.contains(2); // C++20
if (contains2){
		printf("2 was find. Value: %f\n", it->second);
}

for (const auto[key, value] : map2){
  	printf("%d = > %f\n", key, value);
}
```

Swift

```swift
var map1 = Dictionary<Int, Double>()
var map2:[Int:Double] = [:]
var map3: Dictionary<Int, Double> = [1: 1.1, 2 : 2.2, 3:3.3]
var map4 = [1: 1.1, 2 : 2.2, 3:3.3]

var contains2:Double? = map3[2]
if contains2 != nil {
    print("contains 2. Value: \(contains2)")
}

map3[2] = 2.3

for (key, value) in map3 {
    print("\(key) => \(value)")
}

```

# Tuple

C++

```c++
std::tuple<int, double, std::string> tuple1;

using namespace std::string_literals;
std::tuple tuple2 = std::make_tuple(1, 2.2, "abc"s);

int item1 = std::get<0>(tuple2);
double item2 = std::get<1>(tuple2);
std::string item3 = std::get<2>(tuple2);

// std::string item4 = std::get<0>(tuple2); // ERROR: on 0 position type is int.

std::get<0>(tuple2) = 3;
```

Swift

```swift
var tuple1 = (item1: 1, item2: 2.2, item3: "abc")

print("tuple1.item1=\(tuple1.item1)")
print("tuple1.item2=\(tuple1.item2)")
print("tuple1.item3=\(tuple1.item3)")

// tuple1.item1 = "ads" // ERROR: String to Int

tuple1.item1 = 11
```

