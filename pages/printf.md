- > 前言这不是教材也不是lecture note，可能会有些错误地方，毕竟是出于兴趣写的。所以请见谅。当然我会尽量写的稍微正确一点。
- > 可能会有人
-
- 学编程的应该都知道有这么个语言叫做C语言，如果你学个那么一开始的每个人的起手式应该是printf一串字符串了。
- 下面是一个最简单的C语言程序， 简单来说你要有一个程序的**入口**起点叫做main，你引入了一个头文件叫做stdio.h，然后在里面调用了一个C语言库函数叫做printf，然后就能愉快的打印了你想要的东西了。Easy Huh?
-
- ```C
  #include <stdio.h>
  
  int main(){
    printf("Hello world!\n");
    return 0;
  }
  ```
- 然后所有的教程都会告诉你，你需要把这个文件或者叫做**源文件**喂给一个叫做**编译器**的东西，然后ta负责把你的源文件进行加工组装，然后生成一个能够在你的系统上运行的终端程序了。
- 比如最简单的，如果你用linux/macos 你只需要使用GNU C Compiler (GNU C 编译器) 然后运行这个指令你就会在当前目录下面找到，在windows上你可以尝试使用msvc或者mingw来进行编译
- ```console
  gcc test.c -o test
  ```
- 然后你在命令行里面输入
- ```console
  ./test
  ```
- 然后你就有了
- ```console
  Hello world!
  ```
- #### 好耶！
- ---
- ### 一些奇怪的思考
- 你的确打出了Hello world，但是不知道你有没有想过为什么这个Hello world能够打出
- ## 自动机101
-
- ## System V ABI 101
-
- ## System Loader 101
-
- ## 文件系统101
-
- ## IO 101
-