## 一、课程概述与学习工具

### 一些补充

[关于cd的一些知识](.\..\补充\关于cd的一些知识.md)

[一些关于make的细节](.\..\补充\一些关于make的细节.md)

### 1.1 欢迎与学习态度

- **从Scratch到C**：第1讲将Scratch中的编程概念迁移到C语言
- **学习挑战**：初期可能感觉"像从消防水管喝水"，但坚持数周数月后必有显著成长
- **基础概念迁移**：函数、条件语句、循环和变量等概念是任何编程语言的基石

### 1.2 开发环境：Visual Studio Code
- **CS50在线IDE**：`cs50.dev`（基于VS Code的云端开发环境）
- **预配置环境**：已安装所有必需软件，避免本地安装的繁琐
- **三区域界面**：
  - 左侧：文件浏览器
  - 中间：文本编辑器
  - 底部：命令行界面（CLI/终端）

### 1.3 源代码与编译器
```
源代码 (C语言) → 编译器 → 机器码 (二进制)
```
- **源代码**：人类可读的指令列表
- **机器码**：计算机理解的二进制模式（0和1）
- **编译器**：将源代码转换为机器码的软件（如gcc）
- **CS50使用**：`make`命令简化编译过程

## 二、第一个C程序：Hello World

### 2.1 创建与运行流程
```bash
# 1. 创建C源文件
code hello.c

# 2. 编写程序
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
}

# 3. 编译程序
make hello

# 4. 运行程序
./hello
```

### 2.2 代码结构详解
```c
#include <stdio.h>      // 引入标准输入输出库头文件
int main(void)          // 主函数，程序入口点
{                       // 代码块开始
    printf("hello, world\n"); // 输出文本，\n为换行转义字符
}                       // 代码块结束
```

### 2.3 转义字符
| 转义字符 | 功能       |
| -------- | ---------- |
| `\n`     | 创建新行   |
| `\r`     | 返回行首   |
| `\"`     | 打印双引号 |
| `\'`     | 打印单引号 |
| `\\`     | 打印反斜杠 |

### 2.4 常见错误
- **缺失分号**：C语句必须以分号结束
- **缺失引号**：字符串必须用双引号包围
- **大小写敏感**：C语言区分大小写

## 三、终端操作指令大全

### 3.1 清屏与导航
| 指令     | 功能                | 效果                   |
| -------- | ------------------- | ---------------------- |
| `clear`  | 完全清屏            | 清空整个终端窗口       |
| `cls`    | 完全清屏（Windows） | 清空整个终端窗口       |
| `Ctrl+l` | 快速清屏            | 屏幕内容上移，保留历史 |

### 3.2 文件与目录操作
#### 查看目录内容
```bash
# Linux/macOS
ls          # 列出文件和目录
ls -l       # 详细列表
ls -a       # 显示所有文件（包括隐藏文件）

# Windows
dir         # 列出文件和目录
```

#### 导航目录结构
```bash
cd folder_name      # 进入folder_name目录
cd ..               # 返回上一级目录
cd ~                # 返回用户家目录
cd -                # 返回最近访问的目录
```

#### 路径表示法
- **`.`**：当前目录（如`./hello`）
- **`..`**：上级目录
- **`~`**：用户家目录
- **`/`**：根目录或路径分隔符

### 3.3 程序编译与运行
```bash
# CS50简化编译
make program_name    # 自动调用gcc并处理依赖

# 标准C编译
gcc -o output_name source.c
gcc -o hello hello.c -lcs50  # 链接CS50库

# 运行程序
./program_name      # 执行当前目录下的程序
```

### 3.4 进程控制与管理
```bash
Ctrl+C              # 强制终止当前程序
Ctrl+Z              # 暂停程序（放入后台）
fg                  # 恢复暂停的程序到前台
ps                  # 显示当前终端进程
```

### 3.5 文件管理
```bash
mkdir folder_name    # 创建新目录
cp source dest       # 复制文件
mv old new           # 移动/重命名文件
rm file_name         # 删除文件
cat file_name        # 显示文件内容
```

## 四、C语言核心概念

### 4.1 数据类型与变量
#### 基本数据类型
| 类型     | 说明           | 示例            | 格式码     |
| -------- | -------------- | --------------- | ---------- |
| `bool`   | 布尔值         | `true`, `false` | 无         |
| `char`   | 字符           | `'A'`, `'0'`    | `%c`       |
| `int`    | 整数           | `42`, `-7`      | `%i`或`%d` |
| `float`  | 单精度浮点数   | `3.14`, `-0.5`  | `%f`       |
| `double` | 双精度浮点数   | `3.1415926535`  | `%lf`      |
| `long`   | 长整数         | `1000000000L`   | `%li`      |
| `string` | 字符串（CS50） | `"Hello"`       | `%s`       |

#### 变量声明与赋值
```c
int counter = 0;          // 声明并初始化
counter = counter + 1;    // 加1
counter += 1;             // 简写加1
counter++;                // 自增
counter--;                // 自减
```

### 4.2 运算符
#### 算术运算符
- `+`：加法
- `-`：减法
- `*`：乘法
- `/`：除法
- `%`：取余

#### 关系运算符
- `<`：小于
- `>`：大于
- `<=`：小于等于
- `>=`：大于等于
- `==`：等于（注意：双等号）
- `!=`：不等于

#### 逻辑运算符
- `&&`：逻辑与
- `||`：逻辑或
- `!`：逻辑非

### 4.3 条件语句
```c
// 基本if语句
if (x < y) {
    printf("x is less than y\n");
}

// if-else语句
if (x < y) {
    printf("x is less than y\n");
} else if (x > y) {
    printf("x is greater than y\n");
} else {
    printf("x is equal to y\n");
}

// 逻辑运算符示例
if (c == 'Y' || c == 'y') {
    printf("Agreed.\n");
}
```

### 4.4 循环结构
#### while循环
```c
int i = 0;
while (i < 3) {
    printf("meow\n");
    i++;
}
```

#### for循环
```c
for (int i = 0; i < 3; i++) {
    printf("meow\n");
}
```

#### do-while循环
```c
int n;
do {
    n = get_int("Number: ");
} while (n < 0);
```

#### 无限循环与中断
```c
// 无限循环
while (true) {
    printf("meow\n");
    // 按Ctrl+C终止
}

// 使用break退出循环
while (true) {
    n = get_int("What's n? ");
    if (n >= 0) {
        break;
    }
}

// 使用continue跳过
while (true) {
    n = get_int("What's n? ");
    if (n < 0) {
        continue;
    } else {
        break;
    }
}
```

### 4.5 函数
#### 函数定义与调用
```c
// 无参数无返回值函数
void meow(void) {
    printf("meow\n");
}

// 带参数函数
void meow(int n) {
    for (int i = 0; i < n; i++) {
        printf("meow\n");
    }
}

// 有返回值函数
int get_positive_int(void) {
    int n;
    do {
        n = get_int("Number: ");
    } while (n < 1);
    return n;
}

// 函数原型声明
void meow(int n);

int main(void) {
    meow(3);
    return 0;
}
```

#### 变量作用域
```c
// 演示作用域
int main(void) {
    int n = 3;      // main函数的n
    meow(n);        // 传递n的副本给meow
}

void meow(int n) {  // meow有自己的n副本
    for (int i = 0; i < n; i++) {
        printf("meow\n");
    }
}
```

### 4.6 输入输出
#### CS50输入函数
```c
#include <cs50.h>

// 获取各种类型输入
char c = get_char("Do you agree? ");
int i = get_int("What's x? ");
float f = get_float("What's x? ");
string s = get_string("What's your name? ");
```

#### 格式化输出
```c
printf("hello, %s\n", answer);     // 字符串
printf("%i\n", x + y);              // 整数
printf("%f\n", (float)x / y);       // 浮点数
printf("%.50f\n", x / y);           // 高精度浮点数
```

## 五、实际编程示例

### 5.1 compare.c（比较整数）
```c
#include <cs50.h>
#include <stdio.h>

int main(void) {
    int x = get_int("What's x? ");
    int y = get_int("What's y? ");
  
    if (x < y) {
        printf("x is less than y\n");
    } else if (x > y) {
        printf("x is greater than y\n");
    } else {
        printf("x is equal to y\n");
    }
}
```

### 5.2 agree.c（同意判断）
```c
#include <cs50.h>
#include <stdio.h>

int main(void) {
    char c = get_char("Do you agree? ");
  
    if (c == 'Y' || c == 'y') {
        printf("Agreed.\n");
    } else {
        printf("Not agreed.\n");
    }
}
```

### 5.3 meow.c（循环示例）
```c
#include <stdio.h>

int main(void) {
    // 初始版本（重复代码）
    printf("meow\n");
    printf("meow\n");
    printf("meow\n");
  
    // 改进版本（while循环）
    int i = 0;
    while (i < 3) {
        printf("meow\n");
        i++;
    }
  
    // 最佳版本（for循环）
    for (int i = 0; i < 3; i++) {
        printf("meow\n");
    }
}
```

### 5.4 Mario方块打印
```c
#include <stdio.h>

// 打印3x3网格
int main(void) {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("#");
        }
        printf("\n");
    }
}

// 使用常量
int main(void) {
    const int n = 3;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("#");
        }
        printf("\n");
    }
}

// 使用辅助函数
void print_row(int width);

int main(void) {
    const int n = 3;
    for (int i = 0; i < n; i++) {
        print_row(n);
    }
}

void print_row(int width) {
    for (int i = 0; i < width; i++) {
        printf("#");
    }
    printf("\n");
}
```

### 5.5 计算器示例
```c
#include <cs50.h>
#include <stdio.h>

int main(void) {
    int x = get_int("What's x? ");
    int y = get_int("What's y? ");
  
    // 加法
    printf("%i + %i = %i\n", x, y, x + y);
  
    // 除法（注意整数除法）
    printf("%i / %i = %i\n", x, y, x / y);  // 整数除法
    printf("%i / %i = %f\n", x, y, (float)x / y);  // 浮点数除法
}
```

## 六、重要概念深入

### 6.1 头文件与库
```c
#include <stdio.h>   // 标准输入输出库
#include <cs50.h>    // CS50专用库
```
- **stdio.h**：标准输入输出函数（如printf）
- **cs50.h**：CS50教学库函数（如get_int, get_string）

### 6.2 整数溢出
```c
// 整数溢出示例
int dollars = 1;
while (true) {
    dollars *= 2;  // 最终会溢出
}
```
- **问题**：当值超过类型最大值时发生回绕
- **解决**：使用`long`类型（更大范围）
- **格式码**：`%li`用于long类型

### 6.3 浮点数精度问题
```c
float x = get_float("What's x? ");
float y = get_float("What's y? ");
printf("%.50f\n", x / y);  // 显示50位小数，观察精度限制
```
- **问题**：二进制表示导致的小数精度限制
- **注意**：浮点数计算可能有微小误差

### 6.4 代码质量三维度
| 维度       | 含义                 | 检查工具   | 目标               |
| ---------- | -------------------- | ---------- | ------------------ |
| **正确性** | 代码是否按预期运行   | `check50`  | 无错误，功能完整   |
| **设计**   | 代码结构是否优雅高效 | `design50` | 模块化，可维护     |
| **风格**   | 代码格式是否一致美观 | `style50`  | 可读性强，遵循约定 |

## 七、学习资源与建议

### 7.1 CS50资源
- **CS50 Manual Pages**：https://manual.cs50.io/
- **在线IDE**：cs50.dev
- **社区支持**：CS50 Discord、Ed讨论板

### 7.2 学习策略
1. **每日练习**：即使短时间，保持每天编码
2. **增量学习**：从简单开始，逐步增加复杂度
3. **调试技巧**：学习阅读错误信息，使用printf调试
4. **代码复用**：创建自己的函数库
5. **社区参与**：提问和帮助他人

### 7.3 常见陷阱与解决方案
| 问题         | 原因                         | 解决方案                               |
| ------------ | ---------------------------- | -------------------------------------- |
| 编译错误     | 语法错误（缺失分号、括号）   | 仔细阅读错误信息，从第一个错误开始修复 |
| 无限循环     | 循环条件永远不会假           | 检查循环条件，确保有退出机制           |
| 整数除法错误 | 期望小数结果但得到整数       | 使用类型转换：(float)x / y             |
| 变量未定义   | 使用未声明的变量             | 在使用前声明变量                       |
| 格式码不匹配 | printf格式码与变量类型不匹配 | 确保格式码对应变量类型                 |

