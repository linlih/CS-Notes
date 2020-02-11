# STL 常用算法

## 函数对象

重载函数调用操作符的类，其对象常称为函数对象（function object），既他们是行为类似函数的对象，也叫仿函数（functor），其实就是重载“（）”操作符，使得类对象可以像函数那样调用。

函数对象还可以作为参数传递。

1. 函数对象通常不定义构造函数和析构函数，避免了函数调用的运行时问题
2. 函数对象超出了普通函数的概念，内部可以保存状态（定义变量等）。
3. 函数对象可内联编译，性能好。用函数指针几乎不可能
4. 模板函数对象使函数对象具有通用性，这也是他的优势之一

## 谓词

谓词是指普通函数或者重载operator\(\)返回值是bool的函数对象\(仿函数\)。如果接受一个参数就称之为一元谓词，两个参数就是二元谓词。

```cpp
find_if算法
vector<int> v;
...// 插入数据

class GreaterThan20 {
public:
  bool operator(int val) {
        return val > 20;
    }
};
// 第三个参数是函数对象
vector<int>::iterator pos = find_if(v.begin(), v.end(), GreaterThan20());
if (pos != v.end()) {
    cout << "find val bigger than 20:" << *pos << endl;
}

// 匿名函数 lambda表达式 [标识符](参数){实现};
```

## 内建函数对象

## 适配器

```cpp
// 这些都是仿函数，已经实现了重载小括号的功能
//头文件：#include <functional>
template<class T> T plus<T>
template<class T> T minus<T>
template<class T> T multiplies<T>
template<class T> T divides<T>
template<class T> T modulus<T>
template<class T> T negate<T> // 取反

template<class T> bool equal_to<T>
template<class T> bool not_equal_to<T>
template<class T> bool greater<T>
template<class T> bool greater_equal<T>
template<class T> bool less<T>
template<class T> bool less_equal<T>

template<class T> bool logical_and<T>
template<class T> bool logical_or<T>
template<class T> bool logical_not<T>


negate<int> n;
cout << n(10); << endl; // 输出：-10

plus<int> p;
cout << p(1, 1) << endl;

vector<int> v;
sort(v.begin(), v.end(), greater<int>());
for_each(v.begin(), v.end(), [](int val) {cout << val << " ";});

#include <functional>

vector<int> v;
for (int i = 0; i < 10; ++i) {
    v.push_back(i);
}

int num;
cin >> num;

for_each(v.begin(), v.end(), bind2nd(MyPrint(), num));

class MyPrint::public binary_function<int, int, void> {
public:
    void operator() (int val, int start) {
        cout << val + start << endl;
    }
};

// 函数适配器
// 第一步：绑定数据 bind2nd
// 第二步：继承类 binary_function<参数类型1，参数类型2，返回值类型>
// 第三步：加const修饰 operator()

// 适配器，适配原有只有一参数的函数扩展为两个参数


// 取反适配器
// 一元取反, 使用not1
// 要继承 unary_function<参数类型，返回值类型>
// 加上const修饰
vector<int> v;
for (int i = 0; i < 10; i++) {
    v.push_back(i);
}

class GreaterThan5: public unary_function<int, bool> {
public:
    bool operator()(int v) const {
        return v > 5;
    }
};
// 查找大于5的数字，需求改为找小于5的数字
vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(GreaterThan5())); // not1表示一元取反，not2表示二元取反
vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(bind2nd(greater<int>(), 5))); // not1表示一元取反，not2表示二元取反
if (pos != v.end()) {
    cout << *pos << endl;
}
else {
    cout << "not found" << endl;
}

// 函数指针适配器
void MyPrint(int v, int start) {
    cout << v + start << endl;
}

for_each(v.begin(), v.end(), bind2nd(ptr_fun(MyPrint), 100));

// 成员函数适配器
class Person {
public:
    Person(string name, int age) {
        this->m_Name = name;
        this->m_Age = age;
    }
    void showPerson() {
        cout << m_Name << m_Age << endl;
    }
    void plusAge() {
        m_Age+=100;
    }

    string m_Name;
    int m_Age;
};

void MyPrintPerson(Person& p) {
    cout << p.m_Name << p.m_Age << endl;
}

vector<Person> v;

Person p1("aaa", 10);
Person p2("bbb", 15);
Person p3("ccc", 18);
Person p4("ddd", 40);

v.push_back(p1);
v.push_back(p2);
v.push_back(p3);
v.push_back(p4);

for_each(v.begin(), v.end(), MyPrintPerson);
for_each(v.begin(), v.end(), mem_fun_ref(&Persion::showPerson));
for_each(v.begin(), v.end(), mem_fun_ref(&Persion::plusAge));// 这个可以实现所有成员年龄增加100岁
```

## 遍历算法

算法主要由头文件  组成

是所有的STL头文件中最大的一个，常用功能包含：比较，交换，查找，遍历，复制，修改，反转，排序，合并等

体积很小，只包含了几个序列容器上进行简单运算的模板函数

定义了一些模板类，用于声明函数对象

```cpp
/*
遍历算法，遍历容器元素
@param beg 开始迭代器
@param end 结束迭代器
@param _callback 函数回调或者函数对象
@return 函数对象
*/
for_each(iterator beg, iterator end, _callback);

/*
transform算法，将指定容器区间元素搬运到另外一个容器中
注意！transform不会给目标容器分配内存，需要提前分配好内存
@param beg1 源容器开始迭代器
@param end1 源容器结束迭代器
@param beg2 目标容器开始迭代器
@param _callback 回调函数或者函数对象
@return 返回目标容器迭代器
*/
transform(iterator beg1, iterator end1, iterator beg2, _callback);
```

## 查找算法

```cpp
/*
find算法
@param beg 开始迭代器
@param end 结束迭代器
@param value 查找的元素
@return 返回查找元素的位置
*/
vector<int> v;
vector<int>::iterator pos = find(v.begin(), v.end(), 5);
if (pos != v.end())
    cout << "find:" << *pos << endl;

// 利用find查找自定义数据类型
class Person {
public:
    Person(string name, int age) {
        this->m_Name = name;
        this->m_Age = age;
    }
    bool operator==(const Person& p){
        return this->m_Name = p.m_Name && this->age == p.age;
    }
    string m_Name;
    int m_Age;
};

// 创建Person数据，然后压入v, 这个时候我们需要重载==
vector<Person>::iterator pos = find(v.begin(), v.end(), p2);

// find_if使用的而是仿函数
class myCompare : public binary_function<Person*, Person*, bool>{
public:
    bool operator(Person* p1, Person* p2) const {
        return p1->m_Name == p2->m_Name && p1->m_age == p2->m_age;
    }
};

// 使用bind2nd要引入<functional>
vector<Person *>::iterator pos = find_if(v.begin(), v.end(), bind2nd(myCompare(), p));

/*
adjacent find算法，查找相邻的重复的元素
*/
vector<int>::iterator pos = adjacent_find(v.begin(), v.end()); // 返回重复并相邻的第一个位置

/*
binary_search算法，二分查找，需要在有序序列中使用
*/

/*
count、count_if算法，统计元素出现次数
*/
```

## 常用排序算法

```cpp
/*
merge算法，容器元素合并，这两个容器也必须是有序的
@param beg1
@param end1
@param beg2
@param end2
@return dest
*/

vector<int> v1; // 放入1 2 3
vector<int> v2; // 放入4 5 6
vector<int> vTarget;
vTarget.resize(v1.size() +v2.size());
merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());

/*
sort算法
@param beg
@param end
@param _callback
*/ 
sort(v1.begin(), v1.end());// 默认从小到大

// 使用内建算法，需要引入<functional>
sort(v1.begin(), v1.end(), greator<int>()); // 从大到小

/*
srand((unsigned int)time(NULL)); 因为是随机打乱，所以要给种子保证每次都不同
random_shuffle(iterator beg, iterator end) 洗牌算法
*/

/*
reverse(v.begin(), v.end()) 翻转排序
*/
```

## 常用集合算法

```cpp
/*
set_intersection，求两个set的交集
*/
vTarget.resize(min(v1.size(), v2.size());
vector<int>::iterator itEnd = set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
copy(vTarget.begin(), itEnd, ostream_iterator<int>(cout ," ")); // itEnd如果使用vTarget.end(),可能会多输出零，因为vTarget.end()可能不代表元素的结尾位置

/*
set_union，求两个set的并集
*/

/*
set_difference，求两个set的差集
*/
```

## 常用的拷贝和替换算法

```cpp
/*
copy算法，将容器内指定范围的元素拷贝到另一容器中
beg, end, dest参数
*/
// 需要包含<iterator>
copy(v.begin(), v.end(), ostream_iterator<int>(cout, " ");

/*
replace算法
beg, end, oldvalue, newvalue
replace if 算法
beg, end, _callback, newvalue
*/

/*
swap算法
c1, c2 参数
*/
```

## 常用算术生成算法

```cpp
/*
accumulate算法，计算容器元素累计总和
beg, end, value
value指的是起始的累加值
要包含：<numeric>
*/

/*
fill算法
beg, end, value
value表示填充元素
*/
```

