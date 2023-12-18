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

  

