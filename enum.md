# Enum

#### C++

```c++
enum EnumType{
    VALUE_1 = 10,
    VALUE_2 = 15,
    VALUE_3,
    VALUE_4
};
EnumType var1 = VALUE_1;
```

C++11

```c++
enum class EnumClassType : unsigned char {
    CL_VALUE_1 = 10,
    CL_VALUE_2 = 15,
    CL_VALUE_3,
    CL_VALUE_4
};
EnumClassType var2 = EnumClassType::CL_VALUE_2;
```

#### Swift

```swift
enum EnumType{
    case VALUE_1
    case VALUE_2
    case VALUE_3, VALUE_4
};

var value = EnumType.VALUE_1
```

Enum with values

```swift
enum EnumType{
    case VALUE_1(Int, Double)
    case VALUE_2(Int)
    case VALUE_3(String), VALUE_4
};

var value = EnumType.VALUE_1(1, 2.0)
print(value) // VALUE_1(1, 2.0)

switch value {
case .VALUE_1(let firstItem, let secondItem):
    print("firstItem=\(firstItem), secondItem=\(secondItem).")
case .VALUE_2(let item):
    print("item=\(item).")
case .VALUE_3(let item):
    print("item=\(item).")
case .VALUE_4:
    print("without items")
}
```

#### Kotlin

```kotlin
enum class EnumType {
    VALUE_1, VALUE_2, VALUE_3, VALUE_4
}

val obj = EnumType.VALUE_1
print(obj) // VALUE_1
```

Enum with values

```kotlin
enum class EnumType(val value:Int) {
    VALUE_1(10),
    VALUE_2(15),
    VALUE_3(16),
    VALUE_4(17)
}

val obj = EnumType.VALUE_1
println(obj) // VALUE_1
println(obj.value) // 10
```

