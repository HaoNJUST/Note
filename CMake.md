# CMake_使用

### 1.第一个Cmake样例

* 同一个目录下创建源文件、CMakeLists.txt文件

  ```bash
  touch hello.c
  
  # CMakeLists.txt不可以拼写错误
  touch CMakeLists.txt
  ```

* 编写源文件

  ```c
  # include <stdio.h>
  
  int main(void) {
  	printf("Hello, World\n");
  	return 0;
  }
  
  ```

* 编写CMakeLists.txt

  ```cmake
  # 指定cmake的最低版本
  cmake_minimum_required (VERSION 2.8)
  
  # 指定当前cmake构建的项目名字，可以随便指定
  project(demo)
  
  # 生成一个可执行程序 add_executable(可执行程序名 源文件1 源文件2 ...)，空格或者分号都可以间隔。
  add_executable(hello hello.c)
  
  ```

* 执行CMake命令

  ```bash
  # cmake + CMakeLists.txt所在路径
  
  # .代表当前路径，可以在当前路径下使用tree命令
  cmake .
  ```

  输出：

  -- The C compiler identification is GNU 9.4.0

  -- The CXX compiler identification is GNU 9.4.0
  -- Check for working C compiler: /usr/bin/cc
  -- Check for working C compiler: /usr/bin/cc -- works
  -- Detecting C compiler ABI info
  -- Detecting C compiler ABI info - done
  -- Detecting C compile features
  -- Detecting C compile features - done
  -- Check for working CXX compiler: /usr/bin/c++
  -- Check for working CXX compiler: /usr/bin/c++ -- works
  -- Detecting CXX compiler ABI info
  -- Detecting CXX compiler ABI info - done
  -- Detecting CXX compile features
  -- Detecting CXX compile features - done

  -- Configuring done

  -- Generating done
  -- Build files have been written to: /home/zh/文档/CMake_Demo/demo01

* 查看当前路径下多了哪些文件:关键是生成makefile文件

  ```bash
  ls
  
  # CMakeCache.txt  cmake_install.cmake  hello.c
  # CMakeFiles      CMakeLists.txt       Makefile
  
  ```

* 通过makefile文件生成对用的可执行程序：

  ```bash
  make
  
  # 生成的可执行程序名字就是add_executable(hello hello.c)里面规定的
  ```

  这时目录里会多了一个hello的文件

* 执行可执行程序

  ```bash
  ./hello
  
  # 如果是在别的目录下，就先需要定位到当前目录下面,这个和脚本的执行很相似
  ```

* 可能出现的问题

  在执行cmake . 的时候，可能报错，缺少必要的编译环境。

  ```bash
  sudo apt update
  sudo apt install build-essential
  ```

  介绍一下g++ gcc的作用；

  gcc用来编译c语言程序，g++用来编译c++语言程序。

  ```bash
  gcc -o hello hello.c
  g++ -o hello hello.cpp
  ```

### 2.

##### 	set

* 当源文件比较多，使用set，给所有的文件起一个名字。

  set给变量命名，都是字符串类型；

  ```cmake
  set(变量名 [变量值])
  set(SRC_LIST add.c div.c main.c)
  
  # 使用
  add_executable(app ${SRC_LIST})
  ```

* 指定使用的c++标准：默认是C++98

  将使用的版本写在CMakeLists.txt脚本文件中

  ```cmake
  # 增加-std=c++11
  set(CMAKE_CXX_STANDARD 11)
  # 增加-std=c++14
  set(CMAKE_CXX_STANDARD 14)
  # 增加-std=c++17
  set(CMAKE_CXX_STANDARD 17)
  ```

  在执行cmake命令时，指定出这个宏的值

  ```cmake
  cmake CMakeLists.txt文件路径 -DCMAKE_CXX_STANDARD=11
  
  # -D的意思是指定一个宏,后面跟宏的名称和值。
  ```

  这些指定的标准都会被写到MakeFile文件中，然后根据MakeFile文件构建项目

* 指定生成的可执行程序的路径；也可以给生成的动态库指定路径（用另外一个宏）

  ```cmake
  set(HOME /home/zh/文档)
  set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin)
  
  # 最好使用绝对路径，如果路径不存在，、cmake会自动创建
  ```


### 3.搜索文件

* 指定一个目录，可以自动把该目录下的所有文件都作为源文件

  ```cmake
  # 将目录dir下面的所有文件，保存在一个变量名为variable中  
  aux_source_directory(<dir>, <variable>)
  
  ```

  ```cmake
  cmake_minimum_required(VERSION 3.15)
  projecr(test)
  aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR} SRC)
  add_executable(app ${SRC})
  ```

  

* file命令

  GLOB ：将指定目录下的搜索到的所有文件名生成一个列表，存储到变量名

  GLOB_GLOB_RECURSE：递归搜索制定目录

  PROJECT_SOURCE_DIR:    CMakeLists.txt文件所在路径

  CMAKE_CURRENT_SOURCE_DIR:当前访问的CMakeLists.txt文件所在路径

  

  搜索的路径和文件类型 可以加或者不加双引号；

  ```cmake
  file(GLOB\GLOB_RECURSE 变量名 搜索的路径和文件类型)
  
  # *.h就是把所有头文件都搜索出来，必须指定出找什么后缀的文件
  # file(GLOB MAIN_HEAD ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)
  
  
  ```

  ```cmake
  cmake_minimum_required(VERSION 3.15)
  projecr(test)
  file(GLOB SRC ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp})
  set(EXECUTABLE_OUTPUT_PATH /home/zh/模板)
  add_executable(app ${SRC})
  ```

  

### 4.指定头文件目录

* 实际工程中，头文件和cpp文件不在一个目录下面。

* 头文件：

  将可以被多个.cpp文件使用的函数或方法统一封装在一个文件中，当其他.cpp文件需要使用这个变量或函数时，通过#include将其包含进来就可以。

  引用：系统自带的头文件用尖括号括起来，这样编译器会在系统文件目录下查找。用户自定义的头文件用双引号括起来，编译器首先会在用户目录下查找，然后再到c++安装目录下查找，最后在系统文件中查找。

* 一个例子：

  在目录demo1下面创建src放源文件，include放头文件；

  **src里面分别创建以下文件：**

  add.cpp

  ```c++
  #include <stdio.h>
  #include "head.h"
  int add(int a, int b) {
          return a + b;
  }
  ```

  div.cpp

  ```c++
  #include <stdio.h>
  #include "head.h"
  
  double divide(int a, int b) {
          return (double)(a/b);
  }
  
  ```

  mult.cpp

  ```c++
  #include <stdio.h>
  #include "head.h"
  
  int multiply(int a, int b) {
          return a*b;
  }
  
  ```

  main.cpp:调用编写的函数

  ```c++
  #include <stdio.h>
  #include "head.h"
  
  int main() {
          int a = 20;
          int b = 12;
          printf("a = %d, b = %d\n", a, b);
          printf("a + b = %d\n", add(a,b));
          printf("a * b = %d\n", multiply(a,b));
          printf("a / b = %f\n", divide(a,b));
          return 0;
  }
  
  ```

  **include里面分别创建以下文件：**

  head.h:对函数进行声明

  ```c++
  #ifndef _HEAD_H
  #define _HEAD_H
  
  int add(int a, int b);
  
  double divide(int a, int b);
  
  int multiply(int a, int b);
  
  #endif
  
  ```

  **在include  src的同级目录，编写CMakeLists.txt文件**

  ```cmake
  cmake_minimum_required(VERSION 3.15)
  project(demo01)
  
  # ${CMAKE_CURRENT_SOURCE_DIR}/src/指定了源文件的目录，这句不能复制到CMakeLists.txt文件里面
  
  file(GLOB SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
  set(EXECUTABLE_OUTPUT_PATH /home/zh/模板)
  add_executable(app ${SRC})
  ```

  **再在这里新建一个build目录**，用于构建存放构建项目生成的文件，不和这些文件夹放在一块不便于管理

  所以，demo01下面的目录结构为：

  ```
  .
  ├── build
  ├── CMakeLists.txt
  ├── include
  │   └── head.h
  └── src
      ├── add.cpp
      ├── div.cpp
      ├── main.cpp
      └── mult.cpp
  ```

  进入build目录构建这个项目：

  ```bash
  cd build/
  
  # 注意CMakeLists.txt在这个目录的上一级
  cmake ..
  
  # 可以查看build里面生成了makefile文件
  ls
  
  # 此时如果直接make的话，会找不到头文件
  ```

​		在CMakeLists.txt指定头文件路径：include_directories(${}),，建议使用绝对路径

```cmake
cmake_minimum_required(VERSION 3.15)
project(demo01)
file(GLOB SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
set(EXECUTABLE_OUTPUT_PATH /home/zh/模板)

# project_source_dir也行
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include) 
add_executable(app ${SRC})
```

​	此时在build下重新make,即可生成可执行文件。



