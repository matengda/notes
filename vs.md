error-4996
vs希望用户使用vs提供的库函数, 而不是c标准库函数

```c
#pragma warning(disable: 4996) // 方式1
```

```c
#define _CRT_SECURE_NO_WARNINGS // 方式2
```



Visual Studio 写代码时光标变成方块

问题：Visual Studio 写代码时光标变成方块，此时写的代码会覆盖方块里面的字符

解决方法：按下insert 键

