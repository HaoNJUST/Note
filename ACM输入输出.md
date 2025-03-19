# ACM输入输出

1.输入若干行数字，但不知道每行具体有几个，多少行；

将一行输入转为字符串，再从字符串中提取；主要头文件 `<sstream>`，`getline(cin, str)`

```c++
#include <iostream>
#include <sstream>
#include <vector>

using namespace std;
int main(void) {
    string str;
    while (getline(cin, str)) {
		vector<int> vec;
        int num;
        istringstream iss(str);
        while(iss >> num) {
			vec.push_back(num);
        }
        for (int i = 0; i < vec.size(); i++) {
			cout << vec[i] << ' ';
        }
        cout << endl;
    }
    
    return 0;
}
```

2.输入一行字符串，单词用逗号或者空格隔开，为了处理，通常需要转为字符数组

```c++
#include <iostream>
#include <sstream>
using namespace std;
int main(void) {
    string str;
    while (getline(cin, str)) {
        vector<string> vec;
        istringstream iss(str);
        while (iss >> s) {
            vec.push_back(s);
        }
	}
	return 0;
}
```

如果输入的字符串，单词是以逗号隔开，那就先把逗号换成空格

```c++
#include <iostream>
#include <sstream>
#include <vector>

using namespace std;
int main(void) {
    string str;
    while (getline(cin, str)) {
        //逗号变空格
        for (int i = 0; i < str.size(); i++) {
			if (str[i] == ',') {
				str[i] = ' ';
			}
		} 
		vector<string> vec;
        string s;
        istringstream iss(str);
        while(iss >> s) {
			vec.push_back(s);
        }
        for (int i = 0; i < vec.size(); i++) {
			cout << vec[i] << ' ';
        }
        cout << endl;
    }
    return 0;
}
```

3.如果先输入一个整数，再用`getline`接受一行输入，需要用getchar()吸收一个换行符

```C++
/*
输入格式：
输入的第一行是一个整数n，表示一共有n组测试数据。（输入只有一个n，没有多组n的输入）
接下来有n行，每组测试数据占一行，每行有一个词组，每个词组由一个或多个单词组成；每组的单词个数不超过10个，每个单词有一个或多个大写或小写字母组成；
单词长度不超过10，由一个或多个空格分隔这些单词。

EG： 
1
ad dfa     fgs

*/

#include <iostream>
#include <sstream>
#include <vector>
using namespace std;
int main(void) {
    int n;
    cin >> n;
    getchar();
    while (n--) {
        string str;
        getline(cin, str);
        istringstream iss(str);
        string s;
        vector<string> vec;
        while (iss >> s) {
            vec.push_back(s);
        }
        //j
    }
    return 0;
}
```

