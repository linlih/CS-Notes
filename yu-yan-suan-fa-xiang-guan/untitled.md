# STL容器

STL广义上分为：容器（container）、算法（algorithm）、迭代器（iterator），容器和算法之间通过迭代器进行无缝连接

STL 六大组件：容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器

容器：各种数据结构，如vector、list、deque、set、map等，从实现角度上看，STL容器是一种class template

算法：各种常用算法，如sort、find、copy、for\_each等。从实现角度上看，STL算法是一种function template

迭代器：扮演容器与算法之间的粘合剂，共有五种类型，从实现角度上看，迭代器是一种将operator\*, operator→, operator++, operator—等指针相关操作给予重载的class template。所有的STL容器都附带有自己专属的迭代器，只有容器的设计者才知道如何遍历自己的元素。原生指针\(native pointer\)也是一种迭代器。

仿函数：行为类似函数，可以作为算法的某种策略。从实现角度上看，仿函数是一种重载了operator\(\)的class或者class template

适配器：一种用来修饰容器或者仿函数或者迭代器接口的东西

空间配置器：负责空间的配置与管理，从实现角度看，配置器是一个实现了动态空间配置、空间管理、空间释放的class template

六大组件的交互关系：

容器通过空间配置器取的数据存储空间，算法通过迭代器存储容器中的内容，仿函数可以协助算法完成不同的策略的变化，适配器可以修饰仿函数

优点：

* STL是C++内在的一部分，不需要安装什么
* STL的一个重要特性是将数据和操作分离。数据由容器类别加以管理，操作则由可制定的算法定义。迭代器在两者之间充当粘合剂，使得算法和容器可以交互运作。
* 高可重用性，高性能，高移植性，跨平台

  高可重用性：STL几乎所有的代码都采用了模板类和模板函数进行实现的

  高性能：如map可以高效地在十万条记录中找出指定的记录，map采用的是红黑树的变体实现的

  高移植性：项目之间可以快速移植

  跨平台：

## STL 三大组件

### 容器

序列式容器

每个元素有固定的位置，除非用删除或者插入进行改变。比如vector, deque, list等

关联式容器

各元素没有严格的物理上的顺序关系，元素在容器中并没有保存元素插入容器时的逻辑顺序，比如set multiset map multimap等。另一个特定是使用关键字进行索引。

### 算法

质变算法

会改变区间元素中的内容，比如拷贝、替换、删除等

非质变算法

不会改变区间的元素内容，比如查找，计数，遍历等

### 迭代器

iterator定义如下：提供一种方法，使之能够依序寻访某个容器中所含的各个元素，而又无需暴露该容器的内部表示方式。

分为五类，输入、输出、前向、双向、随机访问迭代器。

```cpp
vector<int> v;
v.push_back(10); // 尾插法

vector<int>::iterator itBegin = v.begin(); // 声明一个迭代器, 指向容器的起始位置
vector<int>::iterator itEnd = v.end(); // 指向最后一个元素

*itBegin // 取元素
itBegin++ // 指向下一个元素

// 遍历
while(itBegin!=itEnd) {
    cout << *itBegin << endl;
    itBegin++;
}

// 遍历2
for (vector<int>iterator it=v.begin(); it != v.end(); it++) {
    cout << *it << endl;
    // 如果遍历的是对象的话，则需要使用(*it)指向类的成员变量 eg. (*it).m_Value, it->m_Value
}

// 遍历3, 包含在头文件：algorithm
void myPrint(int v) {
    cout << v << endl;
}
for_each(v.begin(), v.end(), myPrint);

// 容器嵌套
vector<vector<int> > v;
vector<int> v1;
vector<int> v2;
vector<int> v3;

// 遍历
for (vector<vector<int> >::iterator it=v.begin(); it != v.end(); it++){
    for (vector<int>::iterator vit=(*it).begin(); vit != (*it).end(); vit++) {
        cout << *vit << " ";
    }
    cout << endl;
}
```

## 常用容器

### string 容器

构造函数

```cpp
string(); // 创建一个空的字符串, eg: string str;
string(const string& str); // 拷贝构造, eg: string str1(str); string str1 = str;
string(const char* s); // 字符串s构造，eg: string str2("abc"); string str2="abc";
string(int n, char c); // 用n个字符串c来初始化, eg: string str3(10, 'a');
```

基本赋值操作

```cpp
string& operator=(const char* s); // str = "abc";
string& operator=(const string& str); // str = str1;
string& operator=(const char c);
string& assign(const char* s);
string& assign(const char* s, int n); // 将s字符串的前n个字符复制给调用对象
string& assign(const string& str);
string& assign(int n, char c);
string& assign(const string& s, int start, int n); // 从s对象中start开始，拷贝n个字符，从0开始索引
```

存取操作

```cpp
char& operator[](int n);
char& at(int n);
// [] 和 at的区别： [] 访问越界会直接挂掉，at访问越界会抛出异常
try {
}
catch(...) { // 这里的话捕捉所有的异常，用...来代替
}

// 越界异常的写法
try {
    cout << s.a[100] << endl; // 假定s只有10个元素
    // cout << s[100] << endl; // []的访问方式不会抛出异常
}
catch (out_of_range & e) {
    cout << e.what() << endl;
}
```

拼接操作

```cpp
string& operator+=(const string& str);
string& operator+=(const char *str);
string& operator+=(const char c);
string& append(const char* s); // 将字符串s连接到当前字符串结尾
string& append(const char* s, int n); // 将字符串s的前n个字符连接到当前字符串结尾
string& append(const string& s);
string& append(const string& s, int pos, int n); // 从pos开始的n个字符连接到结尾
string& append(int n, char c); // 末尾添加n个字符c
```

查找和替换

```cpp
// 查找失败的返回 -1
int find(const string& str, int pos = 0) const; // 从pos开始查找str第一次出现的位置
int find(const char* s, int pos = 0) const;
int find(const char* s, int pos, int n); // 查找s前n个字符
int find(const char c, int pos = 0);
// r stands for right, 从右往左找
int rfind(const string& str, int pos = npos) const;// 查找str最后一次出现的位置，从pos开始
int rfind(const char* s, int pos = npos) const;
int rfind(const char* s, int pos, int n) const;
int rfind(const char c, int pos = 0);
string& replace(int pos, int n, const string& str); // 替换从pos开始的n个字符串为str, 注意！n个字符是指源字符串的n个，全部替换为str，两者没有数量关系
string& replace(int pos, int n, const char* s);
```

比较

```cpp
// compare函数>时返回1，<时返回-1，等于是返回0
// 大小按照字母顺序，排前面的越小，大学的A比小写的a小
int compare(const string& s);
int compare(const char* s);
```

子串

```cpp
string substr(int pos = 0, int n = npos); // 返回由pos开始的n个字符组成的字符串

// 查找邮件的用户名
string email = "test@qq.com";
int pos = email.find("@");
string name = email.substr(0, pos);
```

插入和删除

```cpp
string& insert(int pos, const char* s);
string& insert(int pos, const string& str);
string& insert(int pos, int n, char c);
string& erase(int pos, int n = npos);
```

string 和 c-style字符串转换

```cpp
// string 转 char*
string str = "hello"
const char* cstr = str.c_str();
// char* 转 string, char*是可以进行隐式类型转换为string的
char* cstr = "hello"
string str(cstr);
```

补充

```cpp
toupper(char c);// 将字符c转换成大写
tolower(char c);// 将字符c转换成小写
```

### vector 容器

和数组array的唯一差别在于空间的灵活性

```cpp
vector<int> v; // 单端数组 动态数组
v.rbegin() // 最后一个元素
v.rend() // 第一个元素
v.begin()// 第一个元素
v.end() // 最后一个元素
v.push_back() // 尾插
v.pop_back() // 尾删
v.capacity() // v的容量，vector的动态扩展是动态的，不是固定扩展两倍
// vector的动态增长，不是在原有空间上增加，而是找一个更大的空间，然后将原有的数据进行拷贝到新的空间上去，然后释放原有的空间，所以要注意一定空间上发生了重新分配，原有的vector的迭代器就失效了，不能再使用，否则将发生错误。
```

vector 构造函数

```cpp
vector<T> v; 
vector<T> v1(v.begin(), v.end()); // 将v对象的内容拷贝过来进行构造
vector<T> v(n, elem); // 将n个elem拷贝给自身
vector<T> v(const vector& vec); // 拷贝构造函数

int arr[] = {1, 2, 3, 4}
vector<int> v1(arr, arr+sizeof(arr)/sizeof(int));
vector<int> v2(v1.begin(), v1.end();
```

赋值操作

```cpp
assign(beg, end); // 将[beg, end)区间的数据拷贝赋值给自身
assign(n, elem); // 将n个元素拷贝给自身
vector& operator=(const vector& vec);
swap(vec); // 将vec与自身的元素互换

// 巧用swap收缩空间
vector<int> v;
for (int i = 0; i < 100000; ++i) {
    v.push_back(i);
}

cout << "v's capacity: " << v.capacity() << endl; // 13万多
cout << "v's size: " << v.size() << endl; // 10万

v.resize(3);
cout << "v's capacity: " << v.capacity() << endl; // 13万多
cout << "v's size: " << v.size() << endl; // 3

vector<int>(v).swap(v);// 利用v初始匿名对象，按照v.size()大小开辟匿名对象空间，swap交换匿名对象和v对象的指针，运行结束后，匿名对象销毁，原来的匿名对象空间变成v对象
cout << "v's capacity: " << v.capacity() << endl; // 3
cout << "v's size: " << v.size() << endl; // 3


// 
vector<int> v;
int *p = NULL;
int num = 0;

// 这段代码可以计算在插入100000个数据时总共开辟了多少次空间
// 运行结果得到30次
for (int i = 0; i < 100000; ++i) {
    v.push_back(i);
    if (p != &v[0]) {
        p = &v[0];
        num++;
    }
}

v.reserve(100000); // 则上述代码执行的结果为1，只进行开辟1次空间
```

大小操作

```cpp
size(); 
empty(); // 返回true，表示为空，反之返回false
resize(int num); // 重新指定容器长度，容器变长填充默认值，变短超出元素删除
resize(int num, elem); // 变长以elem填充，变短超出的删除
capacity(); // 容器的容量
reserve(int len); // 容器预留len个元素长度，预留位置不初始化，不可访问
```

数据存取

```cpp
at(int idx);
operator[];
front(); // 第一个元素 
back(); // 最后一个元素
```

插入和删除

```cpp
insert(const_iterator pos, int count, elem);
push_back(elem);
pop_back(elem); // 删除最后一个元素
erase(const_iterator start, const_iterator end);
erase(const_iterator pos); // 删除指定位置元素
clear();
```

其他

```cpp
vector<int > v;
for (int i = 0; i < 10; i++) {
    v.push_back(i);
}
// 逆序遍历，注意要使用逆序迭代器
for (vector<int>reverse_iteartor it = v.rbegin(); it != rend(); ++i) {
    cout << *it << endl;
}

// vector的迭代器是随机访问的迭代器，支持跳跃访问
vector<int>::iterator itBegin = v.begin();
itBegin = itBegin + 3; // 这个代码如果编译不出错，表明这个容器支持随机访问，list容器不支持随机访问
```

## deque 容器

deque属于双端数组， vector也可以进行头插，但是效率很低

deque内部用的是中控器（保存缓冲区的地址）+缓冲区的结果

大部分的操作和vector是一样的e

```cpp
iterator // 普通的迭代器 
reverse_iterator // 逆序迭代器
const_iterator // const迭代器

// deque特有的
push_front(elem) ; // 前插
pop_front(); // 删除第一个元素
```

## Sort算法

```cpp
deque<int> d;
d.push_back(1);
d.push_back(9);
d.push_back(4);

sort(d.begin(), d.end());// algorithm头文件中, 从小到大排序

// 排序规则的回调函数
bool myCompare(int v1, int v2) {
    return v1 > v2;
}
sort(d.begin(), d.end(), myCompare); // 从大到小
```

## Stack 容器

先进后出的数据结构：First In Last Out\(FILO\)，只能访问栈顶元素

构造

```cpp
stack<T> s;
stach<T>(const stack &s); // 拷贝构造
```

存取操作

```cpp
push(elem); // 栈顶插入元素
pop(); // 栈顶弹出一个元素
top(); // 返回栈顶元素
empty(); 
size();
```

## queue 容器

队列容器，先进先出的数据结构First In First Out\(FIFO\),只允许由一端入队，一端出队，不提供遍历和迭代器

```cpp
// 构造函数
queue<T> q;
queue<T>(const queue& q);
// 存取操作
push(elem);
pop(); // 从队头删除一个元素
front(); // 返回第一个元素
back(); // 返回最后一个元素
// 赋值操作
queue& operator=(const queue &q);
// 大小操作
empty();
size();
```

## List 容器

List容器是一个双向循环链表，动态分配存储信息，消耗较大

List容器提供的迭代器是bidirectional Iterators, 不支持随机访问

List有一个重要性质，插入操作和删除操作不会造成原有迭代器的失效。这个在Vector容器中是不成立的，因为vector容器的插入操作可能引起内存的重新分配，导致原有的迭代器失效。

也可以支持逆序遍历，reverse\_iterator

```cpp
// 构造函数
list<T> l;
list(begin, end);
list(n, elem);
list(const list& l);

// 插入删除操作
push_back(elem);
pop_back();
push_front(elem);
pop_front();
insert(pos, elem);
insert(pos, n, elem);
insert(pos, beg, end);
clear();
erase(beg, end);
erase(pos);
remove(elem);

// 大小操作
size();
empty();
resize();
resize(n, elem);

// 复制操作
assign(beg, end); // 将[beg, end)区间的数据拷贝给本身
assign(n, elem); 
list& operator=(const list& l);
swap(l);

// 存取
front(); 
back();

// 反转排序
reverse(); // 链表逆序
sort() // list排序
// 所有不支持随机访问的迭代器，不可以使用系统提供的算法
// 如果不支持系统提供的算法，那么这个类内部会提供
// eg: l.sort(); // 从小打大排序
// 对于自定义类型需要指定回调函数

// !!! 注意！！！
// 删除自定义类型直接使用remove会报错，可以重载双等号来解决
```

## set、multiset容器

set容器的特性，所有的元素都会根据元素的键值自动排序，set的元素不像map那样可以同时拥有实值和键值，set元素既是键值又是实值。set不允许两个元素有相同的键值。

不可以通过set的迭代器来改变set容器的元素值，换句话说，set的迭代器是一种const\_iterator

set有和list一样的性质，原有的迭代器在插入删除元素后还是有效的，当然，被删除那个元素的迭代器除外。

multiset特性和set完全相通，唯一的差别在于multiset允许键值重复。set、multiset的话底层是用红黑树实现的。

相关操作和上面提到的操作基本一致，不在说明。

```cpp
// 插入
insert(elem); // 元素插入只有这个方法，插入后会自动按照从小到大排序
// 删除
erase(pos);
erase(elem);
erase(beg, end);

// 查找操作，key就是实值
find(key); // 查找键key是否存在，若存在，返回该元素的迭代器，不存在返回set.end()
count(key); // 统计键key的个数
lower_bound(keyElem); // 返回第一个key>=keyElem元素的迭代器
upper_bound(keyElem); // key>keyElem
equal_range(keyElem); // 返回相等的上下限两个迭代器

//set 容器不允许插入重复的值
insert(elem); // 返回值是pair<iterator, bool>, bool返回插入成功或失败

// set容器插入后默认是从小打大的，如果从大到小的话可以使用逆序迭代器
// 或者是在插入之前指定从大到小，指定规则, 仿函数
class myCompare {
public:
    // 重载小括号 ()
    bool operator()(int v1, int v2) {
        return v1 > v2;
    }
};
set<T, myCompare> s; 

// ！！！！插入自定义数据类型需要一开始指定排序方式
```

## Pair对组

```cpp
// 创建对组
pair<T1, T2> p(T1, T2);
pair<T1, T2> p = make_pair(T1, T2);

// 取值
p.first
p.second
```

## map、multimap容器

map的特性是会根据元素的键值自动排序，map所有的元素都是pair，同时拥有键值和实值，pair的第一个元素被视为键值，第二个元素被视为实值，同样和set一样，map也是不允许有两个元素有相同的键值（注意这里是键值，key值，value值可以相同）。

同样map也是不能根据迭代器更改元素内容的。

map和list一样，插入删除元素迭代器不失效。

multimap可以支持键值重复。

```cpp
// 构造函数
map<T1, T2> m;
map(const map& m);
// 赋值操作
map& operator=(const map& m);
swap(map);
// 大小
size();
empty();
// 插入
map.insert(...); // 返回的是pair<iterator, bool>
map<int, string> m;
m.insert(pair<int, string>(3, "Mike");
m.insert(make_pair(1, "Jack"); // 推荐使用
m.insert(map<int, string>::value_type(2, "Jason");
m[3] = "Leon"; // 可以用在访问上
// 删除
clear();
erase(pos);
erase(beg, end);
erase(keyElem); // 删除键值的对组
// 查找和set一致
```

multimap案例：存储员工信息：姓名 年龄 电话 工资等

## STL使用时机

vector使用场景：比如软件的历史操作记录的存储。

deque使用场景：比如排队购票系统

vector与deque的比较：

1. vector.at\(\)比deque.at\(\)的效率高，因为vector.at\(0\)是固定的，而deque的开始位置是不固定的
2. 如果有大量的释放操作的话，vector花费的时间更少
3. deque支持头部的快速插入和删除，是属于deque的优点

list的使用场景，公交车乘客信息的存储，随时有可能乘客下车，存在频繁的不确定位置元素的插入和删除

set的使用场景：手机游戏的个人得分的存储，要求从高分到低分顺序存储

map使用场景：比如按照ID账号存储十万个账号。

