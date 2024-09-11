# C語言
## Data Types

| 資料型態 | Size (Byte) |  |
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

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    // printf("%d\n", sizeof(unsigned long)); 會導致警告，
    // 因为 sizeof 回傳的是 size_t 類別，而 %d 是用於格式化 int 類別的格式符。
    printf("%zu\n", sizeof(unsigned long));
    return 0;
}
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
    printf("s_1 size: %u Bytes\ni_1 size: %zu Bytes\n", sizeof(s_1), sizeof(i_1));
    
    //int -> short
    int i_2 = 10;
    short s_2 = (short)i_2;
    printf("\ni_2 size: %u Bytes\ns_2 size: %zu Bytes\n", sizeof(i_2), sizeof(s_2));
    
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
## function
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
## Library (header檔)
>#include <...> 用於新增系統目錄下的header檔，而#include "..."用於新增檔案目錄底下的header檔(自創的library)。
### 創建標頭檔和函式庫
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
### 靜態/動態函式庫
靜態函式庫(static library)
: 把Library包成一個檔案，檔案容量大。

動態函式庫(dynamic library)
: 把Library包成額外的檔案，執行時與執行檔一起執行，較省空間。

![picture1](https://hackmd.io/_uploads/rkWT_ATn0.jpg)


## 巨集(Macro) #define
>巨集不是變數，在執行期間不會被改變。巨集在程式編譯之前就會被替換。其實就是替身。

```c
#include <stdio.h>

#define MAX_SIZE 10
#define ADD(x) (x + 1)
#define SUB_without＿brackets(a, b) a - b
#define MAX(a, b) ((a) > (b) ? (a) : (b))

int main(int argc, const char * argv[]) {
    char students[MAX_SIZE];
    printf("%zu\n", sizeof(students)); // 10
    printf("%d\n", ADD(8)); // 9
    printf("%d\n", SUB_without＿brackets(21, 9)); // 12
    int res = 2 * SUB_without＿brackets(20, 10) / 4; // 2 * 20 - 10 / 4 = 38
    printf("%d\n", res); // 38
    printf("%d\n", MAX(21, 9)); // 21
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
```
### 巨集判斷式
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
## 別名

## struct

## union

## enum

## pointer

##
