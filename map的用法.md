`std::map` 是 C++ 标准模板库（STL）中的一个关联容器，用于存储键值对（key-value pairs），其中每个key都是唯一的，并且按照特定的顺序（通常是升序）自动排序。`std::map` 通常基于红黑树实现，提供对元素的高效查找、插入和删除操作。

## 1. 基本特性

- **有序性**：`std::map` 中的元素按照key的顺序自动排序，默认使用 `<` 运算符进行比较。
- **唯一键**：每个key在 `std::map` 中必须是唯一的，如果尝试插入重复的key，则插入操作会失败。
- **关联容器**：通过key快速访问对应的value，通常具有对数时间复杂度O(log n)。
- **可变性**：可以动态地插入和删除元素。

要使用 `std::map`，需要包含头文件 `<map>` 并使用 `std` 命名空间：

```cpp
#include<map>
#include<iostream>
#include<string>
int main(){
// 键为 int，值为 std::string 的 map
std::map<int, std::string> myMap;

// 键为 std::string，值为 double 的 map
std::map<std::string, double> priceMap;

 myMap = {
   {1, "Apple"},
   {2, "Banana"},    
   {3, "Cherry"}
};
   return 0;
}
```
## 2. 主要操作

### 2.1 插入元素

有几种方法可以向 `std::map` 中插入元素：

####  使用 `insert` 函数

```cpp
myMap.insert(pair<int, string>(4, "Date"));
// 或者使用 `make_pair`
myMap.insert(std::make_pair(5, "Elderberry"));
// 或者使用初始化列表
myMap.insert({6, "Fig"});
```

####  使用下标运算符 `[]`

```cpp
myMap[7] = "Grape";
// 如果key 8 不存在，则会插入key 8 并赋值
myMap[8] = "Honeydew";
```

**注意**：使用 `[]` 运算符时，如果key不存在，会自动插入该key，并将对应的value初始化为类型的默认值。

### 2.2 访问元素

#### 使用下标运算符 `[]`

```cpp
std::string fruit = myMap[1]; // 获取key为 1 的value "Apple"
```

**注意**：如果key不存在，`[]` 会插入该key并返回默认值。

#### 使用 `at` 成员函数

```cpp
try {
    fruit = myMap.at(2); // 获取key为 2 的value "Banana"
} catch (const out_of_range& e) {
    cout << "Key not found." << endl;
}
```

`at` 函数在key不存在时会抛出 `std::out_of_range` 异常，适合需要异常处理的场景。

#### 使用 `find` 成员函数

```cpp
auto it = myMap.find(3);
if (it != myMap.end()) {
    cout << "Key 3: " << it->second << endl; // 输出 "Cherry"
} else {
    cout << "Key 3 not found." << endl;
}
```

`find` 返回一个迭代器，指向找到的元素，若未找到则返回 `map::end()`。

### 2.3 删除元素

#### 使用 `erase` 函数

```cpp
// 按key删除
myMap.erase(2);

// 按迭代器删除
auto it = myMap.find(3);
if (it != myMap.end()) {
    myMap.erase(it);
}

// 删除区间 [first, last)
myMap.erase(myMap.begin(), myMap.find(5));
```

#### 使用 `clear` 函数

```cpp
myMap.clear(); // 删除所有元素
```

### 2.4 遍历 `std::map`

#### 使用迭代器

```cpp
for (auto it = myMap.begin(); it != myMap.end(); ++it) {
    cout << "Key: " << it->first << ", Value: " << it->second << endl;
}
```

#### 使用基于范围的 `for` 循环（C++11 及以上）

```cpp
for (const auto& pair : myMap) {
    cout << "Key: " << pair.first << ", Value: " << pair.second << endl;
}
```

### 2.5 常用成员函数

- **`size()`**：返回容器中元素的数量。
- **`empty()`**：判断容器是否为空。
- **`count(key)`**：返回具有指定key的元素数量（对于 `map`，返回 0 或 1）。
- **`lower_bound(key)`** 和 **`upper_bound(key)`**：返回迭代器，分别指向第一个不小于和第一个大于指定key的元素。
- **`equal_range(key)`**：返回一个范围，包含所有等于指定key的元素。

## 3. 自定义key的排序

默认情况下，`std::map` 使用 `<` 运算符对key进行排序。 ^5003eb

如果需要自定义排序方式，可以提供一个自定义的比较函数或函数对象。
### 在类中实现
Person.h
```cpp
#pragma once
#include <iostream>
#include <string>
class Person {
public:
	Person(int age, std::string name);
	int GetAge()const;//必须用const修饰函数
	bool operator < (const Person& other)const;//必须用const修饰函数,只能重载<
	//改为bool operator > (const Person& other)const;报错
private:
	int _age;
	std::string _name;
};
```
Person.cpp
```cpp
#include "Person.h"
bool Person::operator<(const Person& other) const{

	return _age < other._age;
	//可以改为return _age > other._age;实现降序
}

Person::Person(int age, std::string name) :_age(age), _name(name) {

}
int Person::GetAge() const {
	return _age;
}
```
也就是说要重载的是`<`,想实现降序可以在具体实现中控制,但是不可直接重载`>`
与[[map的用法#^5003eb|前面]]呼应
在已重载`<`时,重载`>`不会报错

---
main.cpp
```cpp
#include"Class.h"
#include <iostream>
#include <map>
#include <string>

int main() {
    std::map <Person, int>mymap;
    Person a(10, "a");
    Person b(13, "b");
    Person c(18, "c");
    Person d(50, "d");
    mymap.insert({ a,88 });
    mymap.insert({ b,80 });
    mymap.insert({ c,68 });
    mymap.insert({ d,100 });
    for (auto& tem : mymap)
    {
        std::cout << tem.first.GetAge() << " " << tem.second << std::endl;
    }
    return 0;
}
```
输出:
```cpp
10 88
13 80
18 68
50 100
```
### 仿函数实现
Person.h
```cpp
#pragma once
#include <iostream>
#include <string>
class Person {
public:
	Person(int age, std::string name);
	int GetAge()const;//必须用const修饰函数
private:
	int _age;
	std::string _name;
};
class ComparePerson {
public:
	bool operator()(const Person& first, const Person& second)const;//必须用const修饰函数
};
```
Person.cpp
```cpp
#include "Person.h"
Person::Person(int age, std::string name) :_age(age), _name(name) {
}

int Person::GetAge() const {
	return _age;
}

bool ComparePerson::operator()(const Person& first, const Person& second) const{
	return first.GetAge() < second.GetAge();
}
```
main.cpp
```cpp
#include"Person.h"
#include <iostream>
#include <map>
#include <string>

int main() {
    std::map <Person, int,ComparePerson>mymap;//传入比较规则
    Person a(10, "a");
    Person b(13, "b");
    Person c(18, "c");
    Person d(50, "d");
    mymap.insert({ a,88 });
    mymap.insert({ b,80 });
    mymap.insert({ c,68 });
    mymap.insert({ d,100 });
    for (auto& tem : mymap)
    {
        std::cout << tem.first.GetAge() << " " << tem.second << std::endl;
    }
    return 0;
}
```
###  std::function封装Lambda表达式   

Person.h
```cpp
#pragma once
#include <iostream>
#include <string>
class Person {
public:
	Person(int age, std::string name);
	int GetAge()const;//必须用const修饰函数
private:
	int _age;
	std::string _name;
};
```
Person.cpp
```cpp
#include "Person.h"
Person::Person(int age, std::string name) :_age(age), _name(name) {
}

int Person::GetAge() const {
	return _age;
}
```
main.cpp
```cpp
#include"Person.h"
#include <iostream>
#include <map>
#include <string>
#include <functional>//引入才能使用std::function

int main() {
    std::function<bool(const Person& , const Person& )> compare = [](const Person& first, const Person& second) {
        return first.GetAge() < second.GetAge();
        };
    //用std::function封装Lambda表达式   
       
    std::map <Person, int, std::function<bool(const Person&, const Person&)> >mymap(compare);
    Person a(10, "a");
    Person b(13, "b");
    Person c(18, "c");
    Person d(50, "d");
    mymap.insert({ a,88 });
    mymap.insert({ b,80 });
    mymap.insert({ c,68 });
    mymap.insert({ d,100 });
    for (auto& tem : mymap)
    {
        std::cout << tem.first.GetAge() << " " << tem.second << std::endl;
    }
    return 0;
}
```

## 4. `std::map` 与其他关联容器的比较

- **`std::unordered_map`**：基于哈希表实现，提供平均常数时间复杂度的查找、插入和删除操作，但不保证元素的顺序。适用于对顺序无要求且需要高效查找的场景。
- **`std::multimap`**：允许多个相同key的元素，其他特性与 `std::map` 类似。适用于需要存储重复键值对的场景。

## 5. 性能考虑

- 时间复杂度:
    - 查找、插入、删除：O(log n)
    - 遍历：O(n)
- **空间复杂度**：`std::map` 通常需要额外的空间来维护树结构，相比 `std::vector` 等序列容器，内存开销更大。

选择使用 `std::map` 还是其他容器，应根据具体需求和性能要求进行权衡。

## 6. 完整示例

以下是一个完整的示例，展示了 `std::map` 的基本用法：

```cpp
#include <iostream>
#include <map>
#include <string>

int main() {
    // 创建一个 map，键为 int，值为 string
    std::map<int, std::string> myMap;

    // 插入元素
    myMap[1] = "Apple";
    myMap[2] = "Banana";
    myMap.insert({3, "Cherry"});
    myMap.insert(std::make_pair(4, "Date"));

    // 访问元素
    std::cout << "Key 1: " << myMap[1] << std::endl;
    std::cout << "Key 2: " << myMap.at(2) << std::endl;

    // 查找元素
    int keyToFind = 3;
    auto it = myMap.find(keyToFind);
    if (it != myMap.end()) {
        std::cout << "Found key " << keyToFind << ": " << it->second << std::endl;
    } else {
       std::cout << "Key " << keyToFind << " not found." << std::endl;
    }

    // 遍历 map
    std::cout << "All elements:" << std::endl;
    for (const auto& pair : myMap) {
        std::cout << "Key: " << pair.first << ", Value: " << pair.second <<std:: endl;
    }

    // 删除元素
    myMap.erase(2);
    std::cout << "After deleting key 2:" << std::endl;
    for (const auto& pair : myMap) {
        std::cout << "Key: " << pair.first << ", Value: " << pair.second << std::endl;
    }

    // 检查是否为空
    if (!myMap.empty()) {
        std::cout << "Map is not empty. Size: " << myMap.size() << std::endl;
    }

    // 清空所有元素
    myMap.clear();
    std::cout << "After clearing, map is " << (myMap.empty() ? "empty." : "not empty.") << std::endl;

    return 0;
}
```

**输出**：

```cpp
Key 1: Apple
Key 2: Banana
Found key 3: Cherry
All elements:
Key: 1, Value: Apple
Key: 2, Value: Banana
Key: 3, Value: Cherry
Key: 4, Value: Date
After deleting key 2:
Key: 1, Value: Apple
Key: 3, Value: Cherry
Key: 4, Value: Date
Map is not empty. Size: 3
After clearing, map is empty.
```