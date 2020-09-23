gcc的编译过程

1. 预处理

    引入头文件, 宏替换, 条件编译, 这时候的文件仍然是一个c文件

    ```bash
    gcc -E main.c -o main.i # 不会检查语法问题
    ```

2. 编译

    将预处理后的文件编译成汇编文件,

    ```bash
    gcc -S main.i -o main.s # 检查语法问题
    ```

3. 汇编

    将汇编文件生成目标文件(二进制)

    ```bash
    gcc -c main.s -o main.o
    ```

4. 链接

    将各个目标文件和启动文件和库文件生成可执行文件

    ```bash
    gcc main.o -o main.exe
    ```

    **main.c -> main.i -> main.s -> main.o**