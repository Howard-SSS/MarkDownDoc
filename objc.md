# 前提

非零或非null假定为true，零或null假定false

```objective-c
#import <Foundation/Foundation.h>
#define WIDTH 10 // 常量10
extern int a; // 声明变量a
int func(); // 声明函数
int main() {
    int a = 1; // 定义变量a
    a = 2u; // unsigned int 2
    a = 3l; // long 3
    const int width = 20 // 常量20
}
// 定义函数
int func() {
    return 0;
}
```

# 基本类型

| 类型             | 字节大小 |
| ---------------- | -------- |
| (unsigned) char  | 1        |
| (unsigned) int   | 2/4      |
| (unsigned) short | 2        |
| (unsigned) long  | 4        |
| float            | 4        |
| double           | 8        |
| long double      | 10       |

# 运算符优先级

| 优先级 | 运算符                                                  | 关联性 |
| ------ | ------------------------------------------------------- | ------ |
| 1      | `()` `[]` `->` `.` `++` `--`                            | 左到右 |
| 2      | `+` `-` `!` `~` `++` `--` `(type)*` `&` `sizeof`        | 右到左 |
| 3      | `*` `/` `%`                                             | 左到右 |
| 4      | `+` `-`                                                 | 左到右 |
| 5      | `<<` `>>`                                               | 左到右 |
| 6      | `<` `<=` `>` `>=`                                       | 左到右 |
| 7      | `==` `!=`                                               | 左到右 |
| 8      | `^`                                                     | 左到右 |
| 9      | `Ι`                                                     | 左到右 |
| 10     | `&&`                                                    | 左到右 |
| 11     | `ΙΙ左到右                                               | 左到右 |
| 12     | `?:`                                                    | 右到左 |
| 13     | `=` `+=` `-=` `*=` `/=` `%=` `>>=` `<<=` `&=` `^=` `Ι=` | 右到左 |
| 14     | `,`                                                     | 左到右 |

# 类/结构体

```objective-c
@interface MyMath: NSObject
	// 方法声明
    - (void) print2;
@end
@implementation MyMath
    - (void) print2 = {
    	NSLog(@"2");
	}
@end
struct Book {
    NSString title;
    int totalPage;
} book1;
MyMath *point1 = [[MyMath alloc]init];
struct Book *point2 = &book1;
```



# 函数/接口/快

```objective-c
@interface MyMath: NSObject
	// 方法声明
    - (int) sum: firstNumber: (int) num1 secondNumber: (int) num2;
@end
@implementation MyMath
    - (int) sum: firstNumber: (int) num1 secondNumber: (int) num2 {
    	return num1 + num2;
	}
@end
// 块声明
double (^multiplyTwoValues)(double, double);
// 块实现
double (^multiplyTwoValues)(double, double) = ^(double firstValue, double secondValue) {
	return firstValue * secondValue
}
```

# 指针

```objective-c
int a = 1; //a存储1 
int *b = &a; // b存储a的地址
int **c = &b; // c存储b的地址
*b // 访问b存储地址所指向的数据
```

