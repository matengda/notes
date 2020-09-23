error-4996
vs希望用户使用vs提供的库函数, 而不是c标准库函数

```c
#pragma warning(disable: 4996) // 方式1
```

```c
#define _CRT_SECURE_NO_WARNINGS // 方式2
```