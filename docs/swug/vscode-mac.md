---
layout: default
title: VSCode 在 MacOS 上的使用建议
---

# VSCode 在 MacOS 上的使用建议

本文给出在 MacOS 上使用 VSCode 的一些建议。

## 安装字体

作为一个程序员，每天的主要工作就是写代码，而代码是由电脑屏幕上文字呈现的。所有，拥有一款美观、清晰的编程字体，可以在提升些许编程效率的同时，带给我们一个美好的心情，进而让我们的代码少出一些 Bug。所以，一个好的编程字体还是挺重要的。[^1]

所以，先安装 Google 的字体 NotoSansMonoCJKsc：
- 下载字体文件：[NotoSansMonoCJKsc](./vscode-mac.assets/13_NotoSansMonoCJKsc.zip)；
- 下载后鼠标（或触控板）双击可解压缩；
- 进入解压缩后生成的目录，鼠标右键 `NotoSansMonoCJKsc-Regular.otf` 和 `NotoSansMonoCJKsc-Bold.otf`，用 `font Book.app` 打开；
- 在 `font Book.app` 界面安装字体。

## VSCode 更改字体

更改 VSCode 编辑器的字体为 `Noto Sans Mono CJK SC`：
- 点击界面左下角的 ![Setting](../assets/icon/settings-gear.svg) `Setting 设置`
- 在 Setting 界面，找到 `Text Editor | Font`
- 在 `Font Family` 中，把 `'Noto Sans Mono CJK SC',` 增加到最前面
- `Font Size`，修改为适合屏幕的大小，比如 `12`、`14`、`16`、`18`等。

## Terminal 更改字体

进入 Terminal | Setting，主要更改：
- Background
- Font

改成白底黑字，或者黑底白字，都是不错的选择。

## Terminal 其他设置

启动一个 `Terminal(终端)`，执行以下指令：
```bash
vim ~/.zshrc
```

在 vim 界面中，先移到最后一行，然后按 `Shift`和`a`键，再按 `回车`，从而在文件最后新增一个空行。

复制以下内容，粘贴到文件中。
```bash
export CLICOLOR=1
export PROMPT='%~ %# '
```
然后再按 `esc` 键（以退出编辑模式），再输入 `:wq`，可保存退出 vim。

再依次执行以下命令：
```bash
source ~/.zshrc
ls
```

可以看到目录的颜色变化了，命令行提示符也改变了。

<!--  -->
## 设置 workspace

创建一个目录用于测试 C 程序。比如 `~/nfh`。

然后在 VSCode 打开该目录，并保存为 workspace：`File` --> `Save Workspace As...`。

可以向 workspace 添加其他目录。

<!-- 下次启动 VSCode 后，可以点左上角 <svg width="16">![explorer](../assets/icon/files.svg)</svg>`Explorer`，打开保存的 workspace，就一下子打开很多目录了。 -->
下次启动 VSCode 后，可以点左上角 <img src="../assets/icon/files.svg" width="16"> **Explorer**，打开保存的 workspace，就一下子打开很多目录了。

## 编写 并运行 C 程序

样例程序如下。比如在 VSCode中，在目录 `~/nfh` 鼠标右键，选择 **New File ...**，然后输入文件名 `c1.c`。
```c
# include <stdio.h>
# include <string.h>

void main() {
    char *p, st[3][20] = {"China", "Korea", "England"}, cstr[20]; FILE *fp;

    fp = fopen("d1.dat", "w+");
    fprintf(fp, "%s\n", st[1]);
    fputs(st[0], fp);

    p = st[2];
    while((*p) != '\0') fputc(*p++, fp);

    rewind(fp);
    fscanf(fp, "%s", cstr);
    printf("%s\n", cstr);

    strcpy(cstr, "");
    fgetc(fp);
    fgets(cstr, 6 ,fp);
    printf("%s\n", cstr);

    return;
}
```

然后启动一个 **Terminal(终端)**，依次执行以下命令：
```bash
# 切换到 C 程序所在目录
~ % cd nfh

# 编译 C 程序，生成可执行的程序 c1
~/nfh % gcc -o c1 c1.c

# 执行 c1 
~/nfh % ./c1
Korea
China
```

还可以请大模型注释说明，并增加一些代码，以加深理解。
```c
# include <stdio.h>  // 包含标准输入输出函数声明
# include <string.h> // 包含字符串操作函数声明

int main() {  // 主函数，应使用int main()以避免编译器警告
    char *p, st[3][20] = {"China", "Korea", "England"}, cstr[20]; FILE *fp; 
    // 定义：字符指针p，二维字符数组st（包含3个字符串，每个最多19字符+空字符），字符数组cstr（最多19字符+空字符），文件指针fp
    
    char ch0, ch1 = 'A';

    fp = fopen("d1.dat", "w+"); // 以读写模式打开文件d1.dat（若存在则清空内容，不存在则创建）
    fprintf(fp, "%s\n", st[1]); // 将st[1]（"Korea"）写入文件，末尾添加换行符\n
    fputs(st[0], fp); // 将st[0]（"China"）写入文件，不添加换行符

    p = st[2]; // 将指针p指向st[2]（"England"）的首字符'E'
    while((*p) != '\0') fputc(*p++, fp); // 循环将"England"的每个字符逐个写入文件，直到遇到字符串结束符\0

    // 显示文件内容
    printf("\n=== d1.dat 完整内容 ===\n");
    rewind(fp);  // 重置文件指针到开头
    while((ch0 = fgetc(fp)) != EOF) {
        putchar(ch0);
    }
    printf("=====================\n\n");

    rewind(fp); // 将文件指针重置到文件开头，以便后续读取
    fscanf(fp, "%s", cstr); // 从文件读取一个字符串（遇空白字符停止）到cstr，此时读取的是"Korea"
    printf("==第一行== [%s]\n", cstr); // 打印cstr（"Korea"）并换行
    // strcpy(cstr, ""); // 将cstr清空（复制空字符串使其变为""）

    printf("ch1 的初始值 = [%c]\n", ch1);
    ch1 = fgetc(fp); // 从文件读取一个字符但不保存，此处读取的是"Korea"后的换行符\n，使文件指针后移
    printf("从文件中取了一个字符后的 ch1 值 =[%c]\n", ch1);
    // fgetc(fp); // 从文件读取一个字符但不保存，此处读取的是"Korea"后的换行符\n，使文件指针后移
    fgets(cstr, 6 ,fp); // 从文件读取最多5个字符（留1位给\0）到cstr，此处读取"China"的前5个字符
    printf("=== 2nd line === [%s]\n", cstr); // 打印cstr（"China"）并换行

    return 0; 
}
```

<!--  -->
## 提升效率的几个方式

**1、提升打字水平。可以达到盲打程度，即眼睛看屏幕不看键盘。**

**2、左右分屏**

- 左边是 **VSCode**
- 右边是 **Terminal(终端)**

或者上述 2 个应用全屏，用触控板手势切换，也很方便。

**3、VSCode 一些快捷键组合**

- `command` + `/`：注释掉代码，或恢复被注释掉的代码
- `command` + `shift` + `k`：删除行
- `option` + `shift` + `上箭头/下箭头`：复制代码到当前行上面或下面

## 结语

Mac + VSCode + C，好玩！

## 参考资料

[^1]: [Noto Sans Mono: 最佳编程字体](https://zhuanlan.zhihu.com/p/560792115), 知乎，码农贾维斯，2022-09-05