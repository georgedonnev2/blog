---
layout: default
---

# VSCode 在 MacOS 上的使用建议

本文给出在 MacOS 上使用 VSCode 的一些建议。

## 设置 workspace

创建一个目录用于测试 C 程序。比如 `~/nfh`

## 编写 C 程序

用 VSCode 编写如下 C 程序。复制以下样例代码，并保存文件

```c
#include <stdio.h>
#include <string.h>

int main() {
    // 1. 定义二维字符数组并初始化
    char countries[3][20] = {"China", "Korea", "England"};
    
    // 打开文件用于读写
    FILE *fp = fopen("dat1.txt", "w+");
    if (fp == NULL) {
        printf("无法打开文件！\n");
        return 1;
    }
    
    // 2. 用fputc将Korea写入文件
    char *korea = countries[1];
    for (int i = 0; korea[i] != '\0'; i++) {
        fputc(korea[i], fp);
    }
    printf("已写入: %s\n", korea);
    
    // 3. 用字符指针逐个字符写入England
    char *england = countries[2];
    char *ptr = england;  // 字符指针
    while (*ptr != '\0') {
        fputc(*ptr, fp);
        ptr++;
    }
    printf("已写入: %s\n", england);
    
    // 4. 重置文件指针并读取一个字符
    rewind(fp);  // 注意：标准函数是rewind，不是frewind
    
    // 读取第一个字符
    int ch = fgetc(fp);  // 读取一个字符
    if (ch != EOF) {
        printf("读取的第一个字符: %c\n", ch);
    } else {
        printf("文件为空或读取错误\n");
    }
    
    // 可选：显示文件全部内容
    printf("\n文件完整内容: ");
    rewind(fp);
    while ((ch = fgetc(fp)) != EOF) {
        putchar(ch);
    }
    printf("\n");
    
    fclose(fp);
    return 0;
}
```