# CPlusPlus_STL标准库

怎么访问set元素，只能迭代器；

怎么查询set里是否有某个元素，find()返回的是迭代器，找到了，找不到返回指向末尾元素后一个位置的迭代器

怎么查询map里某个元素是否出现过，count() 注意count的参数是键；



![image-20250318230506866](images/image-20250318230506866.png)

## 1.sort快排

* ==**sort快排需要包含头文件<algorithm>**==
* 可以给数组、结构体快速排序，尤其是给结构体按照某种方式排序
* 默认排序为从小到大
* sort进行排序时，前俩个参数是要排序对象的首地址和尾地址，**对数组进行排序时**：sort(a,a+10)；**对容器进行排序时**（比如vector<string>s):   sort ( s.begin() , s.end() );**对string类型进行排序时**，string s  :  sort ( s.begin(), s.end() );

```C++
#include <iostream>
#include <algorithm>//sort快排的头文件  algorithm:算法
//sort()函数一大用法是结构体快速排序：此时需要自定义bool函数
struct People{
	int number;
	int score;
};
bool cmp(People p1,People p2){  //函数类型为：bool  形参为 结构体
	return p1.score>p2.score;   //按照谁的score大谁排在前面
	//return p1.score>p2.score||(p1.score=p2.score&&p1.number<p2.number)//排序规则为谁score大谁排在前面，score一样大比较number,nummber小的排在前
}
using namespace std;
int main(void) {
	int a[10] = {10, 20, 60, 30, 40, 90, 60, 90, 50, 80};
	cout << "原数组顺序：";
	for (int i = 0; i < 10; i++) {
		cout << a[i] << " ";
	}
	cout << endl;  
	sort(a, a + 10);                      //从小到大排序用法
	cout << "sort(a,a+10),从小到大排序之后的数组顺序：";
	for (int i = 0; i < 10; i++) {
		cout << a[i] << " ";
	}
	cout << endl;
	cout << "sort(a,a+10,greater<int>,从大到小的排序后的数组排序：";
	sort(a, a + 10, greater<int>());      //从大到小排序用法
	for (int i = 0; i < 10; i++) {
		cout << a[i] << " ";
	}cout<<endl;
	People p[5];
	cout<<"请输入5行，每行两个数字，分别代表序号和分数：";
	for(int i=0;i<5;i++){
		cin>>p[i].number>>p[i].score;//1 20 3 78 4 90 67 80 5783 2
	}
	sort(p,p+5,cmp);
	for(int i=0;i<5;i++){
	  cout<<p[i].number<<" "<<p[i].score<<endl;	
	}
	return 0;
}
```

## 2.set/  multiset / unordered_set / unordered _multiset容器

* set是内部自动有序（小的在前）而且不含重复元素的容器。所以结构体作为set的元素时，需要重载运算符<；
* set这个东西默认小的东西在前，所以重载小于运算符时
* 使用结构体对作为set容器的元素时，必须要进行重载运算符

### 2.1set的定义

* 头文件#include <set> 可以使用 set 、multiset ；#include <unordered_set>可以使用 unordered_set和unordered_multiset；#include <unordered_set>是无序的，要比普通的set速度快很多，所以优先推荐使用 unordered_set

* ```C++
  set<int> s[100];
  //这里的a[0]~a[99]都是set容器
  ```

### 2.2set容器内元素的访问

* 只能通过迭代器访问

### 2.3set常用函数

* insert(x) :将x插入set容器中，并自动递增排序和去重  O(log N)

* find( value) :返回set容器中值为 value 的**迭代器**，想输出这个值需要 *it  O(log N)

* erase()两种用法：

  * 删除某一个值：参数可以为元素的值 O(log N) 或者迭代器 O(1)
  * 删除一个区间内的值：erase(first,end); 参数都是迭代器，而且，end（）是要删除的最后一个元素的下一个地址；所以这个区间也是左闭右开的；

* size() 时间复杂度 O(1)

  ```C++
  erase(value) :value为要删除元素的值
  erase(200) = erase(s.find(200));
  //两个erase的参数不一样的
  ```

* clear() 清空所有元素 ： O(N)

### 2.4其他

* 所有带前缀的 set map 不支持 upper_bound / lower_bound ,也不支持迭代的++ ，- -;凡是和排序有关的操作都不支持 ,所以无法用 迭代器遍历

* **set / multiset 包含头文件set; unordered_set /unordered _multiset 包含头文件 <unordered_set>**

* **有序的容器操作复杂度是 log(N)无序的操作复杂度是o( 1 )：所有 unordered_set / unordered _multiset复杂度是o(1 )**

* set放入元素之后，**自动排序** (升序）而且**去除重复**

* ==include <set>==

* 容器就类比数组，数组是整数到整数或者浮点数的映射，但是容器是迭代器到各种类型的映射

* **常用函数**

   * **s.insert(x);**将x插入容器中

   * **s.erase(x)**删除x，有两种参数

     ```C++
     (1)输入一个数，删除所有x; 时间复杂度 o( k + log( n ) ) ;k是x的个数
     (2)输入一个迭代器，删除这个迭代器
      //在set中没有区别，在multiset中有区别
     ```

     

   * **set<int>s;**相当于int s[];其中数组的大小是不确定的

   * **s.begin()**返回第一个元素地址

   * **s.end()**指向最后一个元素的后面一位地址（返回容器末尾的地址）

  	* **s.find( )** 查找一个数 ，如果不存在，返回指向end( )的迭代器；如果存在，返回指向该元素的迭代器

  ```C++
  
      multiset<int>s1;
  	for(int i=1;i<=20;i++){
  		s1.insert(i);
  	}
  	auto it=s1.find(7);
  	if(it!=s1.end()) cout<<"Yes";
  	else cout<<"No"<<endl;
      
  ```

  * **count( )**返回某一元素的个数，在set中只有0和1两种情况，multiset中可以有多个

  * **核心操作： lower_bound( ) / upper_bound( )**

    ```C++
    lower_bound( ) :返回大于等于 x 的最小的迭代器
    upper_bound( ) :返回大于 x 的最小的迭代器
    如果不存在，返回end( )
    ```

    

* 如果想遍历容器中的数据，用迭代器进行遍历

  ```c++
  set<int>::iterator it;//it是迭代器名称，可以随便自定义
  for(set<int>::iterator it=s.begin();it!=s.end();it++){
      cout<<*it;
  }
  ```

* 字符串类型按字典排序首选set容器

* **multiset 不会把元素去重**

* #####  set和结构体搭配使用时，元素的插入，直接在结构体内部定义一个东西，更方便赋值

  * 结构体名 （参数列表）：成员名(形参) , 成员名(形参)  {}  

* ```C++
  typedef struct PRODUCT
  {
  	int type;	//类型 
  	int id;		//编号 
  	int score; 	//得分 
  	
  	PRODUCT(int a,int b,int c):type(a),id(b),score(c){}
  	
  	//重定义<操作符，这决定了set的自动排序 
  	bool operator < (const PRODUCT &rhs) const
  	{
  		if(score!=rhs.score)
  			return score>rhs.score;
  		else if(type!=rhs.type) 
  			return type<rhs.type;
  		return id<rhs.id;			
  	}
  }PRODUCT; 
  
  
  //插入值的时候，需要这样插
  set<PRODUCT>s;
  s.insert(PRODUCT(j,id,score));
  
  ```

  

**eg:输入一个英文文本，将文本中的单词按照字典序进行排列之后输出，不区分大小写**

* 涉及字符串排序，使用set容器
* 文本涉及英文之母之外，还涉及各种标点符号，所以处理时，先对文本进行处理，全部转换为小写，不是英文符号的，转换成空格便于stringstream提取
* 读入带空格的文本，使用getline()

```c++
#include <iostream>
#include <set>
#include <string>
#include <sstream>
using namespace std;
int main(void){
	set<string>s;
	string a,buf;
	getline(cin,a);//读取一大串文本
		for(int i=0;i<a.length();i++){
			if(isalpha(a[i])){  //如果是字母返回非零，否则返回0
				a[i]=tolower(a[i]);
			}else{
				a[i]=' ';
			}
	 	}
	 	stringstream ss(a);
	 	while(ss>>buf){
		 	s.insert(buf);
		 }
	for(set<string>::iterator it=s.begin();it!=s.end();it++){
		cout<<*it<<'\n';
	}
	return 0;
}
```

## 3.映射map / multimap / undered_map / unordered_multimap

* 直接使用map的话，会按照键给它的元素排序；使用unordered_map速度快得多，但是不会排序；还有map是一个键只对应一个值的，就是说，不可能有不同的值对应一个下标，如果需要一个键对应多个值，需要使用multimap

* 体验以下排序和不排序的效果

* ```c++
  
  #include <iostream>
  #include <cstdio>
  #include <map>
  #include <unordered_map> 
  using namespace std;
  int main(void) {
  	map<int, int> m;
  	m[0] = 1;
  	m[1] = 2;
  	m[10] = 2;
  	m[4] = -1;
  	cout << endl;
  	for(map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
  		cout << it->first << ' ' << it->second << ' ' << endl;
  	}
  	cout << endl;
  	
  	return 0;
  }
  //结果：发现输出的时候按照下标排序了
  0 1
  1 2
  4 -1
  10 2
  //////////////////////////////////////////////////////////////////////////////
  #include <iostream>
  #include <cstdio>
  #include <map>
  #include <unordered_map> 
  using namespace std;
  int main(void) {
  	unordered_map<int, int> m;
  	m[0] = 1;
  	m[1] = 2;
  	m[10] = 2;
  	m[4] = -1;
  	cout << endl;
  	for(unordered_map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
  		cout << it->first << ' ' << it->second << ' ' << endl;
  	}
  	cout << endl;
  	return 0;
  }
  发现：后赋值的先输出来
  4 -1
  10 2
  1 2
  0 1
  
  ```

* **count(x)函数：返回map容器中key出现的次数，因为key值（下标）出现的次数只能是0或1，所以返回值也只有0或1；**

* **插入数据**

    ```C++
    map<string,int>m0;
    m0.insert(pair<int,string>(000,"zero"));//第一种
    m0[123]="one";//第二种直接赋值操作
    x
    ```

* **支持下标访问：完全像数组一样！遍历map中的元素** 

    ​	用下标访问时间复杂度是 o(log n);数组用下标是 o( 1 )

    ```C++
    #include  <iostream>
    #include <map>
    
    using namespace std;
    map<string,int >m0;
    int main(void){
    	m0["abc"]=123;
    	m0["def"]=456;
    	m0["ghi"]=789;
    	for(map<string ,int>::iterator it=m0.begin();it!=m0.end();it++){
    		cout<<it->first<<":"<<it->second<<endl;	
    	}
        //first是指key值，second是指value
    	return 0;
    }
    ```

* **支持函数**：

    * **insert( )**;因为map是映射，所以插入删除都是两种数据类型，可以看为 pair

    * **erase():**删除一个元素，参数是键（下标）或者迭代器

    * **find()**查找一个元素: 返回的是一个迭代器

      * 在容器中寻找键（下标）为k的元素，返回该元素的迭代器。否则，返回map.end()，注意参数是下标

      * ```C++
        map<string, int> m;
        m["qwe"] = 1;
        m["abc"] = 2;
        if(m.find("abc") != m.end()){
        	cout << m["abc"];
        }
        ```

        

    * **size()**;返回map中元素的个数

    * **clear()**:清除所有元素

    * **lower_bound( ) / upper_bound( )**

    ```C++
    
    #include  <iostream>
    #include <map>
    #include <cstdio>
    using namespace std;
    map<string,int >m0;
    int main(void){
    	m0["abc"]=123;
    	m0["def"]=456;
    	m0["ghi"]=789;
    	map<string ,int>::iterator it;
        
    	it=m0.find("abc");
    	cout<<it->first<<" "<<it->second;
        
        
    	iter = mapStudent.find("123");
    	mapStudent.erase(iter);
    
    	return 0;
    }
    ```

    

* **例题**：输入一个英文文本，（各单词之间以空格隔开），以#结尾；找出所有满足如下条件的单词：该单词不能通过字母重排，和文本中其他的单词相同。在判断这个条件时，忽略字母的大小写；但是输出时，保留输入中的大小写，并将单词按字典序进行排列

  * 对大量单词进行字典序排序----对数组元素进行排序，用sort;但是这是一个元素是string类型的数组，就要使用vector;
  * 在读入文本的时候，就记录这个单词出现的次数；map容器key和value 的对应，以及count函数是找key值（下标）;
  * 找出不分大小写，只是每隔单词的顺序不同：将每个单词变为小写，**并把每个字符串排序，同样用sort**

  ​		

  ```c++
  #include <iostream>
  #include <vector>
  #include <map>
  #include <string>
  #include <cctype>//关于字符串的变换操作的头文件，里面的函数的参数都是一个字符
  #include <algorithm>
  using namespace std;
  //set<string>set0;
  //set<string>set1;
  vector<string>v0;
  vector<string>v1;
  map<string,int>map0;
  string standard(string s){
  	//下面将字符串全部变为小写，并进行排序，存储的时候就记录
  	for(int i=0;i<s.length();i++){
  		s[i]=tolower(s[i]);
  	}
  		sort(s.begin(),s.end());//注意：给字符串排序
  	return s;		
  			
  }
  int main(void){
  	string s;
  	while(cin>>s){
  		if(s=="#"){
  		  break;
  		}
  		//将读入的字符串储存两份，一份容易排序，另一份保存原样，后面容易提取
  		v0.push_back(s);
  		
  		string str=standard(s);
  		if(map0.count(str)){ //count()函数通过下标寻找键值
  			//找到这个下标，返回1,代表不是第一次出现
  			map0[str]++;//它的值加一
  		}else{
  			map0[str]=1;
  		}
  		
  	}
  	//下面筛选出出现次数不止一次的字符串并存入新的容器中输出
  	for(int i=0;i<map0.size();i++){
  		if(map0[standard(v0[i])]==1){
  			v1.push_back(v0[i]);//容器v1存储的是答案字符串
  		}
  	}
  	sort(v1.begin(),v1.end());
  	for(int i=0;i<v1.size();i++){
  		cout<<v1[i]<<endl;
  	}
  	return 0;
  }
  ```

  

## 4.vector

### 4.1vector的定义

* 数组的加强版，长度不固定，可以存储各种类型数据，直接通过下标访问；相当于是int到各种基本类型的映射；

* **头文件#include <vector> 以及using namespace std;**

* ```
  vector<typename> name;
  eg: vector<int>name;
  //相当于一个长度可变的一维数组name[n];
  	vector<int>name[N];
  //第一维可变第二维固定的数组
  	vector< vector<string> >name;
  //两个维度都可变的数组:搞不懂，最好不要这么使用
  	vector<node>name;
  //自定义的结构体，也可以这么使用
  	
  ```

### 4.2vector内元素的访问

* 两种方式：下标访问和迭代器访问

* 下标访问：v[0] v[1]等等，注意下标范围是 0 ~ v.size()-1

* 迭代器访问:第一种写法

  ```C++
  vertor<typename>::iterator it;
  //迭代器的定义
  //可以通过*it访问vertor里面的元素
  
  #include<iostream>
  #include <vector>
  using namespace std;
  int main(void)
  {
  	vector<int> vi;
  	for(int i = 0;i < 5; i++) {
  		vi.push_back(i);
  	}
  	vector<int>::iterator it = vi.begin();
  	//vi.begin()为取vi的首元素地址，让it指向这个地址
    
  	for(int i = 0;i < 5; i++){
  		printf("%d ",*(it + i));
  	} 
  	return 0;
  }
  
  ```

* 我们发现*(vi.begin()+i)和vi[i]是等价的

* 补充一下vi.end()函数，和begin()不同的是，end()不是取vi的尾元素地址，而是取尾元素地址的下一个地址。end()作为迭代器末尾标志，不存储任何元素。美国人思维比较习惯左闭右开。

* 迭代器还有 ++it 、it++操作，于是有另一种写法

  ```c++
  #include<iostream>
  
  #include <vector>
  
  using namespace std;
  
  int main(void)
  {
  	vector<int> vi;
  	for(int i=0;i<5;i++) {
  		vi.push_back(i);
  	}
  	vector<int>::iterator it = vi.begin();
  	//vi.begin()为取vi的首元素地址，让it指向这个地址
  	
  	for(vector<int>::iterator it=vi.begin(); it != vi.end(); it++){
  		printf("%d ", *it);
  	}
  	return 0;
  }
  
  ```

### 4.3vector的常用函数

* push_back(),数组后面添加一个元素 O(1)

* pop_back()删除数组的尾元素  O(1)

* size() 获得容器的元素个数，返回的 unsigned 类型，使用 %d 无伤大雅 O(1)

* clear() 清除所有元素 O(N)

* insert(it,x):在迭代器it处，插入x,注意是插入，在迭代器原来的元素以及它后面的元素，会顺势向后推一位，时间复杂度 O(N)

* erase() 删除单个元素或者一个区间内的所有元素 O(N)

  * erase(it) :删除it处的元素，后面的元素会补上来
  * erase(first,end) : 删除 [first, last)内的所有元素，注意右边是开区间

* ```C++
  //如果想删除所有元素的话，是erase(vi.begin(), vi.end());但是我们有clear()函数
  ```


### 4.4其他

```C++
#include <iostream>
#include <vector>
using namespace std;
vector<string>v;

int main(void){
	v.push_back("ddsd");
	v.push_back("我是");
	cout<<v[1];//输出结果是"我是"
	
	return 0;
}
```

* set容器虽然自动去重，但是不能直接通过下标访问，相比之下，这是vector容器的一大优势；

* 定义一个长度为10,且值全为3的vector

  ```C++
  vector<int>a(10,3);
  for(auto x:a) cout<<x<<" ";
  //3 3 3 3 3 3 3 3 3 3 
  ```

* 定义vector数组

  ```C++
  vector<int>a[10];//定义了10个vector
  ```

* 支持的函数：

    * **size( ):**返回容器中元素个数****
    * **empty( )** ;如果是空的就返回 true ,否则返回false,这两个函数，所有容器都有
    * **clear( )** :清空 ；C++中操作系统，为某一进程分配空间时，所需时间与空间大小无关，与申请次数有关；因为这个特点，所以变长数组 vector 要尽量减少申请空间的次数:先定义长度为32，每次长度不够时，再申请一个长度为64的，再把之前的元素copy过来（倍增思想）
    * **front( )** 返回第一个数
    * **back( )**返回最后一个数
    * **push_back()**向尾部添加元素
    * **pop_back()**删除最后一个元素；
    * 具有迭代器：end( )指向最后一个元素的后面的数；begin( )指向第一个数
    * vector 支持随机寻址，和数组一样 [ ]

* 遍历vector中的元素

  ```C++
  	vector<int>a;
  	for(int i=0;i<10;i++)	a.push_back(i);
  	//采用数组下标遍历
  	for(int i=0;i<a.size();i++) cout<<a[i]<<" ";
  	cout<<endl;
  	//采用迭代器遍历:解引用
  	for(vector<int>::iterator it=a.begin();it!=a.end();it++) cout<<*it<<" ";
  
  //vertor<int>::iterator 可以写成auto;auto是C++关键字，可以让系统自动推断变量类型
  	cout<<endl;
  	//c++11新语法遍历:可以遍历任何容器
  	for(auto x:a) cout<<x<<" ";
  	cout<<endl;
  ```

* 支持比较运算：按照字典序进行比较

  ```C++
  vector<int>a(4,3);//4个3
  vector<int>b(3,4);//3个4
  if(a<b) cout<<"a<b";
  ```

  

## 5.queue

* 头文件==：<queue>==
* push() 在队尾插入一个元素
* pop() 弹出队列第一个元素
* front() 返回队列中的第一个元素
* back() 返回队列中最后一个元素
* size() 返回队列中元素个数
* empty() 如果队列空则返回true,即返回1
* queue没有清空函数：向清空的话，重新构造一个：q=queue<int>( );

## 优先队列

### 定义

* 头文件 #include <queue>

* 优先队列内元素优先级的设置

* ```c++
  //优先队列的默认设置是数字大的优先级高，char类型则是字典序大的优先级高
  priority_queue<int> q;
  priority_queue<int, vector<int>, less<int> > q;
  //less<int> 表示数字大的优先级大，greater<int> 表示数字小的优先级大
  ```

  

### 元素访问

* 只能访问队首元素：top()
* push() 令x入队
* pop()删除队首元素
* empty()检测优先队列是否为空，如果为空，返回true;
* size() 返回优先队列的元素个数

* **定义优先队列**:通过堆来实现

  ```C++
  priority_queue<Type, Container, Functional>    
  such as :priority_queue<int,vector<int>,greater<int>>pq;//小根堆，从小到大
  //默认是大根堆：从大到小排序
  //第三个参数决定了优先级，这里是从大到小的顺序
  priority_queue<int>q;
  //如何利用大根堆实现从小到大排序？
  // 存x的话，存储-x;
  ```

* 优先队列中没有front() back() 函数：只有top()返回优先队列的最顶层元素

* **例题**：丑数是指不能被2,3,5以外的其他素数整除的数。规定1是最小的丑数。丑数序列：1，2，3，4，5，6，8，9，10，12......求第1500个丑数

  分析：题目不难，但是容易超时：思路是怎么生成丑数：如果x是丑数，那么2x,3x,5x也都是丑数;如果开一个数组，把丑数做标记，空间会爆掉，这里只把丑数存在队列中

  ```C++
  #include <iostream>
  #include <queue>
  #include <vector>
  #include <set>
  using namespace std;
  priority_queue<long long,vector<long long>,greater<long long> >pq;//这个顺序是：越小的数字优先级越高，和sort排序不太一样
  //优先队列也用来存丑数，不是把所有丑数都先算好在存进来，而是动态地只存几个，用一个删一个
  set<long long>s1;//用来存丑数，达到去重
  int a[3]={2,3,5};
  int main(void){
  	pq.push(1);
  	s1.insert(1);
  	for(int i=1;;i++){
  		long long x=pq.top();pq.pop();
          //每循环一次，取出队列顶部的数字，并删除这个数字，
  		if(i==1500){
  			cout<<x;
  			break;
  		}
          
  		for(int j=0;j<3;j++){
  			long long x2=x*a[j];
  			if(!s1.count(x2)){
  				pq.push(x2);
  				s1.insert(x2);
  			}
  		}
  	}
  
  	
  	return 0;
  }
  ```

  

## 6.stack

* 最简单的使用：stack<int>s;
* 元素符合先进后出，后进入的元素放在栈顶
* **push():在堆栈顶部插入新的元素**
* **pop()  删除顶部元素**
* **top()    返回堆栈顶部元素**
* **size( )**
* **empty()  返回值为布尔类型，如果堆栈为空，返回true**

```C++
#include <stack>
#include <iostream>
#include <stack>
#include <cstdio>
using namespace std;
int main(void){
	stack<int>s1;
	s1.push(1);
	s1.push(2);
	cout<<s1.top();
	return 0;
}
输出的结果是2
```

## 7.pair

​		1.**pair是将两个元素绑在一起**:当一个东西具有两种不同的属性，需要按照某一属性排序；

​		2.**定义：**

```
pair<int,int>p;
```

​		3.**包含头文件 #include <utility>**

​		4.**访问：**

```C++
int x=p.second;//访问第二个元素
int y=p.first;//访问第一个元素
```

​		5.**构建一个pair**

```C++
pair<int ,string>p;

//使用自带的pair函数：
p=make_pair("haha",5);
//可以配合queue，将构建的pair压入队列中，q.push(make_pair("haha",5));

//在C++11中可以直接赋值
p={20,"abc"};
```

​		6.**当使用==、！=、<、>比较大小时，比较规则是以first大小作为标准，只有当first大小相等时才去判别second大小**

​		7.用pair存储三个不同的属性

```C++
#include <iostream>
#include <cstdio>
#include <map>
using namespace std;
int main(void){
	pair<string ,pair<int,int> >p;
	pair<int ,int >p1;
	p1.first=1;
	p1.second=2;
	p=make_pair("zhang",p1);
	cout<<p.second.first;
}
```



## 9.string

​	C++把字符串封装成一个类，为了方便操作。

### 9.1string 的定义

* 包含头文件：<string> ; attention ! <string>和<cstring>是不同的头文件

### 9.2string 的访问

* 直接通过下标访问

  * 如果要读入整个字符串只能用cin , cout
  * 如果非要用printf()函数输入，可以借助函数c_str() 将string 变为字符数组类型

* 通过迭代器访问：因为有些函数的参数是迭代器，所以需要学习一下迭代器的知识；只有

  * ```C++
    //string::iterator it;
    
    #include<iostream>
    #include <string> 
    using namespace std;
    int main(void)
    {
    	string str="asdf";
    	for(string :: iterator it = str.begin(); it != str.end() ; it++){
    		printf("%c", *it);
    	}
    	return 0;
    }
    ```

### 9.3string 常用函数解析

* 直接使用 += 操作符：两个字符串可以直接拼接起来

* ==  !=  <    >     <=   >=   ;比较规则是字典序

* length()/size()  返回长度，O（1）

* insert()

  * insert(pos,string)  **pos从0开始的**，是下标，插入字符串，原位置的字符被往后挤了；

  * ```C++
    #include<iostream>
    #include <string> 
    using namespace std;
    int main(void)
    {
    	string str="abcd";
    	string str2="345";
    	
    	str.insert(2,str2);
    	cout<<str;
    	//str=ab345cd	
    }
    ```

  * insert(it , it2 , it3):参数都是迭代器，it是原字符串的插入位置，it2 和 it3是待插入字符的首尾迭代器，注意区间 it2和it3也是左闭右开的

  * ```C++
    #include<iostream>
    #include <string> 
    using namespace std;
    int main(void)
    {
    	string str = "abcd";
    	string str2 = "345";
    	str.insert(str.begin()+2 , str2.begin() , str2.begin()+2);
    	cout << str;
    	//str=ab34cd
    }
    
    ```

* erase() 函数: 时间复杂度是 O( N )

    * erase（it) 删除单个元素，it是**迭代器**
    * erase (first,end) 删除左闭右开区间内的元素 ，参数也是**迭代器**，

* clear() 清除 string 中的所有数据

* **substr()**,比较常用

  * ```C++
    #include<iostream>
    #include <string> 
    using namespace std;
    int main(void)
    {
    	string str = "abcdefgh";
    	//从下标为 1 的位置，截取长度为 3 的字符串 
    	cout << str.substr(1,3) << endl ;//bcd
    	//没有第二个参数，默认剪切到末尾 
    	cout<< str.substr(3);//defgh 
    }
    
    ```

* string::npos 是一个常数，其本身的值为-1，但由于是unsigned_int类型，也可以认为是unsigned_int类型的最大值。所以等于-1或者4294967295；用作find()函数失配时的返回值。

  ```C++
  #include<iostream>
  #include <string>
  using namespace std;
  int main() {
  	if (string::npos == -1) {
  		cout << "string::npos == -1" << endl;
  	}
  	if(string::npos == 4294967295){
  		cout << "string::npos == 4294967295" << endl;
  	}else{
  		cout << "No " << endl;
  	}
  }
  //结果竟然是：
  //	string::npos == -1
  // 	No
  //运行结果和算法笔记说的不太一样
  ```

* find()  O(nm);  

  * str.find(str2): 当str2是str的字串时，返回其在str中第一次出现的位置（下标），如果str2不是str的字串，返回 string::npos；
  * str.find(str2,pos) ：从str   的pos位置开始匹配str2

  ```c++
  #include<iostream>
  #include <string>
  using namespace std;
  int main() {
  	string str = "I want to be proud of myself proud .";
  	string str2 = "proud";
  	string str3 = "off";
  
  	if (str.find(str2) != string::npos) {
  		cout << str.find(str2) << endl;
  	}
  
  	if (str.find(str3) != string::npos) {
  		cout << str.find(str3) << endl;
  	} else {
  		cout << "No position for str3 !" << endl;
  	}
  
  	cout << str.find(str2, 15) << endl;
  
  }
  
  //输出结果
  13
  No position for str3 !
  29
  ```

  

* 支持函数：

    * size( ) /  length( )	返回字符串长度

    * empty( )

    * clear( )

    * substr ( ); 根据两个参数截取字符串的内容，**第二个参数是截取的字符串长度**；当第二个参数超过字符串长度,就截取到末尾；也可以省略掉第二个参数，直接截取到末尾

      ```C++
      string s="wer";
      	s+="w";
      	s+="er";
      	s+='t';
      	cout<<s<<endl;
      	cout<<s.substr(1,2);
      //werwert
      //er
      
      ```

    * c_str(）：如果想用printf( )函数输出string 用到这个函数；得到字符数组的起始地址

      ```C++
      printf("%s\n",s.c_str());
      ```

      

* 支持字符串相加和 加一个字符

   ```C++
   string a="qwe";
   a+="rr";
   a+='u';
   ```

   

​	c_str( ):返回string 对应的字符数组的头指针

## 10.deque

​	双端队列，队头队尾都可以插入，支持随机元素的访问；但是速度比较慢

 * 包含头文件 <deque>
 * 支持函数
    * size( )
    * empty( )
    * clear( )
    * front( )
    * back( )
    * push_back( ) /  push_front( );从队尾/ 队首插入元素
    * pop_back( ) / pop_front( ) 从队尾/ 队首弹出元素

* 支持随机寻址 [ ]
* 用的不多，速度很慢



## 11.bitset

* 定义

   ```C++
   bitset<10000>s;//长度为10000的bitset
   
   ```

   

* 支持操作：~ , & , | ,^ , >> ,<<  == != [ ]

* **count( )**返回有多少个1

* **any( )** 判断是否至少有一个1

* **none( )** 判断是否全为0

* set( )把所有位置成1

* set(k , v)将第K位变成v

* reset( ) 把所有位变成0

* flip( )等价于 ~

  * flip( k )第k位取反

## 12 algorithm 头文件下的常用函数

需要加上using namespace std; 才能使用里面的函数

### 12.1 max() min() abs()

* 注意 abs(x) 返回 x 的绝对值，x只能是整数，浮点型绝对值请用 <cmath> 头文件下的fabs() 

### 12.2 swap(x,y)

* 交换x和y的值

### 12.3 fill()

* 将数组或者容器中某一段区间赋值为某个相同的值。

  ```c++
  int a[5];
  fill(a, a+4, 233);//将a[0] - a[4] 都赋值为233
  ```

  
