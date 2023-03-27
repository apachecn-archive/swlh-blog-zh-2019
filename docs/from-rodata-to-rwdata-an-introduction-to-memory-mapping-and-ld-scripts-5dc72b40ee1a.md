# 从。罗达托。rw data——内存映射和 LD 脚本简介

> 原文：<https://medium.com/swlh/from-rodata-to-rwdata-an-introduction-to-memory-mapping-and-ld-scripts-5dc72b40ee1a>

前几天我的一个同事，刚开始学 C，对下面这段代码很疑惑:

```
char *foo = "AAAAAA";
foo[0] = 'B';
```

根据他所遵循的教程，这被描述为有效的代码，然而当运行它时，出现了分段错误:

```
guy@localhost ~/b/string_elf> gcc sample_1.c
guy@localhost ~/b/string_elf> ./a.out fish: "./a.out" terminated by signal SIGSEGV (Address boundary error)
```