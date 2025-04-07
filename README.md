 # 📓Advanced_C📓
----

<details>
<summary><b>📖BÀI 1: COMPILER - MACRO</b></summary>
 
## 1. Compiler - Trình biên dịch
- **Compiler (Trình biên dịch )**: là chương trình biên dịch các code của ngôn ngữ lập trình tương ứng thành các mã nhị phân mà máy có thể hiểu được.
- Quá trình biên dịch gồm 4 giai đoạn:

![image](https://github.com/user-attachments/assets/a0dfa386-3802-4682-a506-cd6534989b3d)
<br>&nbsp;**a. Preprocess (Tiền xử lý):**<br>
&nbsp;&nbsp;- &nbsp;**Tác dụng:** Chuyển các _file.c_, _file.h_ sang _file.i_.<br>
&nbsp;&nbsp;- &nbsp;**Đặc điểm:**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;+ &nbsp;Xử lý các loại chỉ thị tiền xử lý.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;+ &nbsp;Xóa bỏ các chú thích.<br>
&nbsp;&nbsp;- &nbsp;**Cú pháp:** `gcc -E main.c -o main.i`.<br>

&nbsp;**b. Compiler (Biên dịch):**<br>
&nbsp;&nbsp;- &nbsp;**Tác dụng:** Chuyển _file.i_ sang _file.s_.<br>
&nbsp;&nbsp;- &nbsp;**Đặc điểm:**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;+ &nbsp;_file.s_: là file assembly code thao tác được trực tiếp với CPU.<br>
&nbsp;&nbsp;- &nbsp;**Cú pháp:** `gcc -S main.i -o main.s`.<br>

&nbsp;**c. Assembler (Hợp ngữ):**<br>
&nbsp;&nbsp;- &nbsp;**Tác dụng:** Chuyển _file.s_ sang _file.o_.<br>
&nbsp;&nbsp;- &nbsp;**Đặc điểm:**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;+ &nbsp;Dịch chương trình sang mã máy 0 và 1.<br>
&nbsp;&nbsp;- &nbsp;**Cú pháp:** `gcc -c main.s -o main.o`.<br>

&nbsp;**d. Linker (Liên kết):**<br>
&nbsp;&nbsp;- &nbsp;**Tác dụng:** Chuyển _file.o_ sang _file.exe_.<br>
&nbsp;&nbsp;- &nbsp;**Đặc điểm:**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;+ &nbsp;Dịch chương trình sang mã máy 0 và 1.<br>
&nbsp;&nbsp;- &nbsp;**Cú pháp:** `gcc main.o test.o -o main`.<br>
## 2. Marco
- **Marco:** Là từ chỉ những thông tin sẽ được xử lý ở quá trình tiền xử lý 
- Các loại chỉ thị tiền xử lý bao gồm:

&nbsp;**a. #include:** Chỉ thị bao hàm tệp.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Chức năng:**  Chèn nội dung file khác vào mã nguồn chính.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**#include <...>:** Thư viện trữ của C. Tìm kiếm file trong thư mục cài đặt.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**#include "...":**  File thư viện do người dùng tự tạo. Tìm kiếm file trong thư mục hiện tại.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Ví dụ:**.<br>
```c
#include <stdio.h>
#include "test.h"                          
```
&nbsp;**b. #define:** Chỉ thị định nghĩa.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Chức năng:**  Dùng để định nghĩa marco, tránh lặp lại những mã nguồn.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;***Note:**  Khi viết define cho 1 hàm có nhiều dòng thì phải có giấu `\` dể liên kết các dòng.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Ví dụ:**.<br>
```c
#define Creat_func(name, cmd)        \
int main()                           \
{                                    \
     printf(#cmd);                   \
}                                    \
```
&nbsp;**c. #undef:** Chỉ thị hủy định nghĩa.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Chức năng:**  Dùng để hủy định nghĩa marco.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Ví dụ:**
```c
#define SIZE 50    
#undef SIZE                          
#define SIZE 40
```

&nbsp;**d. #if, #elif, #else, #endif:** Chỉ thị biên dịch có điều kiện.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Chức năng:**  Dùng để kiểm tra điều kiện của marco.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Ví dụ:**<br>
```c
#define ESP32 1   
#define STM32 2
#define ATmega324 3

#define MCU STM32

#if MCU == STM32
   void digitalWrite(Pin pin, Status state){
     if(state == HIGH){
        GPIOA->BSRR = (1 << pin);
     }
#elif MCU == ESP32
   void digitalWrite(Pin pin, Status state){
     if(state == HIGH){
        GPIO.out_w1ts = (1 << pin);
     }
#else MCU == ATmega324
   void digitalWrite(Pin pin, Status state){
     if(state == HIGH){
        PORTA |= (1 << pin);
     }
#endif
```

&nbsp;**e. #ifdef, #ifndef:** Chỉ thị biên dịch có điều kiện.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Chức năng:**  Kiểm tra xem marco đã được định nghĩ hay chưa để thực hiện thao tác phía dưới nó.<br>
&nbsp;&nbsp;&nbsp;- &nbsp;**Ví dụ:**<br>
```c
#ifndef TEST_H    
#define TEST_H                        

void display();

#endif
```
- Các loại toán tử trong marco bao gồm:

&nbsp;- &nbsp;**##:** nối chuỗi.<br>
&nbsp;- &nbsp;**Ví dụ:**<br>
```c
#define CREATE_VAR(name)    \
int int_##name;             \
char char_##name;           \
CREATE_VAR(test1);   
```
```c
Kq:  int int_test1; char char_test1;   
```
&nbsp;- &nbsp;**#:** chuẩn hóa đoạn văn bản thành chuỗi.<br>
&nbsp;- &nbsp;**Ví dụ:**<br>
```c
#define CREATE_FUNC(name, cmd)
   void name()
   {
     printf(#cmd);
   }
CREATE_FUNC(test1, This is function\n);   
```
```c
Kq:  void test1(){ printf("This is function\n"); }    
```
&nbsp;- &nbsp;**Variadic:** dùng cho những hàm không xác định được tham số truyền vào và gồm 2 thành phần.<br>
&nbsp;&nbsp;&nbsp;&nbsp;+ &nbsp;**... :** biểu thị danh sách đối số.<br>
&nbsp;&nbsp;&nbsp;&nbsp;+ &nbsp;**__VA_ARG__ :** Thay thế bằng danh sách các đối số.<br>
&nbsp;- &nbsp;**Ví dụ:**<br>
```c
#define print(...) __VA_ARG__   
```
  </details>

  
-----------------------------------------------------------------------------------------------------------------------------------------------


<details>
<summary><b>📖BÀI 2: STDARG - ASSERT</b></summary>
 
## 1. Thư viện STDARG
- Cung cấp cá phương thức để làm việc với các hàm số có danh sách đối số không xác định.
- Các marco trong thư viện STDARG:

|Các marco|Cú pháp|Đặc điểm|
|:------------------------:|:------------------------:|:------------------------|
|**`va_list`**|**`va_list ap`**|- Là 1 kiểu dữ liệu đẫ được định nghĩa lại để đại diện cho danh sách các đối số biến đổi.<br> - Có thể viết lại: `typedef char* va_list`.<br> - Ví dụ: **`va_list args`**|
|**`va_start`**|**`va_start(va_list ap, last_fixed_param)`**|- Khởi tạo `va_list` để bắt đầu truy xuất các tham số biến đổi. Nó cần tham số cuối cùng cố định trong danh sách tham số của hàm.<br> - `last_fixed_param` là tên của tham số cố định cuối cùng trước danh sách tham số biến đổi.<br> - Ví dụ:<br>`void ham(int count, ...){ `<br> &nbsp;&nbsp;&nbsp;`va_list args;`<br> &nbsp;&nbsp;&nbsp;`va_start(args, count);}`|
|**`va_arg`**|**`va_arg(va_list ap, type)`**| - Truy cập 1 đối số trong danh sách và chuyển về kiểu `type`.<br> - Mỗi lần gọi sẽ lấy 1 phần tử. <br> - Ví dụ: `va_arg(args, int)`|
|**`va_copy`**|**`va_copy(va_list dest, va_list src);`**| - `dest`: Biến đích kiểu va_list sẽ nhận bản sao.<br> - `src`: Biến nguồn kiểu va_list đã được khởi tạo bằng va_start.<br> - Sao chép dữ liệu từ biến nguồn vào biến đích.<br> - Sao chép dữ liệu giữa các biến có cùng kiểu `va_list`.<br> - Ví dụ: `va_copy(check, args)`|
|**`va_end`**|**`va_end(va_list ap);`**| - Thu hồi địa chỉ con trỏ,<br> - Giải phóng tài nguyên được cấp phát bởi `va_start`<br> - Ví dụ: `va_end(args)`|
<br>

- Ví dụ:<br>
&nbsp;+ Ví dụ 1: Viết hàm in ra dãy số bất kì được điền vào.<br> 
```c
#include <stdio.h>
#include <stdarg.h>

void display(int count, ...) {
    va_list args;
    va_start(args, count);

    for (int i = 0; i < count; i++) {
        printf("Value at %d: %d\n", i, va_arg(args,int)); 
    }
    va_end(args);
}

int main()
{
    display(5, 5, 8, 15, 10, 13);
    return 0;
}
```
&nbsp;+ Ví dụ 2: Viết hàm tính tổng với tham số không xác định (Kết hợp **`STDARG`** với **`__VA_ARGS__`**).<br> 
```c
#include <stdio.h>
#include <stdarg.h>

#define tong(...)  sum(__VA_ARGS__,'\n')
int sum(int count, ...)
    va_list args;
    va_list check;
    
    va_start(args, count);
    va_copy(check, args);

    int result = count;

    while((va_arg(check, char*)) !=  (char*)'\n')
    {
       result +=  va_arg(args, int);
    }
    va_end(args);
    return result;
}

int main()
{
    printf("Tong: %d\n", tong(3, 1, -1, 0, 2));
    return 0;
}
```
## 2. Thư viện ASSERT
- Cung cấp marco `assert` dùng để kiểm tra một điều kiện trong quá trình debug.<br>
&nbsp;+ Nếu điều kiện đúng (true), không có gì xảy ra và chương trình tiếp tục thực thi.<br>
&nbsp;+ Nếu điều kiện sai (false), chương trình dừng lại và thông báo 1 thông điệp lỗi.<br>
- Nếu định nghĩa macro NDEBUG trước khi include `assert.h`, thì toàn bộ các `assert()` sẽ bị vô hiệu hóa khi biên dịch.
- Ví dụ:<br>
  ```c
  #include <assert.h>

  int main()
  {
    int x = 6;
    assert( x = 5); \\ Nếu x không bằng 5 dừng chương trình báo lỗi, nếu x = 5 thực thi tiếp
  }
  ```
  </details>
