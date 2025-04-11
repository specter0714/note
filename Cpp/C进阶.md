[TOC]

# fgets函数：

将一行字符从文件或屏幕中读入，可以读空格，遇到换行跳出

```c
#include<stdio.h>
int main()
{
	char str[100];
	fgets(str,99,stdin);
	printf("%s",str);
	return 0;
}
```

上面代码表示从标准输入stdin中读取一行，并存储在 str 数组中

它的标准形式为：fgets(str,n,fp)

str :指向存放字符串的起始地址指针

n :一个 int 类型的变量，指定最多读入的字符数（包括结尾自动添加的'\0'）

fp :文件指针，指向要读取的文件

# sqrt函数：

开平方根函数；

需要头文件包：#include<math.h>

# getchar函数：

很好用的；

```c
char i;
i=getchar();
```

# scanf函数：

也可以读空格和换行符

# pow函数：

指数函数 int pow(inta ,intb);

a 的 b 次方

```c
include<math,h>
int c;
c=pow(10,5)
```

# 函数fabs():

用来求***浮点数***绝对值

# 全局变量：

全局使用，自动初始化为0；

全局数组能开的更大

# 加减乘除返回值：

两个int 类型的值相乘返回值为int 类型；

int 与long相乘返回值为long类型

小乘大会往上转型

# 宏：

### 宏定义：

把一个东西直接替换成另一个东西

例如：

```c
#define a 1
#define str "abcdefg"
```

例如把数据类型开小了，要把所有的int 换成 long long可以使用宏定义

但是需要把 main 前面的 int 替换成 unsigned

```c
#inlcude<iostream>
#define int long long
unsigned main()
{
	int a=1;
	cout<<a;
	return 0;
}
```

# switch语句：

```c
switch(表达式)
{
    case 常量或者常量表达式1:语句体1;break;
    case 常量或者常量表达式2:语句体2;break;
    case 常量或者常量表达式3:语句体3;break;
    ... ...
        default:语句体;
}
```

例如：

```c
#include<stdio.h>
int main()
{
	int a=5;
	switch(a)
	{
		case 1:printf("%d",1);break;
		case 2:printf("%d",2);break;
		case 3:printf("%d",3);break;
		case 4:printf("%d",4);break;
		case 5:printf("%d",5);break;
		case 6:printf("%d",6);break;
            
	}
	return 0;
}
```

打印结果为：5

# 映射：

如题

![](C:\Users\29287\Pictures\Screenshots\屏幕截图 2024-09-12 155454.png)

本题可以用映射的思路，将第一个数组编程单调递增的数组，然后第二个数组按照同一种映射方式进行变换，只需要求得第二组最长严格上升子序列，就能得到最长公共子序列。

```c
#include<stdio.h>
const int N = 100010;
int a[N], b[N], map[N], f[N];
int max(int a, int b) {return a > b ? a :b;}
int main()
{
    int n;
    scanf("%d", &n);
    
    //做映射
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
        map[a[i]] = i;
   }
    for (int i = 1; i <= n; i++) {
        scanf("%d", &b[i]);
        b[i] = map[b[i]];
   }
    //背景部分的代码
    int len = 1;
    for (int i = 1; i <= n; i++){
        f[i] = 1;
        for (int j = 1; j <= i; j++)
            if (b[j] < b[i]){
                f[i] = max(f[i], f[j] + 1);
                len = max(len, f[i]);
           }
   }
    printf("%d", len);
    return 0;
}
```

# 有多少个矩形：

一个矩形有多个小正方形组成，问，其中有多少个矩形，有多少个正方形，多少个长方形。

思路如下：在长上选择两个端点，在宽上选择两个端点，四个端点组成一个矩形。

加设矩形的，长为 m ，宽为 n 。

长上选两个端点：组合数(m + 1)中选两个：(m + 1) * m / 2

宽上选两个端点：组合数(n + 1)中选两个：(n + 1) * n / 2

矩形总数为：

```c
((m + 1) * m / 2) * ((n + 1) * n / 2);
```

正方形边长相等，所以正方形总数为：

```c
while(n > 0 && m > 0){
	num += n * m;
	n--;
	m--;
}
```

长方形总数等于  矩形  -  正方形

# 深度优先搜索算法：DFS

例题：
一矩形阵列由数字 0 和 1 组成，以 0 为分界线，将非 0 的数字分成多少个区域

![](C:\Users\29287\Pictures\Screenshots\屏幕截图 2024-09-14 125556.png)

输入样例：

4 10

0111000011

1011110100

1011100111

0000000011

输出样例：

4

程序代码如下：

```c++
##include<iostream>
using namespace std;
char a[1010][1010];
int main()
{
	int n, m;
    cin >> n >> m;
    for(int i = 0; i < n; i++){
		for(int j = 0; j < m; j++){
			cin >> a[i][j];
        }
    }
    int cnt = 0;
    for(int i = 0; i < n; i++){
		for(int j = 0; j < n; j++){
			if(a[i][j] == '0'){
				cnt++;
                dfs(i, j);
            }
        }
    }
    return 0;
}

void dfs(int x, int y)
{
	if(x < 1 || x > n || y < 1 || y > m ||a[x][y] == '0')return;
    a[x][y] = '0';
    dfs(x + 1, y);//下
    dfs(x - 1, y);//上
    dfs(x, y + 1);//右
    dfs(x, y - 1);//左
}
```

# DFS （深搜）与 回溯 与 N皇后：

***DFS***：对一张具体的，有节点，有边的，图的遍历

***回溯法***：对一颗隐形的树的遍历，每搜完这棵树的子树后就会将该子树回溯为未，来进行下一个情况的搜素

  步骤：

声明数组：a[i]:第 i 行的皇后放在第 a[i] 列

 声明数组：b[i],c[i],d[i]:bool类型

分别标记当前列，当前两条对角线是否可用，false 为存在冲突，进行剪枝

标记当前列的冲突情况：如果第 1 列放了皇后则b[1] 赋值为false,直接使用下标表示

## 全排列：

```c++
#include<iostream>
using namespace std;
int a[50];
int book[50];

void dfs(int n, int stap);

int main()
{
	int n;
	cin >> n;
	dfs(n, 1);
	return 0;
}

void dfs(int n, int stap)
{
    //递归结束条件
	if(t == n + 1){
        //输出结果
		for(int i = 1; i <= n; i++){
			cout << a[i];
		}
		cout << endl;
		return;
	}
    //递归关系式
	for(int i = 1; i <= n; i++){//遍历每一个数字
		if(book[i] == 0){// i 没有用过的
			a[t] = i;//将 i 存入数组
			book[i] = 1;//标记 i 已经用过
			dfs(n, t + 1);//往后移动一位
			book[i] = 0;//回溯
		}
	}
}



```



# include\<map\>:

map 是C++标准模板库(STL)的一个关联容器，提供一对一的映射关系

“第一个”称为关键字(key)，别名是 first, 每个关键字只能在map中出现一次

“第二个”称为该关键字的值(value)，别名是second

map  以模板（泛型）的方式实现，可以存储任意类型的数据，包括使用者自定义的数据类型：

例如：

```c++
map<int, int>mp;
map<int, string>m2;
map<string, string>m3;
```

### 一：

可以定义一个map对象

```c++
map<string,string>friends;
```

把朋友-电话号码保存在 friends 中：

```c++
friends.insert("Alice","13824611111");
friends.insert("Bob","1390279779798");
friends.insert("jimy","798123790980");
friends.insert("Jett","783728173810");
```

要查询朋友的电话号码：

```c++
cout << "Jett的号码是：" << friends.find("Jett");
```

### 二：

给出一段英文

ksdfghaskjhdaskjhlsahdlsa

统计其中每个字母出现的次数

把每个字母的出现次数保存在 cnt 中

```c++
map<char,int>cnt;
```

 把次数初始化为 0；

```c++
cnt.insert('a',0);
cnt.insert('b',0);
```

若遇到就使储存那个字母出现次数的cnt自增，取代 insert 动作

```c++
cnt['a']++;
cnt['b']++;
```

## map插入元素的方式：

### 方式一：

```c++
map<int, string>student;
```

用insert函数插入pair(pair是构造函数)

```c++
student.insert(pair<int, string>(0, "Zhangsan"));
```

### 方式二：

```
map<int, string>student;
```

用 insert 函数插入 value_type 数据：（调用 map 的一个静态成员函数 value_type）

```c++
student.insert(map<int, string>::value_type(1, "Lisi"));
```

### 方式三：

```c++
map<int, string>student;
```

用类似数组的方式增加元素：

```c++
student[123] = "Wangwu";
```

## map查找元素的方式：

find( ) 返回一个迭代器，指向查找的元素，找不到，则返回map::end( )位置（NULL）

```c++
iter = student.find(123);
if(iter != student.end( )){
	cout << "found,the value is" << iter -> second;
}
else{
	cout << "not found";
}
```

如果关键字是整形的，也可以通过

studebt[1] 读取 关键字 1 对应的值，如 "Lisi"



# unordered_map

```c++
include<unordered_map>

unordered_map<int, int> map;

//直接这样
int k = map[1];
int k = map.count(1);//这两个当键不存在时都会返回0 

//map的遍历
for(auto i = map){
    std::cout << i.first << ' ' << i.second << endl;
}

typedef pair<int, int> PII;
for(PII i : map){
    std::cout << i.first << ' ' << i.second << endl;
}
//map的增强for循环遍历实际上是将map的键赋值给pair<int, int>
```



# 关于汉字：

汉字是需要两个字符的空间来储存

# 头文件#include\<algorithm>

这行代码是在 C++ 程序中包含了`<algorithm>`标准库头文件。

`<algorithm>`头文件提供了大量的算法，比如排序（`sort`）、查找（`find`、`binary_search`等）、遍历（`for_each`）、反转（`reverse`）等。这些算法可以对各种容器中的元素进行操作，提高编程效率。

### 1.sort函数：

```c++
#include<algorithm>
const int N = 8;
int a[N] = {1, 0, 9, 8, 6, 4, 0, 2};
sort(a, a + N);//从小到大排序
sort(a, a + N, less<int>());//从小到大排序
sort(a, a + N, greater<int>());//从大到小排序
```

也可以

```c++
int a[8] = {1, 0, 9, 8, 6, 4, 0, 2};
bool mycmp(int &a, int &b){
	if(a > b)return 1;
	else return 0;
}
sort(a, a + 8, mycmp);//相当于sort(a, a + 8, greater<int>());
```

当然也可以是

```c++
typedef pair<int, int> PII;
vector<PII> all;
bool mycmp(PII &a, PII &b)
{
    if(a.first < b.first)return 1;
    else return 0;
}
sort(all.begin(), all.end(), mycmp);//依照第一个元素从小到大排序
```



# 头文件#include\<vector>

```c++
vector<int > a(9, 3);//创建
vector<int > b(a);//将a容器整体复制到b容器
vector<int > c(b. begin() + 1, b. begin() + 3);//将b容器指定区域复制到c容器

int k[5] = {3, 5, 4, 1, 2};
vector<int > d(k + 1, k + 3);//将指定数组区间复制到d

vector<vector<int > > e;//定义二维vecter，取数据时可当作二维数组
vector<vector<int > > f(3, vecter<int >(2, 4));//二维vector初始化，第一维长度为3， 第二维长度为2， 值均为4（其实就是创建了3行2列的的数组，值均为4）

vector<int > g;
g.assign(b. begin() + 1, b. begin() + 3);//将向量b指定区间的元素复制给g容器，相当于清空g容器（容器不变，超出会自动扩容）， 再将指定区间赋值给g容器
g. assign(2, 3);//将g清空，然后赋上两个3

a. push_back(2);//从容器末尾插入一个2

for(int i = 0; i < a. size(); i++){//a. size()返回a容器中的元素个数
	cout << a[i];//就像数组那样取出
}

a. capacity()//返回当前容器的容量数

a. reserve(x)//将当前容器的容量改为x

a. front()//返回容器第一个元素

a. back()//返回容器最后一个元素

a. clear()//清空容器（但容量不变）

a. empty()//判断容器是否为空，为空则返回true，否则返回false

a. pop_back()//删除最后一个元素
    
a. swap(b)//将两个同类型的vector容器交换

a.erase(a.begin() + 2)//接受一个迭代器作为参数，该迭代器指向要删除的元素，函数返回一个指向被删除元素之后位置的迭代器

a.erase(a.end() - 5, a.end())//接受两个迭代器作为参数分别表示要删除的元素范围的起始和结束位置。函数返回一个指向被删除范围之后位置的迭代器
    
a.insert(a.begin() + 1, 3)//在迭代器指向的位置插入一个元素3
    
a.insert(a.begin() + 2, 4, 5)//在迭代器指向的位置插入4个1

int t[100] = {...................};
a.insert(a.begin() + 4, t + 1, t + 7)//在迭代器指向的位置开始插入数组中从t + 1到t + 7区间的数,这后两个参数也可以是其他同类型的迭代器区间
```

### 迭代器：

迭代器是一个变量，内部比指针还要高级，但用的时候暂时当作指针去用

```c++
容器类型的名字::iterator 迭代器变量名
```

```c++
vector<int >::iterator it;
```

现在只是创建了一个迭代器变量，它没有指向任何容器，所以要给它赋值



使用a. begin()可以返回a容器的初始迭代器，指向第一个位置

使用a. end()可以返回a容器的末尾迭代器，指向了最后一个位置

一般将a. begin()赋给迭代器， 让迭代器指向初始位置，将a. end()作为遍历容器的结束条件

```c++
#include <cstdio>
#include <vector>
using namespace std;

int main (void) {
	vector<int> arr {3, 5, 4, 1, 2};
	vector<int>::iterator it;
	for (it = arr.begin(); it != arr.end(); ++it) {
		printf ("%d ", *it);
	}	// 输出：3 5 4 1 2 
    return 0;
}
```

# 头文件#include\<stack>

栈  “先进后出”

```c++
#inlcude<stack>
using namespace std;

stack<int> st, st2;

st.push(3);//把3压入栈

st.top();//返回栈顶元素（最后那个数）

st.pop();//消除一个栈顶元素

st.size();//返回当前栈又几个元素

st.empty();//判断当前栈是否为空，为空返回1，不为空返回0

st2.swap(st);//交换栈（C++11标准）
```

# 头文件#include\<queue>

队列  “先进先出”

```c++
#include<queue>
using namespace std;

queue<int> q, q2;

q.push(3);//将3入队

q.front();//返回队首元素

q.back();//返回队尾元素

q.pop();//将一个队首元素出队

q.size();//返回队列中又多少元素

q.empty();//判断当前队列是否为空，为空返回1，不为空返回0

q2.swap(q);//交换队列（C++标准）
```

### 优先队列（堆）

```c++
#include<queue>
priority_queue<int> a;//默认为大根堆

priority_queue<int, vector<int>, greater<int> > q;//小根堆
priority_queue<int, vector<int>, less<int> > p;//大根堆
```

### 仿函数，堆

```c++
typedef struct three{
    int num;
    int p1;
    int q1;
}TH;

struct ennsmall{
    //下面的opperator是关键字，后面要加()(参数)；
    bool operator()(TH x, TH y)
    {
        return x.num > y.num;
    }
};

priority_queue<TH, vector<TH>, ennsmall> small;//小根堆
```



### 头文件#include\<deque>

双端队列，可两端入队，比vector功能多但效率更低，vector有的功能这里不介绍

```c++
#include<deque>
using namespace std;

deque<int> q, q2;

q.push_back(3);//将3入队尾

q.push_front(5);//将5入队首

q.front();//返回队首元素
    
q.back();//返回队尾元素

q.pop_front();//将一个队首元素出队

q.pop_back();//将一个队尾元素出队

q.size();//返回队列中有多少元素

q.empty();//判断当前队列是否为空，为空返回1，不为空返回0

q2.swap(q);//交换队列
```

# 头文件#include<unordered_map>

 内部有哈希表实现，一般可以在O(1)的时间复杂度实现查找

```c++
#include<unordered_map>
unordered_map<string, int> a;

string t;

a.count(t);//因为是哈希表，键是唯一的，所以存在返回1，不存在返回0
```

# 头文件#include\<string>

```c++
//string类型有很多操作
#include<string>
string a = "aaa";
string b = "bbb";
string c = a + b;//string类型可以相加但不可以相减，相加为"aaa" + "bbb" = "aaabbb"

if(a == b)c = "ccc";//string 类型也可以用来直接比较

//string的函数

c.find('c');//找到第一个'c'，并返回它所在位置的下标

char p = 'A';
c.append(1, p);//将 1 个 'A' 添加到字符串末尾
```

# 加快cin 读取速度

```c++
ios::syoc_with_stdio(false);
```

优点：加快 cin 读取速度

缺点：不能使用 scanf 了

# 三个码

例如：

原码：00010101

反码：11101010

补码：11101011

原码 + 补码 = 00000000

# reverse

使用头文件include\<algorithm>

将指定区间或迭代器范围内的元素反转

数组的话，传两个指针，左闭右开

```c++
#include<algorithm>
#include<vector>
int a[5] = {1, 2, 3, 4, 5};
reverse(a + 1, a + 4);
//或者
vector<int> a = {1, 2, 3, 4, 5};
reverse(a.begin(), a.end());
```

# pair

需要使用头文件

include\<utility>

例如

```c++
#include<iostream>
#include<utility>
using namespace std;
pair<int, int> a(1, 10000);
pair<int, string> b(100, "kslajdl");
```

调用值：

```c++
cout << a.first;
cout << b.second;
```



# auto

是一个关键字用于自动类型推导

### 作用：

1. 简化变量声明：当使用 auto 声明变量时，编译器会根据变量的初始化表达式自动推断出变量的类型。这可以减少代码中的冗长类型声明， 特别对于复杂的模板类型。

   例如：

   ```c++
   vector<int> v = {1, 2, 3};
   for(auto it = v.begin(); it != v.end(); it++){
   	cout << *it << ' ';
   }
   ```

   这里 auto 被用来自动推导迭代器 it 的类型，而不需要显式的写出 vector\<int>::iterator

2. 提高代码的可维护性：如果一个变量的类型在未来发生了变化，使用 auto 可以减少需要修改的地方。只要初始化表达式的类型不变， 使用 auto 声明的变量的类型也会自动更新。

### 注意：

1.  auto 不能用于推导函数参数的类型，也不能用于推导数组的类型。
2.  auto 推导的类型时一个值类型，而不是引用类型。如果需要声明一个引用类型的变量，可以使用 auto& 或者是 const auto& 。

# unique

### 用法：

需要头文件#include\<algorithm>

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;


int main()
{
    vector<int> a = {1, 2, 3, 3, 4, 4, 5};
    auto if = unique(a.begin(), a.end());
    a.erase(it, a.end());
	return 0;
    
}
```

先使用 unique 对 a 容器中的元素进行去重，然后通过 erase 函数删除去重后范围末尾到容器末尾的元素，最终得到去重后的容器。

## 原理：

unique 并不是真正地删除重复元素，而是将不重复的元素依次移动到容器的前部，覆盖重复的元素，然后返回一个指向新的逻辑末尾的迭代器。

### 注意：

unique 只能移除连续的重复元素，如果元素不是连续重复的，可能无法完全去重。

通常需要结合其他操作(如 erase )来真正删除被覆盖的重复元素，以达到完全去重并调整容器大小的目的。

# 二进制运算符：

^  按位异或：不相同为1，相同为0

|  按位或：至少一个是1则为1， 否则为0

&  按位与：都是1则为1，否则为0

# 取模保证数值不溢出：

再计算多个数相乘再取模时，例如：

计算a ^ 20000 % 123

如果直接计算a^20000数据会溢出

可以**乘一次再取模一次**

例如：

```c++
int sum = 1;
int a;
cin >> a;
while(20001--){
	sum *= a;
	sum %= 123;
}
cout << sum;
```

# 判断是否为数字字符的函数

```c++
#include<cctype>
char x;
bool m = isdigit(x);
```

如果 x 为数字字符，返回true，否则返回false

