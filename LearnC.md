# C語言基礎
## Data Types

| 資料型態 | Size (Byte) | 格式 |
| --- | --- | --- |
| (unsigned)short | 2 | %d |
| (unsigned)int | 4 | %d |
| (unsigned)long | 4 (32bit) or 8 (64bit) | %d |
| (unsigned)long long | 8 | %d |
| float | 4 | %f |
| double | 8 | %f |
| long double | 12 | %f |
| char | 1 | %c |
| (unsigned)char | 1 | %c |
| string | 1 * stringSize | %s |

==unsigned 資料型態格式為%u==

![pic](https://hackmd.io/_uploads/Syly6QyaR.png)

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    // printf("%d\n", sizeof(unsigned long)); 會導致警告，
    // 因为 sizeof 回傳的是 size_t 類別，而 %d 是用於格式化 int 類別的格式符。
    printf("%zu\n", sizeof(unsigned long));
    return 0;
}

//output
8
Program ended with exit code: 0
```
>`size_t` is an unsigned data type，對於 32-bit 的系統而言，size_t 的大小為 4 bytes，而 64-bit 的系統則為 8 bytes。(%zu is used for size_t values)
```c
跳脫字元：
\n: 換行
\r: 將游標移至目前所在行的起始位置
\t: 水平TAB
\a: 警告。讓系統發出警告聲
\\: 在字串中顯示反斜線
\': 在字串中顯示單引號
\": 在字串中顯示雙引號
\?: 在字串中顯示問號
```

## 型別轉換

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    // short -> int
    short s_1 = 10;
    int i_1 = (int)s_1;
    printf("s_1 size: %zu Bytes\ni_1 size: %zu Bytes\n", sizeof(s_1), sizeof(i_1));
    
    //int -> short
    int i_2 = 10;
    short s_2 = (short)i_2;
    printf("\ni_2 size: %zu Bytes\ns_2 size: %zu Bytes\n", sizeof(i_2), sizeof(s_2));
    
    //int -> float
    int i_3 = 10;
    float f_1 = (float)i_3;
    printf("\nf_1 = %f\n", f_1);
    
    //float -> int
    float f_2 = 10.5;
    int i_4 = (int)f_2;
    printf("i_4 = %d\n", i_4);
    
    //overflow
    const int i_5 = 32768;
    short s_3 = (short)i_5;
    printf("\n%d overflow occur! short range is -32768 ~ 32767\n", s_3);
    int i_6 = -15;
    unsigned short us_4 = (unsigned short)i_6;
    printf("%u overflow occur! unsigned short range is 0 ~ 65535\n", us_4);
    double d_1 = 3.5e38; // d_1 = 3.5*10^38
    float f_3 = (float)d_1;
    printf("%f overflow occur! float range is -3.4e38 ~ 3.4e38\n\n", f_3); //inf: 無窮大
}
```
```
//output
s_1 size: 2 Bytes
i_1 size: 4 Bytes

i_2 size: 4 Bytes
s_2 size: 2 Bytes

f_1 = 10.000000
i_4 = 10

-32768 overflow occur! short range is -32768 ~ 32767
65521 overflow occur! unsigned short range is 0 ~ 65535
inf overflow occur! float range is -3.4e38 ~ 3.4e38

Program ended with exit code: 0
```
# 陣列

## Array

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    //一維陣列
    int i_arr[] = {1, 2, 3, 4, 5};
    for (int i = 0; i < 5; i++) {
        printf("%d ", i_arr[i]);
    }
    printf("\n");
    
    int i_arr2[5];
    i_arr2[0] = 6;
    i_arr2[1] = 7;
    i_arr2[2] = 8;
    i_arr2[3] = 9;
    i_arr2[4] = 10;
    
    for (int i = 0; i < 5; i++) {
        printf("%d ", i_arr2[i]);
    }
    printf("\n");
    
    printf("\n");
    //二維陣列，行size一定要寫
    float f_arr[][3] = { // f_arr[列][行]
        {1.1, 1.2, 1.3},
        {2.1, 2.2, 2.3},
        {3.1, 3.2, 3.3}
    };
    
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%f ", f_arr[i][j]);
        }
        printf("\n");
    }
    
    printf("\n");
}
```
```
//output
1 2 3 4 5 
6 7 8 9 10 

1.100000 1.200000 1.300000 
2.100000 2.200000 2.300000 
3.100000 3.200000 3.300000 

Program ended with exit code: 0
```
## String : 字元陣列

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    char str[] = "Hello World!";
    // str 實際上是由13個字元組成的字串 -> H,e,l,l,o, ,W,o,r,l,d,!,\0
    printf("%zu\n", sizeof(str));
    printf("%s\n", str);
    
    char str2[][4] = {
        "cat", // c, a, t, \0
        "dog", // d, o, g, \0
        "pig" // p, i, g, \0
    };
    printf("%s %s %s\n", str2[0], str2[1], str2[2]);
}

//output
13
Hello World!
cat dog pig
Program ended with exit code: 0
```

# 輸入輸出

## scanf & printf

### scanf 的運作邏輯: scanf(”%d%f%c”, &input1, &input2, &input3)

會將輸入的內容以string的格式儲存在緩衝記憶體 (Buffer) 中，在依對應的轉換詞(eg. %d, %f, %c, etc.)，存入指定的記憶體位置。

### printf 的運作邏輯: printf(”%d”, input1)

會將 %d 替換成 input 的內容，再以 string 的格式存入緩衝記憶體 (Buffer) 中，並打印出來。也就是說打印出來的內容皆是string的型態。

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int input;
    scanf("%d", &input); // 輸入並指派給 input
    printf("output = %d\n", input); // 輸出
    
    int input2, input3, input4;
    scanf("%d%d%d", &input2, &input3, &input4);
    printf("output = %d %d %d\n", input2, input3, input4);
    
    // 輸入/輸出字串
    char input5[10];
    scanf("%s", input5); // 不需加&，另外，輸入空白鍵會被當作是\0
    printf("output = %s\n", input5);
    
    //print integer.
    int a = 100;
    printf("%d\n", a);
    
    // print long integer.
    long b = 100;
    printf("%ld\n", b);
    
    // print %md, m -> 含 a 本身共 m 格，a 前補 space。
    printf("%20d\n", a);
    
    // print float number.
    float c = 3.141592;
    printf("%f\n", c);
    
    // print %mf, m -> Round float number to m place.
    printf("%.4f\n", c);
}
```
```
//output
10
output = 10
11 12 13
output = 11 12 13
Hello World
output = Hello
100
100
                 100
3.141592
3.1416
Program ended with exit code: 0
```
## sscanf

### sscanf 的運作邏輯: scanf(自定義的儲存空間<string>, ”%d%f%c”, &input1, &input2, &input3)

自行宣告字串來取代掉 scanf 使用的 buffer，也就不用在程式執行時才輸入input。

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int input1;
    float input2;
    char input3[4];
    char input[20] = "10 3.14 abc";
    sscanf(input, "%d%f%s", &input1, &input2, input3);
    printf("%d, %f, %s\n", input1, input2, input3);
}

//output
10, 3.140000, abc
Program ended with exit code: 0
```

## sprintf

### sprintf 的運作邏輯: sprintf(自定義的儲存空間<string>, ”%d%f%c”, &input1, &input2, &input3)

自行宣告字串來取代掉 printf 使用的 buffer，將要列印出的內容存入自行宣告的字串中。且不會列印出來。

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    float f_1 = 3.14;
    char output[20];
    sprintf(output, "out = %f", f_1);
    printf("%s\n", output); //印出output字串的內容
}

//output
out = 3.140000
Program ended with exit code: 0
```

## gets

s → string (已不再使用，因為gets沒有辦法限制輸入的長度，這可能導致緩衝區溢出，從而引發安全漏洞。)

### gets 與 scanf 的功能類似，但是gets是可以輸入空格的!!

## fgets

char *fgets(char *str, int n, FILE *stream)

### fgets 功能與 gets 相似，皆可輸入空格，差別在 fgets 有限制輸入 n 個字數的機制（較安全），且在輸入完按下 Enter 後會多一個 ‘\n’ 字元。

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    printf("輸入字串：");
    char str[10];
    fgets(str, 10, stdin); //stdin: 從鍵盤輸入
    printf("輸出字串：%s\n", str);
    printf("%c, %d, %d\n", str[6], str[7], str[8]); 
    // str[7] 會輸出 10 對應到 ASCII table 為換行鍵，即 Enter 鍵
    // 所以 str: a, b, c,  , d, e, f, \n, \0
}

//output
輸入字串：abc def
輸出字串：abc def

f, 10, 0
Program ended with exit code: 0
```

## puts

### puts 與 printf 的功能類似，差別在於puts會自動換行，即printf(”\n”)的效果。

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    char str1[] = "I love coding.";
    char str2[] = "And you?";
    printf("printf的效果：\n");
    printf("%s", str1);
    printf("%s", str2);
    
    printf("\nputs的效果：\n");
    puts(str1);
    puts(str2);
}

\\output 
printf的效果：
I love coding.And you?
puts的效果：
I love coding.
And you?
Program ended with exit code: 0
```

## fputs

int fputs(const char *str, FILE *stream)

### fputs 功能與 puts 相似，差別在 fputs 不自帶換行。

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    char str1[] = "I love coding.";
    char str2[] = "And you?";
    fputs(str1, stdout); // stdout: 印到螢幕上
    fputs(str2, stdout);
    printf("\n----------------------\n");
    puts(str1);
    puts(str2);
}
```

```
//output
I love coding.And you?
----------------------
I love coding.
And you?
Program ended with exit code: 0
```

## getc / putc

c → char

int getc(FILE *stream) / int putc(int char, FILE *stream)

### getc()只接受一個字元，

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    char c;
    c = getc(stdin);
    putc(c, stdout); // 印出的是字元
    printf("\n字元 %c 對應到ASCII code: %d\n", c, c);
}

```

```
//output
5
5
字元 5 對應到ASCII code: 53
Program ended with exit code: 0
```

## getchar() / putchar()

getchar() = int getc(stdin) / putchar(int char) = int putc(int char, stdout)

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    char c;
    // c = getc(stdin);
    c = getchar();
    // putc(c, stdout);
    putchar(c);
    printf("\n字元 %c 對應到ASCII code: %d\n", c, c);
}

```

```
//output
5
5
字元 5 對應到ASCII code: 53
Program ended with exit code: 0
```

# 數學/邏輯運算式
    ＋ , － , * , / , % , & , | , ^ , ～ , << , >>

注意：浮點數不能取餘數 (%)

bit操作術語：pull hight: 0 → 1 / pull down: 1 → 0
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    unsigned short hex = 0x55AA;
    // 將 hex 的 bit_9 pull hight
    unsigned short bit_9 = 0x01 << 9;
    hex = hex | bit_9;
    printf("result = %x\n", hex); // 57aa
    
    // 再將 hex 還原成 55AA ，及對 hex 的 bit_9 pull down
    hex = hex & ~bit_9;
    printf("result = %x\n", hex); // 55aa
}

//output
result = 57aa
result = 55aa
Program ended with exit code: 0
```
# 條件運算式
    >, >=, <, <=, ==, !=, !, &&, ||
## 布林值
```c
#include <stdbool.h>
```
| state |value|
| - | - |
| True |1|
| False|0|s
## 判斷式
### if else
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int age;
    scanf("%d", &age);
    if (age <= 0 || age > 100) {
        printf("請輸入正確的年齡\n");
    }
    else if (age < 13) {
        printf("兒童票\n");
    }
    else if (age >= 13 && age < 65) {
        printf("成人票\n");
    }
    else {
        printf("敬老票\n");
    }
    return 0;
}

```
### switch case
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int age;
    scanf("%d", &age);
    switch (age) {
        case -10000 ... 0:
        case 101 ... 9999:
            printf("請輸入正確的年齡\n");
            break; // 一般情況不可省略
        case 1 ... 12: // 1 ~ 12
            printf("兒童票\n");
            break; // 一般情況不可省略
        case 13 ... 64: // 13 ~ 64
            printf("成人票\n");
            break; // 一般情況不可省略
        default: // other
            printf("敬老票\n");
            break; // 一般情況可省略，因為程式在進入到default時，也是準備要結束switch case
    }
    return 0;
}
```
# Loop
## while
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {   
    // 印九九乘法表
    int i = 1;
    int j = 1;
    while (i <= 9) {
        while (j <= 9) {
            printf("%d * %d = %2d    ", j, i, j * i);
            j++;
        }
        j = 1;
        i++;
        printf("\n");
    }
    
    return 0;
}

//output
1 * 1 =  1    2 * 1 =  2    3 * 1 =  3    4 * 1 =  4    5 * 1 =  5    6 * 1 =  6    7 * 1 =  7    8 * 1 =  8    9 * 1 =  9    
1 * 2 =  2    2 * 2 =  4    3 * 2 =  6    4 * 2 =  8    5 * 2 = 10    6 * 2 = 12    7 * 2 = 14    8 * 2 = 16    9 * 2 = 18    
1 * 3 =  3    2 * 3 =  6    3 * 3 =  9    4 * 3 = 12    5 * 3 = 15    6 * 3 = 18    7 * 3 = 21    8 * 3 = 24    9 * 3 = 27    
1 * 4 =  4    2 * 4 =  8    3 * 4 = 12    4 * 4 = 16    5 * 4 = 20    6 * 4 = 24    7 * 4 = 28    8 * 4 = 32    9 * 4 = 36    
1 * 5 =  5    2 * 5 = 10    3 * 5 = 15    4 * 5 = 20    5 * 5 = 25    6 * 5 = 30    7 * 5 = 35    8 * 5 = 40    9 * 5 = 45    
1 * 6 =  6    2 * 6 = 12    3 * 6 = 18    4 * 6 = 24    5 * 6 = 30    6 * 6 = 36    7 * 6 = 42    8 * 6 = 48    9 * 6 = 54    
1 * 7 =  7    2 * 7 = 14    3 * 7 = 21    4 * 7 = 28    5 * 7 = 35    6 * 7 = 42    7 * 7 = 49    8 * 7 = 56    9 * 7 = 63    
1 * 8 =  8    2 * 8 = 16    3 * 8 = 24    4 * 8 = 32    5 * 8 = 40    6 * 8 = 48    7 * 8 = 56    8 * 8 = 64    9 * 8 = 72    
1 * 9 =  9    2 * 9 = 18    3 * 9 = 27    4 * 9 = 36    5 * 9 = 45    6 * 9 = 54    7 * 9 = 63    8 * 9 = 72    9 * 9 = 81    
Program ended with exit code: 0
```
```c
// 無窮迴圈寫法：
while(1) {
    printf("in loop\n");
}
```
## for
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    // 印九九乘法表
    for (int i = 1; i <= 9; i++) {
        for (int j = 1; j <= 9; j++) {
            printf("%d * %d = %2d    ", j, i, j*i);
        }
        printf("\n");
    }

    return 0;
}
//output
1 * 1 =  1    2 * 1 =  2    3 * 1 =  3    4 * 1 =  4    5 * 1 =  5    6 * 1 =  6    7 * 1 =  7    8 * 1 =  8    9 * 1 =  9    
1 * 2 =  2    2 * 2 =  4    3 * 2 =  6    4 * 2 =  8    5 * 2 = 10    6 * 2 = 12    7 * 2 = 14    8 * 2 = 16    9 * 2 = 18    
1 * 3 =  3    2 * 3 =  6    3 * 3 =  9    4 * 3 = 12    5 * 3 = 15    6 * 3 = 18    7 * 3 = 21    8 * 3 = 24    9 * 3 = 27    
1 * 4 =  4    2 * 4 =  8    3 * 4 = 12    4 * 4 = 16    5 * 4 = 20    6 * 4 = 24    7 * 4 = 28    8 * 4 = 32    9 * 4 = 36    
1 * 5 =  5    2 * 5 = 10    3 * 5 = 15    4 * 5 = 20    5 * 5 = 25    6 * 5 = 30    7 * 5 = 35    8 * 5 = 40    9 * 5 = 45    
1 * 6 =  6    2 * 6 = 12    3 * 6 = 18    4 * 6 = 24    5 * 6 = 30    6 * 6 = 36    7 * 6 = 42    8 * 6 = 48    9 * 6 = 54    
1 * 7 =  7    2 * 7 = 14    3 * 7 = 21    4 * 7 = 28    5 * 7 = 35    6 * 7 = 42    7 * 7 = 49    8 * 7 = 56    9 * 7 = 63    
1 * 8 =  8    2 * 8 = 16    3 * 8 = 24    4 * 8 = 32    5 * 8 = 40    6 * 8 = 48    7 * 8 = 56    8 * 8 = 64    9 * 8 = 72    
1 * 9 =  9    2 * 9 = 18    3 * 9 = 27    4 * 9 = 36    5 * 9 = 45    6 * 9 = 54    7 * 9 = 63    8 * 9 = 72    9 * 9 = 81    
Program ended with exit code: 0
```
```c
// 無窮迴圈寫法：
for(;;) {
    printf("in loop\n");
}
```
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    // for迴圈特殊寫法：
    int i, j, k;
    for (i = 0, j = 0, k = 0; k <= 5; i++, j++, k++) {
        printf("%2d %2d %2d\n", i, j, k);
    }
    return 0;
}
                                    
//output
 0  0  0
 1  1  1
 2  2  2
 3  3  3
 4  4  4
 5  5  5
Program ended with exit code: 0
```
## pooling array
### 1D Array
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int int_array[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    for (int i = 0; i < sizeof(int_array) / sizeof(int); i++) {
        printf("%d, ", int_array[i]);
    }
    printf("\n");
    return 0;
}

// output
1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 
Program ended with exit code: 0
```
```c
#include <stdio.h>
#include <string.h>

int main(int argc, const char * argv[]) {
    char str[] = "Hello World!";
    printf("str的長度(含 \\0)：%zu\n", sizeof(str) / sizeof(char));
    printf("str的長度(不含 \\0)：%zu\n", strlen(str));
    // 法一
    for (int i = 0; i < strlen(str); i++) {
        printf("%c, ", str[i]);
    }
    printf("\n");
    
    // 法二
    for (int i = 0; str[i] != 0; i++) { // 因為字串最後一個字元都是0
        printf("%c, ", str[i]);
    }
    printf("\n");
    
    return 0;
}


//output
str的長度(含 \0)：13
str的長度(不含 \0)：12
H, e, l, l, o,  , W, o, r, l, d, !, 
H, e, l, l, o,  , W, o, r, l, d, !, 
Program ended with exit code: 0
```
### 2D Array
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int int_2Darray[][10] = {
        {1, 2, 3, 4, 5, 6, 7, 8, 9, 10},
        {11, 12, 13, 14, 15, 16, 17, 18, 19, 20},
        {21, 22, 23, 24, 25, 26, 27, 28, 29, 30},
    };
    
    int col_size = sizeof(int_2Darray[0]) / sizeof(int);
    int row_size = (sizeof(int_2Darray) / sizeof(int)) / col_size;
    printf("col_size = %d, row_size = %d\n", col_size, row_size);
    
    for (int i = 0; i < row_size; i++) {
        for (int j = 0; j < col_size; j++) {
            printf("%2d, ", int_2Darray[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}

//output
col_size = 10, row_size = 3
 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 
11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 
21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 
Program ended with exit code: 0
```
```c
#include <stdio.h>
#include <string.h>

int main(int argc, const char * argv[]) {
    char str2D[][11] = {
        "JIMMY HSU",
        "JEREMY LIN",
        "WEBBER SU",
    };
    
    int col_size = sizeof(str2D[0]) / sizeof(char);
    int row_size = sizeof(str2D) / sizeof(char) / col_size;
    printf("col_size = %d, row_size = %d\n", col_size, row_size);
    
    // 法一：多印了空格
    for (int i = 0; i < row_size; i++) {
            for (int j = 0; j < col_size; j++) {
                printf("%c, ", str2D[i][j]);
            }
            printf("\n");
    }
    
    printf("--------------------------------\n");
    
    // 法二
    for (int i = 0; i < row_size; i++) {
        for (int j = 0; j < strlen(str2D[i]); j++) {
            printf("%c, ", str2D[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}

//output
col_size = 11, row_size = 3
J, I, M, M, Y,  , H, S, U, , , 
J, E, R, E, M, Y,  , L, I, N, , 
W, E, B, B, E, R,  , S, U, , , 
--------------------------------
J, I, M, M, Y,  , H, S, U, 
J, E, R, E, M, Y,  , L, I, N, 
W, E, B, B, E, R,  , S, U, 
Program ended with exit code: 0
```
## do while
## goto
```c
#include <stdio.h>
#include <string.h>

int main(int argc, const char * argv[]) {
    int a = 1, b = 2;
    if (a > b) goto point1;
    else goto point2;
    
point1:
    printf("point1:a > b\n");
    return 0;

point2:
    printf("point2:a <= b\n");
    return 0;
}

//output:
point2:a <= b
Program ended with exit code: 0
```
### goto 迴圈應用
```c
#include <stdio.h>
#include <string.h>

int main(int argc, const char * argv[]) {
    int i = 0;
LOOP:
    printf("%d\n", i);
    if (i < 5) {
        i++;
        goto LOOP;
    }
}

//output
0
1
2
3
4
5
Program ended with exit code: 0
```
# Function
```c
#include <stdio.h>
#include <string.h>

void sayHi(char object[][50], int size) {
    for (int i = 0; i < size; i++) {
        printf("Hi, %s.I'm your dad!\n", object[i]);
    }
}

int main(int argc, const char * argv[]) {
    char friends[][50] = {
        "Jimmy Xu",
        "Jeremy Lin",
        "Webber Su",
        "Jack Liu"
    };
    
    int arr_size = sizeof(friends) / sizeof(char) / (sizeof(friends[0]) / sizeof(char));
    
    sayHi(friends, arr_size);
    return 0;
}

//output
Hi, Jimmy Xu.I'm your dad!
Hi, Jeremy Lin.I'm your dad!
Hi, Webber Su.I'm your dad!
Hi, Jack Liu.I'm your dad!
Program ended with exit code: 0
```
```c
#include <stdio.h>
#include <stdbool.h>

bool isBigger(float a, float b){
    return a > b;
}

int main(int argc, const char * argv[]) {
    printf("%d\n", isBigger(10.5, 20));
    return 0;
}

//output
0
Program ended with exit code: 0
```
# Library (header檔)
>#include <...> 用於新增系統目錄下的header檔，而#include "..."用於新增檔案目錄底下的header檔(自創的library)。
## 創建標頭檔和函式庫
![截圖 2024-09-11 上午10.59.03](https://hackmd.io/_uploads/Bkttwt02C.png)
main.c (使用library中的function)
```c
#include <stdio.h>
#include "myLibrary.h"

int main(int argc, const char * argv[]) {
    char friends[][50] = {
        "Jimmy Xu",
        "Jeremy Lin",
        "Webber Su",
        "Jack Liu"
    };
    
    int arr_size = sizeof(friends) / sizeof(char) / (sizeof(friends[0]) / sizeof(char));
    sayHi(friends, arr_size);
    
    printf("%d\n", isBigger(10.5, 20));
}

//output
Hi, Jimmy Xu.I'm your dad!
Hi, Jeremy Lin.I'm your dad!
Hi, Webber Su.I'm your dad!
Hi, Jack Liu.I'm your dad!
0
Program ended with exit code: 0
```
myLibrary.h (負責宣告有哪些function)
```c
#ifndef myLibrary_h
#define myLibrary_h
#include <stdbool.h>

void sayHi(char object[][50], int size);
bool isBigger(float a, float b);

#endif /* myLibrary_h */
```
myLibraby.c (負責實作myLibrary.h中的function)
```c
#include <stdio.h>
#include "myLibrary.h"

void sayHi(char object[][50], int size) {
    for (int i = 0; i < size; i++) {
        printf("Hi, %s.I'm your dad!\n", object[i]);
    }
}

bool isBigger(float a, float b){
    return a > b;
}
```
## 靜態/動態函式庫
靜態函式庫(static library)
: 把Library包成一個檔案，檔案容量大。

動態函式庫(dynamic library)
: 把Library包成額外的檔案，執行時與執行檔一起執行，較省空間。

![picture1](https://hackmd.io/_uploads/rkWT_ATn0.jpg)


# 巨集(Macro) #define
:::warning
注意：巨集不是變數，在執行期間不會被改變。巨集在程式編譯之前就會被替換。其實就是替身。
:::

```c
#include <stdio.h>

#define MAX_SIZE 10
#define ADD(x) (x + 1)
#define SUB_without_brackets(a, b) a - b // 括號陷阱
#define MAX(a, b) ((a) > (b) ? (a) : (b))

int main(int argc, const char * argv[]) {
    char students[MAX_SIZE];
    printf("%zu\n", sizeof(students)); // 10
    printf("%d\n", ADD(8)); // 9
    printf("%d\n", SUB_without＿brackets(21, 9)); // 12
    printf("%d\n", MAX(21, 9)); // 21

    // 括號陷阱
    int res = 2 * SUB_without＿brackets(20, 10) / 4; // 2 * 20 - 10 / 4 = 38 
    printf("%d\n", res); // 38
}

//output
10
9
12
38
21
Program ended with exit code: 0
```
>\## 相連符號
```c
#include <stdio.h>

#define A(x) Value_##x
#define B(x, y) x##y

int main(int argc, const char * argv[]) {
    int A(1) = 9;
    int A(a) = 21;
    printf("%d, %d\n", Value_1, Value_a);
    
    int B(a, 1) = 21;
    int B(b, 1) = 9;
    printf("%d, %d\n", a1, b1);
}

//output
9, 21
21, 9
Program ended with exit code: 0
```  
> \# 引入字串  
```c
#include <stdio.h>

#define A(x) #x

int main(int argc, const char * argv[]) {
    char str[] = A(Hello World!);
    printf("%s\n", str);
}

//output
Hello World!
Program ended with exit code: 0
```
> \ 換行要加的
```c
#include <stdio.h>

#define COMPARE(a, b, ans) \
if (a > b) ans = 1; \
else if (a < b) ans = 0; \
else ans = -1;

int main(int argc, const char * argv[]) {
    int res;
    int a = 21;
    int b = 9;
    COMPARE(a, b, res);
    printf("%d\n", res);
}

//output
1
Program ended with exit code: 0
```
## 巨集判斷式
`#if` `#elif` `#else` `#endif`
```c
#include <stdio.h>

#define SWITCH 0

int main(int argc, const char * argv[]) {
    // 反灰的部分等同於註解
#if SWITCH == 0
    printf("offical_mode\n");
#elif SWITCH == 1
    printf("develop_mode\n");
#else
    printf("test_mode\n");
#endif
}

//output
offical_mode
Program ended with exit code: 0
```
`#ifdef` `#ifndef`
```c
#include <stdio.h>

#define MAJOR "CSIE"

int main(int argc, const char * argv[]) {
    // 反灰的部分等同於註解
#ifdef MAJOR
    printf("You have defined MAJOR!\n");
#else
    printf("You haven't defined MAJOR!\n");
#endif
    
#ifndef SUBJECT
    printf("You haven't defined SUBJECT!\n");
#else
    printf("You have defined SUBJECT!\n");
#endif
}

//output
You have defined MAJOR!
You haven't defined SUBJECT!
Program ended with exit code: 0
```
# 別名 typedef
>就是把資料型別重新取名字
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef unsigned char uint8; //char -> 1 byte -> 8 bits
typedef unsigned short uint16; // short -> 2 bytes -> 16 bits
typedef unsigned int uint32; // int -> 4 btyes -> 32 bits
typedef unsigned long long uint64; // long long -> 8 bytes -> 64 bits
typedef char* String; // 字元陣列

#define STRING_COPY(s, x) strcpy(s, #x)

int main(int argc, const char * argv[]) {
    uint8 a = 255;
    printf("uint8_MAX = %u\n", a);
    uint16 b = 65535;
    printf("uint16_MAX = %u\n", b);
    uint32 c = 4294967295;
    printf("uint32_MAX = %u\n", c);
    uint64_t d = 9223372036854775807;
    printf("uint64_MAX = %llu\n", d);
    
    String s = (String)malloc(50);
    STRING_COPY(s, Hello World!); // strcpy(s, "Hello World!");
    printf("%s\n", s);
}

//output
uint8_MAX = 255
uint16_MAX = 65535
uint32_MAX = 4294967295
uint64_MAX = 9223372036854775807
Hello World!
Program ended with exit code: 0
```
# 結構體 struct
### struct 宣告、初始化 (assignment)

```C
#include <stdio.h>

struct Student {
    char name[50];
    char major[30];
    int age;
};

void show_student(struct Student student) {
    printf("Name: %s\n", student.name);
    printf("Major: %s\n", student.major);
    printf("Age: %d\n", student.age);
}

int main(int argc, const char * argv[]) {
    struct Student Jimmy = {"Jimmy", "Computer Science", 22}; // struct init
    show_student(Jimmy);
    printf("------------------------------\n");
    
    // 對 struct 單一元素初始化
    struct Student Jeremy = {.name = "Jeremy"}; 
    show_student(Jeremy);
}

//output
Name: Jimmy
Major: Computer Science
Age: 22
------------------------------
Name: Jeremy
Major:
Age: 0
Program ended with exit code: 0
```
## struct 巢狀結構：struct 裡再放 struct
```c
#include <stdio.h>
#include <string.h>

struct Student {
    char name[50];
    char major[30];
    int age;
};

struct School {
    char name[10];
    float PR;
    struct Student student;
};

void show_school(struct School school) {
    printf("School: %s\n", school.name);
    printf("PR: %f\n", school.PR);
    printf("Student: %s\n", school.student.name);
    printf("Major: %s\n", school.student.major);
    printf("Age: %d\n", school.student.age);
}

int main(int argc, const char * argv[]) {
    struct School NCKU = {"NCKU", 91.5, {"Jimmy", "Computer Science", 22}}; // struct init
    show_school(NCKU);
    /*NCKU.student.name = "Jeremy";*/ // error
    strcpy(NCKU.student.name, "Jeremy");
    printf("-------------------------\n");
    show_school(NCKU);
}

//output
School: NCKU
PR: 91.500000
Student: Jimmy
Major: Computer Science
Age: 22
-------------------------
School: NCKU
PR: 91.500000
Student: Jeremy
Major: Computer Science
Age: 22
Program ended with exit code: 0
```
:::warning
注意：字串只能在初始化時用assign的方式賦值，否則皆須用strcpy()來更改。
:::
## struct 簡寫
```c
#include <stdio.h>
#include <string.h>

typedef struct {
    char name[50];
    char major[30];
    int age;
} Student;

typedef struct {
    char name[10];
    float PR;
    Student student;
} School;

int main(int argc, const char * argv[]) {
    School NCKU = {"NCKU", 91.5, {"Jimmy GAY", "Computer Science", 22}}; // struct init
}

```
## struct size 計算
:::info
Compiler為了效能考量，會自動做最佳化，也就是資料對齊。

步驟：

1. 從 struct 中選出最大的型別之 size 為單位。
2. struct 由上而下擺放。
3. 剩餘空間補好補滿。
:::
```c
#include <stdio.h>

typedef struct {
    int a;
    char b;
    char c;
} Size1;

typedef struct {
    char b;
    int a;
    char c;
} Size2;

int main(int argc, const char * argv[]) {
    printf("Size1: %zu bytes\nSize2: %zu bytes\n", sizeof(Size1), sizeof(Size2));
}

//output
Size1: 8 bytes
Size2: 12 bytes
Program ended with exit code: 0
```
![截圖 2024-09-15 上午9.27.30](https://hackmd.io/_uploads/Bymzun760.png)

```c
#include <stdio.h>

typedef struct {
    char b;
    char c;
    double a;
} Size3;

typedef struct {
    char b;
    double a;
    char c;
} Size4;

int main(int argc, const char * argv[]) {
    printf("Size3: %zu bytes\nSize4: %zu bytes\n", sizeof(Size3), sizeof(Size4));
}

//output
Size3: 16 bytes
Size4: 24 bytes
Program ended with exit code: 0
```
![截圖 2024-09-15 上午9.33.08](https://hackmd.io/_uploads/SySPKnQpA.png =50%x)![截圖 2024-09-15 上午9.33.43](https://hackmd.io/_uploads/BJ8tYhXTR.png =50%x)

# BitFields
```c
#include <stdio.h>

typedef struct{
    unsigned char bit0: 1; // 控制 1 個 bit
    unsigned char bit1: 1;
    unsigned char bit2: 1;
    unsigned char bit3: 1;
    unsigned char bit4: 1;
    unsigned char bit5: 1;
    unsigned char bit6: 1;
    unsigned char bit7: 1;
} BitFieldsChar;

int main(int argc, const char * argv[]) {
    // 對 char_8 的每一個 bit 初始化
    BitFieldsChar char_8 = {0, 0, 0, 0, 0, 0, 0, 0};

    // 將第 4 個 bit 設成 1 (即 char_8 -> 000010002 = 8)
    char_8.bit3 = 1;
    printf("%d\n", *((unsigned char*)&char_8));
}

//output
8
Program ended with exit code: 0
```
# 共用體 union
:::info
語法與 struct 相同，但運作邏輯不同。union 的記憶體是共用的。如下圖所示：![截圖 2024-09-15 上午10.36.32](https://hackmd.io/_uploads/ry6EOTXa0.png)
因此，不論初始化 union 中的哪一個元素，都**只會有一個元素被儲存**，使 union 能夠**支援多種不同的資料型別(多型)**。

:::
```c
#include <stdio.h>

typedef union{
    int a;
    float b;
    char c;
} Sample;

int main(int argc, const char * argv[]) {
    Sample s = {65, 15.3, 'a'};
    printf("%d %f %c\n", s.a, s.b, s.c);
}

//output
65 0.000000 A
Program ended with exit code: 0
```

## union size 計算
:::info
取 union 中 size 最大的元素作為 union 的 size。
:::
```c
#include <stdio.h>

typedef union{
    int a; // 4 bytes
    float b; // 4 bytes
    char c; // 1 bytes
} Size1; // -> 4 bytes

typedef union{
    int a; // 4 bytes
    double b; // 8 bytes
    char c; // 1 bytes
} Size2; // -> 8 bytes

int main(int argc, const char * argv[]) {
    printf("Size1 = %zu\nSize2 = %zu\n", sizeof(Size1), sizeof(Size2));
}

//output
Size1 = 4
Size2 = 8
Program ended with exit code: 0
```

## Union 實現多型
```c
#include <stdio.h>
#include <string.h>

typedef union{
    int a;
    float b;
    char c[20];
} Polymorphism; // -> Size = 20 bytes

int main(int argc, const char * argv[]) {
    Polymorphism p;
    p.a = 10;
    printf("%d\n", p.a);
    
    p.b = 15.3;
    printf("%f\n", p.b);
    
    strcpy(p.c, "Hello World!");
    printf("%s\n", p.c);
}

//output
10
15.300000
Hello World!
Program ended with exit code: 0
```
## Union 應用
### 控制 Byte
```c
#include <stdio.h>
#include <string.h>

typedef union{
    unsigned int total;
    char detail[4]; // id, majorNum, score, clubNum
} Student; // Size -> 4 Bytes

int main(int argc, const char * argv[]) {
    Student Jimmy;
    Jimmy.detail[0] = 9;
    Jimmy.detail[1] = 21;
    Jimmy.detail[2] = 90;
    Jimmy.detail[3] = 7;
    printf("%d\n", Jimmy.total);
}

//output
123344137
Program ended with exit code: 0
```
![75B1C348-E98B-4833-87F2-F72261804771_1_201_a](https://hackmd.io/_uploads/rJJDv0XTR.jpg)

### union + struct 控制 Byte
```c
#include <stdio.h>
#include <string.h>

typedef union{
    unsigned int total;
    struct {
        char id;
        char majorNum;
        char score;
        char clubNum;
    };
} Student; // Size -> 4 Bytes

int main(int argc, const char * argv[]) {
    Student Jimmy;
    Jimmy.id = 9;
    Jimmy.majorNum = 21;
    Jimmy.score = 90;
    Jimmy.clubNum = 7;
    printf("%d\n", Jimmy.total);
}

//output
123344137
Program ended with exit code: 0
```

### union + struct 控制 Bits
```c
#include <stdio.h>
#include <string.h>

typedef union {
    char header;
    struct {
        unsigned char bit0: 1;
        unsigned char bit1: 1;
        unsigned char bit2: 1;
        unsigned char bit3: 1;
        unsigned char bit4: 1;
        unsigned char bit5: 1;
        unsigned char bit6: 1;
        unsigned char bit7: 1;
    };
} ControlBits;

int main(int argc, const char * argv[]) {
    ControlBits cb;
    cb.header = 0; // init
    
//    cb.header |= 0x01 << 3;
    cb.bit3 = 1;
    printf("pull high bit3 -> %d\n", *((unsigned char*)&cb.header));
    
//    cb.header &= ~(0x01 << 3);
    cb.bit3 = 0;
    printf("then, pull down bit3 -> %d\n", *((unsigned char*)&cb.header));
}

//output
pull high bit3 -> 8
then, pull down bit3 -> 0
Program ended with exit code: 0
```
# 枚舉 enum
:::info
程式碼要盡量避免使用缺乏解釋或命名的數值，又稱Magic Number，而 enum 通常就是用來取代 Magic Number，目的是為了**增加程式碼的可讀性**。
:::
```c
#include <stdio.h>

typedef enum {
    Monday, // 0
    Tuesday, // 1
    Wednesday, // 2
    Thursday = 10, // 以下的枚舉會從 10 開始累加
    Friday, // 11
    Saturday, // 12
    Sunday, // 13
} DAY;

int main(int argc, const char * argv[]) {
    DAY today = Sunday;
    printf("Today is Sunday: %d\n", today);
    
    printf("%d\n", Wednesday);
    printf("%d\n", Thursday);
    printf("%d\n", Friday);
}

//output
Today is Sunday: 13
2
10
11
Program ended with exit code: 0
```
```c
#include <stdio.h>

typedef enum {
    Monday, // 0
    Tuesday, // 1
    Wednesday, // 2
    Thursday, // 3
    Friday, // 4
    Saturday, // 5
    Sunday, // 6
    
    MAX, // 7 統計共有多少枚舉
} DAY;

int main(int argc, const char * argv[]) {
    char TODO[][30] = {
        "go swimming",
        "play basketball",
        "play volleyball",
        "play piano",
        "go hiking",
        "go on a trip",
        "go camping"
    };
    
    DAY today = Sunday;
    printf("Today: %s\n", TODO[today]);
    
    for (int i = 0; i < MAX; i++) {
        printf("%s\n", TODO[i]);
    }
}

//output
Today: go camping
go swimming
play basketball
play volleyball
play piano
go hiking
go on a trip
go camping
Program ended with exit code: 0
```

# 指標 pointer
:::info
指標大小: 4bytes(電腦為32bits) or 8bytes(電腦為64bits) ，不論甚麼型態指標皆同樣size e.g. sizeof(char*) == sizeof(int*) == sizeof(double*)
:::

## 記憶體配置

![截圖 2024-09-16 上午9.36.24](https://hackmd.io/_uploads/rJNCoWr6A.png)

## & : 取址運算子 、 * : 宣告及初始化指標/取值運算子

```c
#include <stdio.h>

int a = 1; // global variable

void foo(void) {
    static int b = 9; // static variable
    int c = 21; // local variable
    
    int* a_ptr = &a; // 指標 a_ptr 指向 a 的位址
    int* b_ptr = &b;
    int* c_ptr = &c;
    
    printf("%p %p %p\n", a_ptr, b_ptr, c_ptr); // 印出指標指向的位址
    printf("%p %p %p\n", &a, &b, &c); // 印出 a b c 變數對應的位址
    
    printf("%d %d %d\n", *a_ptr, *b_ptr, *c_ptr); // 印出指標所指位址中的值
    (*a_ptr) ++; // a++;
    (*b_ptr) ++; // b++;
    (*c_ptr) ++; // c++;
    printf("%d %d %d\n", *a_ptr, *b_ptr, *c_ptr);
}

int main(int argc, const char * argv[]) {
    foo();
}

// output
0x100008000 0x100008004 0x16fdff2bc
0x100008000 0x100008004 0x16fdff2bc
1 9 21
2 10 22
Program ended with exit code: 0
```

## Swap 函式
```c
#include <stdio.h>

void swap(int* a, int* b) { 
    int tmp = *a; // 將位址 a 中的值取出存入 tmp 中
    *a = *b; // 再將位址 b 中的值存入位址 a 中
    *b = tmp; // 最後，將 tmp 的值存入位址 b 中
}

int main(int argc, const char * argv[]) {
    int a = 9;
    int b = 21;
    printf("Before: a = %d, b = %d\n", a, b);
    swap(&a, &b); // 傳入a, b的位址
    printf("After: a = %d, b = %d\n", a, b);
}

//output
Before: a = 9, b = 21
After: a = 21, b = 9
Program ended with exit code: 0
```
## Pointer size 計算
:::info
無論指標的型別，其 size 皆為 8 bytes(64-bit OS) or 4 bytes(32-bit OS)
:::
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    printf("%zu %zu %zu\n", sizeof(int*), sizeof(char*), sizeof(double*));
}

//output
8 8 8
Program ended with exit code: 0
```

## 動態記憶體配置
:::info
malloc() 創造出的空間會存放在 Heap，直到執行 free() 後該空間才會被釋放。另外，free() 只能對儲存在 Heap 區的資料使用。
:::
### malloc()
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, const char * argv[]) {
    int* pi; // 宣告整數指標 pi
    pi = (int*)malloc(sizeof(int)); // pi 指向新配置的動態記憶體位址
    *pi = 5; // 於動態記憶體位址中存入5

    printf("pi指標存放的記憶體位址：%p\npi動態配置的記憶體位址(即pi的內容)：%p\npi指向之記憶體位址中的內容為：%d\n", &pi, pi, *pi);
}

//output
pi指標存放的位址：0x16fdff2c8
pi動態配置的位址(即pi的內容)：0x6000021940e0
pi指向之記憶體位址中的內容為：5
Program ended with exit code: 0
```
:::danger
記憶體內容如下所示：
| 記憶體位址 | 存放的內容 | 記憶體區塊 |
| --- | --- | --- |
| 0x6000021940e0 | 5 | Heap |
| 0x16fdff2c8 | 0x6000021940e0 | Stack |
:::

### free()

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, const char * argv[]) {
    int* pi; // 宣告整數指標 pi
    pi = (int*)malloc(sizeof(int)); // pi 指向新配置的動態記憶體位址
    *pi = 5; // 於動態記憶體位址中存入5
    printf("pi指標存放的記憶體位址：%p\tpi動態配置的記憶體位址(即pi的內容)：%p\tpi指向之記憶體位址中的內容為：%d\t", &pi, pi, *pi);
    
    // 釋放 pi 所指向的記憶體空間
    free(pi);
    printf("pi指標存放的記憶體位址：%p\tpi動態配置的記憶體位址(即pi的內容)：%p\tpi指向之記憶體位址中的內容為：%d\t", &pi, pi, *pi);
}

// output
pi指標存放的記憶體位址：0x16fdff2c8 pi動態配置的記憶體位址(即pi的內容)：0x600000668060 pi指向之記憶體位址中的內容為：5
pi指標存放的記憶體位址：0x16fdff2c8 pi動態配置的記憶體位址(即pi的內容)：0x600000668060 pi指向之記憶體位址中的內容為：710836320
Program ended with exit code: 0
```
:::danger
可以發現，雖然 pi 仍指向原本存放 5 的記憶體位址，但其內容已經被存入其他數值了！(被作業系統視為已釋放的記憶體區塊)
| 記憶體位址 | 存放的內容 | 記憶體區塊 |
| --- | --- | --- |
| 0x600000668060 | 710836320 | Heap |
| 0x16fdff2c8 | 0x600000668060 | Stack |
:::

### 常見問題 -- 記憶體洩漏 (Memory leak)
:::info
發生原因：因為程式在動態分配記憶體後沒有正確地釋放它。

在使用 malloc、calloc 或 realloc 等函數分配記憶體時，會得到一個指向某記憶體區域的指標。如果這個指針在使用過程中被覆蓋掉，且原本的記憶體區域沒有被釋放，那麼這部分記憶體就會變成「失去指標的記憶體」，即稱記憶體洩漏。
:::
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, const char * argv[]) {
    // 第一次分配
    int* pi = (int*)malloc(sizeof(int)); 

    // 第二次分配，第一次分配的記憶體現在已經沒有指標指向它了
    pi = (int*)malloc(sizeof(int)); 

    free(pi); // 只會釋放第二次分配的記憶體，第一次分配的記憶體依然還存在，這就是記憶體洩漏。
}
```
### 常見問題 -- 非法記憶體存取 (segment fault)
#### 存取已被釋放的記憶體空間
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, const char * argv[]) {
    int* pi = (int*)malloc(sizeof(int));
    *pi = 5;
    free(pi);
    *pi = 10; // pi 指向的記憶體空間已被釋放，但又在存取該記憶體空間。
}
```
#### 避免存取已被釋放的記憶體空間之方法
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, const char * argv[]) {
    int* pi = NULL;
    if (pi == NULL) { // 若 pi 尚未指向任何記憶體空間
        pi = (int*)malloc(sizeof(int)); // 才 malloc 記憶體空間給 pi
    }
    
    *pi = 5;
    
    if (pi != NULL) { // 若尚未釋放 pi 所指向之記憶體空間
        free(pi); // 則釋放 pi 所指向之記憶體空間
        pi = NULL;
    }
}
```

### 常見問題 -- 全域指標與靜態指標初始化問題
:::info
全域指標與靜態指標是無法直接初始化的，要先將指標指派為NULL，再初始化。另外，全域指標只能在函數中指派。
:::
#### 全域指標、靜態指標的初始化與釋放 (雙重指標)
```c
#include <stdio.h>
#include <stdlib.h>

int* global = NULL;

void pointer_init(int** p) {
    if (*p == NULL) {
        *p = (int*)malloc(sizeof(int));
    }
    printf("pointer_address: %p\n", *p);
}

void pointer_free(int** p) {
    if (*p != NULL && p != NULL) {
        free(*p);
        *p = NULL;
    }
    printf("pointer_address: %p\n", *p);
}

int main(int argc, const char * argv[]) {
    pointer_init(&global);
    pointer_init(&global); // 與第一次init的位址一樣
    pointer_free(&global);
    static int* s_pointer = NULL;
    pointer_init(&s_pointer);
    pointer_init(&s_pointer); // 與第一次init的位址一樣
    pointer_free(&s_pointer);
}

//output
pointer_address: 0x600000e94080
pointer_address: 0x600000e94080
pointer_address: 0x0
pointer_address: 0x600000e94080
pointer_address: 0x600000e94080
pointer_address: 0x0
Program ended with exit code: 0
```

## 指標與陣列
:::info
陣列名稱即為指向該陣列首位元素之指標。
:::
### 一維陣列
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int arr[] = {1, 2, 3, 4, 5};
    // arr[1] 即 *(arr + 1)
    printf("%d\n", arr[1]);
    printf("%d\n", *(arr + 1));
    
    // &arr[1] 即 arr + 1
    printf("%p\n", &arr[1]);
    printf("%p\n", arr + 1);
    
    int* arr_ptr = arr;
    for (int i = 0; i < 5; i++) {
        printf("%d, ", *(arr_ptr + i));
    }
    printf("\n");
}

// output
2
2
0x16fdff274
0x16fdff274
1, 2, 3, 4, 5, 
Program ended with exit code: 0
```
### 二為陣列
```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int matrix[][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };
    
    // matrix[1][1] 即 *(*(matrix + 1) + 1)
    printf("%d\n", matrix[1][1]);
    printf("%d\n", *(*(matrix + 1) + 1));
    
    // &matrix[1][1] 即 *(matrix + 1) + 1 或 matrix[1] + 1
    printf("%p\n", &matrix[1][1]);
    printf("%p\n", *(matrix + 1) + 1);
    printf("%p\n", matrix[1] + 1);
    
    int (*matrix_ptr)[3] = matrix; // 宣告二維陣列的指標
    int* ptr1D = matrix_ptr[0]; // matrix_ptr[0] 即 matrix[0]
    for (int i = 0; i < 6; i++) {
        printf("%d, ", *(ptr1D + i));
    }
    printf("\n");
}

//output
5
5
0x16fdff280
0x16fdff280
0x16fdff280
1, 2, 3, 4, 5, 6, 
Program ended with exit code: 0
```
### 利用函式修改陣列內容
```c
#include <stdio.h>

void set_1Darr_val(int* arr, int size, int val) {
    for(int i = 0; i < size; i++) {
        arr[i] = val;
    }
}

void set_2Darr_val(int (*arr)[3], int row, int col, int val) {
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            arr[i][j] = val;
        }
    }
}

int main(int argc, const char * argv[]) {
    int arr1D[] = {1, 2, 3, 4, 5};
    printf("Brfore: %d, %d, %d, %d, %d\n", arr1D[0], arr1D[1], arr1D[2], arr1D[3], arr1D[4]);
    set_1Darr_val(arr1D, 5, 21);
    printf("After: %d, %d, %d, %d, %d\n", arr1D[0], arr1D[1], arr1D[2], arr1D[3], arr1D[4]);
    
    int arr2D[][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };
    printf("Brfore: %d, %d, %d, %d, %d, %d\n", arr2D[0][0], arr2D[0][1], arr2D[0][2], arr2D[1][0], arr2D[1][1], arr2D[1][2]);
    set_2Darr_val(arr2D, 2, 3, 21);
    printf("After: %d, %d, %d, %d, %d, %d\n", arr2D[0][0], arr2D[0][1], arr2D[0][2], arr2D[1][0], arr2D[1][1], arr2D[1][2]);
}

//output
Brfore: 1, 2, 3, 4, 5
After: 21, 21, 21, 21, 21
Brfore: 1, 2, 3, 4, 5, 6
After: 21, 21, 21, 21, 21, 21
Program ended with exit code: 0
```
### 動態記憶體配置一維陣列
```c
#include <stdio.h>
#include <stdlib.h>

void set_1Darr_val(int* arr, int size, int val) {
    for(int i = 0; i < size; i++) {
        arr[i] = val;
    }
}

int main(int argc, const char * argv[]) {
    int* vector1D = (int*)malloc(sizeof(int) * 5);
    set_1Darr_val(vector1D, 5, 9);
    printf("%d, %d, %d, %d, %d\n", vector1D[0], vector1D[1], vector1D[2], vector1D[3], vector1D[4]);
}

//output
9, 9, 9, 9, 9
Program ended with exit code: 0
```