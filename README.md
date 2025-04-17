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

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 3: BITMASK</b></summary>
 
## 1. Khái niệm
- **Bitmask**: Là một kỹ thuật trong lập trình, dùng để truy xuất hoặc thao tác trực tiếp trên các bit trong một giá trị nhị phân. Có thể sử dụng bitmask để đặt, xóa và kiểm tra trạng thái của các bit cụ thể trong một từ (word).
- **Bitmask** thường được sử dụng để tối ưu hóa bộ nhớ, thực hiện các phép toán logic trên một cụm bit, và quản lý các trạng thái, quyền truy cập, hoặc các thuộc tính khác của một đối tượng.
## 2. Các toán tử bitwise
### 2.1. Toán tử NOT - NOT bitwise
- Dùng để thực hiện phép NOT bitwise trên từng bit của một số. Kết quả là bit đảo ngược của số đó.<br>
![image](https://github.com/user-attachments/assets/40656c9e-3be8-4e7c-ac22-b7a035ec1d10)
    |a|y = ~a|
    |:--:|:--:|
    |0|1|
    |1|0|
- Ví dụ:
  ```c
  int main()
  {
     uint8_t a = 0b00001110;
     a = ~a; 
  ```
  ```c
  Kq: a = 0b11110001
  ```
### 2.2. Toán tử AND - AND bitwise
- Dùng để thực hiện phép AND bitwise giữa từng cặp bit của hai số. Kết quả là 1 nếu cả hai bit tương ứng đều là 1, ngược lại là 0.<br>
 ![image](https://github.com/user-attachments/assets/2ae95c18-e924-4da4-89fb-8cd7791fb963)
    |a|b|y = a & b|
    |:--:|:--:|:--:|
    |0|0|0|
    |0|1|0|
    |1|0|0|
    |1|1|1|
 - Ví dụ:
   ```c
   int main()
   {
      uint8_t a = 0b00001110;
      uint8_t b = 0b11110001;
      uint8_t result;
      result = a & b;
   ```
   ```c
   Kq: result = 0b00000000
   ```
### 2.3. Toán tử OR - OR bitwise
- Dùng để thực hiện phép OR bitwise giữa từng cặp bit của hai số. Kết quả là 1 nếu có hơn một bit.<br>
 ![image](https://github.com/user-attachments/assets/34b7b8f0-6dd2-4a73-9712-56fde6a8246e)
    |a|b|y|
    |:--:|:--:|:--:|
    |0|0|0|
    |0|1|1|
    |1|0|1|
    |1|1|1|
- Ví dụ:
  ```c
  int main()
  {
     uint8_t a = 0b00001110;
     uint8_t b = 0b11110001;
     uint8_t result;
     result = a | b;
  ```
  ```c
  Kq: result = 0b11111111
  ```
### 2.4. Toán tử XOR - XOR bitwise
- Dùng để thực hiện phép XOR bitwise giữa từng cặp bit của hai số. Kết quả là 1 nếu chỉ có một bit tương ứng là 1.<br> 
 ![image](https://github.com/user-attachments/assets/7b000a23-1941-4702-b8f9-6e374947a4ca)
    |a|b|y = a ^ b|
    |:--:|:--:|:--:|
    |0|0|0|
    |0|1|1|
    |1|0|1|
    |1|1|0|
- Ví dụ:
  ```c
  int main()
  {
     uint8_t a = 0b00001111;
     uint8_t b = 0b11110001;
     uint8_t result;
     result = a ^ b;
  ```
  ```c
  Kq: result = 0b11111110
  ```
### 2.5. Các phép dịch trái (Shift left) và phép dịch phải (Shift right)
- Dùng để di chuyển bit sang trái hoặc sang phải.
- **Phép dịch trái (Shift left):** Các bit ở bên phải sẽ được dịch sang trái, và các bit trái cùng sẽ được đặt giá trị 0.
- **Phép dịch phải (Shift right):** Các bit ở bên trái sẽ được dịch sang phải, và các bit phải cùng sẽ được đặt giá trị 0 hoặc 1 tùy thuộc vào giá trị của bit cao nhất.
- Ví dụ:
  ```c
  int main()
  {
     uint8_t a = 0b00001111;
     uint8_t b = 0b11110001;
     a = a << 5; //dịch trái
     b = b >> 4; //dịch phải
  ```
  ```c
  Kq: a = 0b11100000
      b = 0b00001111
  ```
## 3. Ví dụ tổng quát
```c
#include <stdio.h>
#include <stdint.h>

#define GENDER        1 << 0  // Bit 0: Giới tính (0 = Nữ, 1 = Nam)
#define TSHIRT        1 << 1  // Bit 1: Áo thun (0 = Không, 1 = Có)
#define HAT           1 << 2  // Bit 2: Nón (0 = Không, 1 = Có)

// Bật tính năng
void enableFeature(uint8_t *options, uint8_t feature)
{
    *options = *options | feature;
}

//Tắt tính năng
void disableFeature(uint8_t *options, uint8_t feature) {
    *options = *options & (~feature);
}

//Kiểm tra tính năng
int isFeatureEnabled(uint8_t options, uint8_t feature) {
    return ((options & feature) != 0);
}

//Liệt kê các tính năng đã bật
void listSelectedFeatures(uint8_t options) {
    printf("Selected Features:\n");

    const char* featureName[] =
    {
        "Gender",
        "Shirt",
        "Hat",

    };
    for (int i = 0; i < 8; i++)
    {
      if ((options >> i) & 1)
      {
        printf("%s\n", featureName[i]);   
      }
    }
}

int main(int argc, char const *argv[]) {
{
  uint8_t options = 0;
  uint8_t *ptr = &options;

  enableFeature(&options, GENDER | TSHIRT | HAT);   // Bật tính năng

  disableFeature(&options, HAT | TSHIRT);    // Loại bỏ tính năng
}
  listSelectedFeatures(options);    // Liệt kê tính năng
  return 0;
}
```

  </details>


-----------------------------------------------------------------------------------------------------------------------------------------------


<details>
<summary><b>📖BÀI 4: POINTER</b></summary>
 
## 1. Khái niệm
- **Con trỏ (pointer):** Là một biến chứa địa chỉ bộ nhớ của một đối tượng khác (biến, mảng, hàm)
- Việc sử dụng con trỏ giúp chúng ta thực hiện các thao tác trên bộ nhớ một cách linh hoạt hơn.
## 2. Đặc điểm con trỏ
### 2.1. Khai báo con trỏ
- Cú pháp: `<Kiểu dữ liệu> *<tên biến>`
- Trong đó:<br>
&nbsp;+ Kiểu dữ liệu là: void, char, int, ...<br>
&nbsp;+ Dấu * trước tên biến là ký hiệu báo cho trình biên dịch biết ra.
- Ví dụ: <br>
```c
int *ptr_int;       // con trỏ đến kiểu int
char *ptr_char;     // con trỏ đến kiểu char
float *ptr_float;   // con trỏ đến kiểu float
```
### 2.2. Lấy địa chỉ của biến
- Con trỏ khi trỏ đến biến sẽ lưu địa chỉ ô nhớ đầu tiên được cấp phát cho biến đó.
- Cú pháp: `<Kiểu dữ liệu> *<tên biến 1> = &<tên biến 2>`
- Trong đó:<br>
&nbsp;+ Kiểu dữ liệu là: void, char, int, ...<br>
&nbsp;+ Dấu * trước tên biến là ký hiệu báo cho trình biên dịch biết ra.<br>
&nbsp;+ &<tên biến 2>: là phép lấy địa chỉ của biến 2.
- Ví dụ: <br>
```c
int x = 10;       //Address: 0x01 0x02 0x03 0x04
                  //Value:	0b00..00
int *ptr_x = &x;  // ptr_x chứa địa chỉ của x
                  // &ptr_x = 0xc1
                  // ptr_x = 0x01
```
 Truy cập giá trị (giải tham chiếu - dereference)
- Để lấy giá trị từ con trỏ ta sử dụng phép giải tham chiếu.
- Cú pháp: `*<tên biến 1> = <tên biến 2>`
- Trong đó:<br>
&nbsp;+ *<tên biến 1>: là phép lấy giá trị từ con trỏ.
- Ví dụ: <br>
```c
int x = 10;
int *ptr_x = &x;
*ptr_x = *(0x01) = 10
```
### 2.4. Kích thước con trỏ
- Kích thước của con trỏ phụ thuộc vào kiến trúc máy tính và trình biên dịch hoặc kiến trúc vi xử lý.
- Phải đồng bộ kiểu dữ liệu với biến để tránh đọc sai giá trị
- Ví dụ: Với máy tính có hệ điều hành 64 bit thì con trỏ sẽ có kích thước 8 bytes (64 bit).
## 3. Mối quan hệ giữa con trỏ và mảng
- Kích thước mảng = số lượng phần tử của mảng x kích thước kiểu dữ liệu
```c
int main() {
  int arr[] = {1, 2, 3, 4, 5};
  
  int *ptr = arr;
  
  int n = sizeof(arr)/sizeof(arr[0]);  // số lượng phần tử trong mảng
  
  for (int i; i < n; i++)
  {
     // ptr là địa chỉ phần tử thứ 1
     // ptr + 1 là địa chỉ phần tử thứ 2
     // .....
     // *ptr +i 'là giá trị phần tử thứ i
     printf("Dia chi: %p - Gia tri: %d\n",ptr + i, (*ptr +i));
  }
}
```
## 3. Void pointer
- **Void pointer** thường dùng để trỏ để tới bất kỳ địa chỉ nào mà không cần biết tới kiểu dữ liệu của giá trị tại địa chỉ đó.
- Cú pháp: ` void *ptr_void;`
- Ưu điểm: Thay vì phải khai báo nhiều con trỏ với các kiểu dữ liệu khác nhau thì ta có thể tối ưu bằng cách khai báo 1 con trỏ void và dùng nó để trỏ tới nhiều biến với các kiểu dữ liệu khác nhau giúp tối ưu bộ nhớ hơn
- Nhược điểm: cú pháp phức tạp vì phải ép kiểu lại
- Ví dụ:
```c
#include <stdio.h>

int main()
{
   void *ptr

    int value = 5;
    double test = 15.7;
    char arr[] = "Hello World"; //ký tự NULL (\'0')
    
    ptr = &value;
    printf("Address: %p - Value: %d\n", ptr, *(int*)(ptr));

    ptr = &test;
    printf("Address: %p - Value: %f\n", ptr, *(double*)(ptr));

    ptr = arr;
    for (int i = 0; i < (sizeof(arr)/sizeof(arr[0])); i++)
    {
       printf("Address: %p - Value: %c\n", ptr+i, *(char*)(ptr+i));
    }

    void *ptr1[] = { &value, &test, arr };
    printf("Address: %p - Value: %d\n", ptr1[0], *(int*)ptr1[0]);
    printf("Address: %p - Value: %f\n", ptr1[1], *(double*)ptr1[1]);
    printf("Address: %p - Value: %c\n", ptr1[1], *(char*)ptr1[1]);
    return 0;
}
```
## 4. Con trỏ hàm - Function Pointer
### 4.1. Khái niệm - Cú pháp
- **Con trỏ hàm (Function Pointer)** là một biến mà giữ địa chỉ của một hàm. Có nghĩa là, nó trỏ đến vùng nhớ trong bộ nhớ chứa mã máy của hàm được định nghĩa trong chương trình.
- Trong ngôn ngữ lập trình C, con trỏ hàm cho phép bạn truyền một hàm như là một đối số cho một hàm khác, lưu trữ địa chỉ của hàm trong một cấu trúc dữ liệu, hoặc thậm chí truyền hàm như một giá trị trả về từ một hàm khác.
- Cú pháp: `<return_type> (*func_pointer)(<data_type_1>, <data_type_2>);`
- Ví dụ:
```c
int sum ( int a, int b);
int (*ptr)(int, int);
ptr = sum; 
```
### 4.2. Các cách gọi hàm 
```c
void funcA();
void (*ptr)();
ptr = &funcA; // hoặc có thể viết ptr = funcA
```
- Gọi thông qua tên: FuncA();
- Gọi thông qua con trỏ hàm:<br> 
  &nbsp;+ Gọi trực tiếp như gọi hàm: ptr();<br>
  &nbsp;+ Sử dụng dấu * giải tham chiếu: (*ptr)();
### 4.3. Ưu điểm - nhược điểm
- Ưu điểm: Có độ linh hoạt cao
- Nhược điểm: Tốc độ châm hơn so với gọi hàm thông qua tên
### 4.4. Ví dụ
- Ví dụ 1: Khai báp như 1 biến
  ```c
  #include <stdio.h>
  
  int tong(int a, int b){ return a + b; }
  
  int hieu(int a, int b){ return a - b; }
  
  int tich(int a, int b){ return a * b; }
  
  int thuong(int a, int b){ return (double)a / b; }
  
  int main()
  {
      int (*ptr)(int,int);  //Khai báo con trỏ hàm
  
       ptr = tong;
       printf("Tong: %d\n, ptr(2,3));
  
       ptr = hieu;
       printf("Hieu: %d\n, ptr(2,3));
  
       ptr = tich;
       printf("Tich: %d\n, ptr(2,3));
  
       ptr = thuong;
       printf("Thuong: %d\n, ptr(5,3));
  
       return 0;
  }
  ```
  ```c
  Kq: Tong: 5
       Hieu: -1
       Tich: 6
       Thuong: 1,666666
  ```
 - Ví dụ 2: Khai báo con trỏ hàm dưới dạng mảng con trỏ
   ```c
   void tong(int a, int b) {printf("Tong là: %d", a+b);}
   
   void hieu(int a, int b) {printf("Hieu là: %d", a-b);}
   
   void tich(int a, int b) {printf("Tich là: %d", a*b);}
   
   void thuong(int a, int b) {printf("Thuong là: %d", (double)a/b);}
   
   int main ()
   {
     void (*ptr)(int, int);  // Khai báo con trỏ hàm
   
     void (*ptr_arr[])(int, int) = {tong, hieu, tich, thuong};  
     ptr_arr[0](2,3);  // Gọi hàm tổng
     ptr_arr[1](2,3);  // Gọi hàm hiệu
     ptr_arr[2](2,3);  // Gọi hàm tích
     ptr_arr[3](5,3);  // Gọi hàm thuong
   
    return 0;
   }
   ```
   ```c
   Kq: Tong: 5
        Hieu: -1
        Tich: 6
        Thuong: 1,666666
   ```
   - Ví dụ 3: Khai báo con trỏ hàm dưới dạng tham số truyền vào
   ```c
   void tong(int a, int b) {printf("Tong là: %d", a+b);}
   
   void hieu(int a, int b) {printf("Hieu là: %d", a-b);}
   
   void tich(int a, int b) {printf("Tich là: %d", a*b);}
   
   void thuong(int a, int b) {printf("Thuong là: %d", (double)a/b);}
   
   void tinhtoan(void (*pheptoan)(int, int), int a, int b)
   {
      pheptoan(a,b);
   }
   
   int main ()
   {
     void (*ptr)(int, int);  // Khai báo con trỏ hàm
   
     tinhtoan(tong, 2, 3);  // Truyền tham số là hàm tong để tính tổng.
     tinhtoan(hieu, 2, 3);  // Truyền tham số là hàm tong để tính hiệu.
     tinhtoan(tich, 2, 3);  // Truyền tham số là hàm tong để tính tích.
     tinhtoan(thuong, 5, 3);  // Truyền tham số là hàm tong để tính thương.
     return 0;
   }
   ```
   ```c
   Kq: Tong: 5
        Hieu: -1
        Tich: 6
        Thuong: 1,666666
   ```
## 5. Con trỏ trỏ tới hằng số - Pointer to Constant
- Khái niệm: Là cách định nghĩa một con trỏ không thể thay đổi giá trị tại địa chỉ mà nó trỏ đến thông qua dereference nhưng giá trị địa chỉ đó có thể thay đổi.
- Cú pháp:<br>
  ```c
  <data type> const *ptr_const;
  const <data type> *ptr_const;
  ```
- Ứng dụng: Giứ lại dữ liệu trước đó 
- Ví dụ:<br> 
  ```c
  int main()
  {
     int value = 5;
     int test = 8;
     const int *ptr_const = &value;

     printf("value: %d\n", *ptr_const);  //read - only

     //*ptr_const = 7; Sai
     ptr_const = &test //đúng

     printf("value: %d\n", *ptr_const);
     return 0;
  }
  ```
## 6. Hằng con trỏ -  Constant Pointer
- Khái niệm: Định nghĩa một con trỏ khi được khởi tạo thì nó sẽ không thể trỏ tới địa chỉ khác.
- Cú pháp:<br>
  ```c
  <data type> *const ptr_const;
  const <data type> *const ptr_const;  // kết hợp hằng con trỏ và con trỏ hằng
  ```
- Ví dụ:<br> 
  ```c
  #include <stdio.h>
 
  int main()
  {
      int value = 5;
      int test = 15;
      int *const const_ptr = &value;
  
      printf("value: %d\n", *const_ptr);
  
      *const_ptr = 7;
      printf("value: %d\n", *const_ptr);
  
      //const_ptr = &test; // wrong
      return 0;
  }
  ```
## 7. Con trỏ NULL -  NULL Pointer
- Khái niệm: là một con trỏ không trỏ đến bất kỳ đối tượng hoặc vùng nhớ cụ thể nào.
- Ứng dụng: Dùng để kiểm tra xem một con trỏ đã được khởi tạo và có trỏ đến một vùng nhớ hợp lệ chưa tránh thay đổi dữ liệu mà nó trỏ tới => Khi dùng xong hoặc không dùng nên gắn con trỏ NULL
- Ví dụ:<br> 
  ```c
  #include <stdio.h>

  int main()
  {
      int *ptr = NULL;  // Gán giá trị NULL cho con trỏ    0x0000000
  
      if (ptr == NULL)
      {
          printf("Pointer is NULL\n");
      }
      else
      {
          printf("Pointer is not NULL\n");
      }
  
      int score_game = 5;
      if (ptr == NULL)
      {
          ptr = &score_game;
          *ptr = 30;
          ptr = NULL; // khi không dùng nữa gắn NULL
      }
      return 0;
  }
  ```
  ## 8. Con trỏ đến con trỏ -  Pointer to Pointer (Con trỏ cấp 2
- Khái niệm: cho phép lưu trữ địa chỉ của một con trỏ khác
- Ứng dụng:<br>
&nbsp;+ Kiểu dữ liệu JSON.<br>
&nbsp;+ Cấu trúc dữ liệu danh sách liên kết. <br>
- Ví dụ:<br> 
  ```c
  int test = 5; //Address: 0x01, Value:	5
  
  int *ptr = &test; // Address: 	0xf1, Value:	0x01
  
  int **ptp = &ptr; //Address: 	0xef, Value:	0xf1

  **ptp = 5 // Giải tham chiếu con trỏ cấp 2
  ```
  </details>

  
-----------------------------------------------------------------------------------------------------------------------------------------------


<details>
<summary><b>📖BÀI 5: Goto - setjmp.h </b></summary>
 
## 1. Goto
- **Goto:**: à một từ khóa trong ngôn ngữ lập trình C, cho phép chương trình nhảy đến một nhãn (label) đã được đặt trước đó trong cùng một hàm. 
- Ưu điểm: Kiểm soát luồng chạy chương trình
- Nhược điểm:
  &nbsp;+ Làm cho mã nguồn trở nên khó đọc và khó bảo trì.<br>
  &nbsp;+ Chỉ sử dụng trong cùng 1 hàm.<br>
- Ví dụ:
```c
 #include <stdio.h>
 
 int main()
 {
    int i = 0;
 
    // Đặt nhãn
    start:
       if (i >= 5)
       {
          goto end;  // Chuyển control đến nhãn "end"
       }
 
       printf("%d ", i);
       i++;
 
       goto start;  // Chuyển control đến nhãn "start"
 
    // Nhãn "end"
    end:
       printf("\n");
    return 0;
 }

```
## 2. Thư viện setjmp
- **setjmp.h:** là một thư viện trong ngôn ngữ lập trình C, cung cấp hai hàm chính là setjmp và longjmp.
- Ứng dụng: Dùng để xử lý ngoại lệ trong C (debug chương trình ).
- **setjmp(jmp_bufenv)**: Lưu trữ vị trí mà cái hàm được gọi ra ( vị trí setjmp đang đứng) để có thể quay lại bằng **longjmp**.<br>
&nbsp;+ Trả về 0 khi được gọi lần đầu.<br>
&nbsp;+ Trả về một giá trị khác 0 khi quay lại từ **longjmp**.<br>
- **longjmp(jmp_buf env, int value):** Nhảy về vị trí hiện tại của setjmp và tiếp tục thực thi từ đó.
- Ví dụ:<br>
 ```c
 #include <stdio.h>
 #include <setjmp.h>
 
 jmp_buf buf;
 
 int exception = 0;
 
 void func2()
 {
     printf("This is function 2\n");
     longjmp(buf, 2);  // Nhảy trở lại vị trí setjmp và trả lại giá trị 2
 }
 
 void func3()
 {
     printf("This is function 3\n");
     longjmp(buf, 3);   // Nhảy trở lại vị trí setjmp và trả lại giá trị 3
 }
 
 void func1()
 {
     exception = setjmp(buf);   //đánh dấu lưu trữ vị trí hàm setjmp đang thực thi
     if (exception == 0)
     {
         printf("This is function 1\n");
         printf("exception = %d\n", exception);
         func2();
     }
     else if (exception == 2)
     {
         printf("exception = %d\n", exception);
         func3();
     }
     else if (exception == 3)
     {
         printf("exception = %d\n", exception);
     }
 }
 
 int main(int argc, char const *argv[])
 {
     func1();
     return 0;
 }
 ```
## 3. Xử lý ngoại lệ - Exception Handling
- **Xử lý ngoại lệ (Exception Handling):** là một cơ chế trong lập trình giúp phát hiện và xử lý các lỗi thường liên quan lỗi hệ thống hoặc tình huống bất thường xảy ra trong quá trình thực thi chương trình, giúp chương trình hoạt động ổn định và không bị dừng đột ngột.
### 3.1. **Ngoại lệ (Exception):** là những lỗi hoặc sự kiện không mong muốn xảy ra trong quá trình thực thi chương trình, chẳng hạn như:<br>
&nbsp;+ Chia một số cho 0 (division by zero).<br>
&nbsp;+ Truy cập mảng ngoài phạm vi (out of bounds array access).<br>
&nbsp;+ Truy xuất con trỏ null (null pointer dereference).<br>
&nbsp;+ Lỗi khi mở hoặc đọc tập tin (file not found).<br>
&nbsp;+ Lỗi cấp phát bộ nhớ (bad allocation).<br>
### 3.2. **Cơ chế xử lý ngoại lệ:** giúp chương trình phản ứng kịp thời với các lỗi mà không làm gián đoạn toàn bộ chương trình.
- Hầu hết các ngôn ngữ lập trình hiện đại như C++, Java, Python, C# đều hỗ trợ xử lý ngoại lệ thông qua các từ khóa chính như:
&nbsp;+ **try:** Định nghĩa một khối lệnh có thể phát sinh lỗi.<br>
&nbsp;+ **catch:** Xử lý ngoại lệ nếu có lỗi xảy ra.<br>
&nbsp;+ **throw:** Ném ra một ngoại lệ khi xảy ra lỗi.<br>
=> Muốn sử dụng những lệnh trên trong C phải định nghĩa trong setjump.

- Cú pháp:<br>
  ```c
   try
   {
      // Khối lệnh có thể phát sinh lỗi
   }
   catch (loại_ngoại_lệ_1)
   {
      // Xử lý ngoại lệ loại 1
   }
   catch (loại_ngoại_lệ_2)
   {
      // Xử lý ngoại lệ loại 2
   }
   catch (...)
   {
      // Xử lý tất cả các ngoại lệ khác
   }
  ```
- Ví dụ: <br>
  ```c
    #include <stdio.h>
  #include <setjmp.h>
  
  jmp_buf buf;
  
  int exception_code;
  
  typedef enum
  {
      NO_ERROR,
      NO_EXIT,
      DIVIDE_BY_0
  } ErrorCodes;  
  
  #define TRY if ((exception_code = setjmp(buf)) == 0)
  #define CATCH(x) else if (exception_code == x)
  #define THROW(x) longjmp(buf, x)
  
  double divide(int a, int b)
  {
      if (a == 0 && b == 0)
      {
          THROW(NO_EXIT);
      }
      else if (b == 0)
      {
          THROW(DIVIDE_BY_0);
      }
  
      return (double)a/b;
  }
  
  int main(int argc, char const *argv[])
  {
      exception_code = NO_ERROR;
  
      TRY
      {
          printf("Ket qua: %0.3f\n", divide(0,0));
      }
      CATCH(NO_EXIT)
      {
          printf("ERROR! Không tồn tại\n");
      }
      CATCH(DIVIDE_BY_0)
      {
          printf("ERROR! Chia cho 0\n");
      }
  
      // thêm code ở đây
      printf("Hello world\n");
      return 0;
  }
  ```
- **Note:** Sự khác nhau giữa **TRY - CATCH - THROW** và **ASSERT**:<br>
&nbsp;+ **ASSERT:** Khi có lỗi đưa ra thông báo lỗi chi tiết và dừng ngay chương trình khi có lỗi.<br>
&nbsp;+ **TRY - CATCH - THROW:** Khi có lỗi đưa ra thông báo lỗi nhưng không dừng ngay chương trình khi có lỗi.<br>
</details>

  
-----------------------------------------------------------------------------------------------------------------------------------------------


<details>
<summary><b>📖BÀI 6: Goto - setjmp.h </b></summary>
 
## 1. Từ khóa Extern
- **Goto:**: à một từ khóa trong ngôn ngữ lập trình C, cho phép chương trình nhảy đến một nhãn (label) đã được đặt trước đó trong cùng một hàm. 
- Ưu điểm: Kiểm soát luồng chạy chương trình
- Nhược điểm:
  &nbsp;+ Làm cho mã nguồn trở nên khó đọc và khó bảo trì.<br>
  &nbsp;+ Chỉ sử dụng trong cùng 1 hàm.<br>
- Ví dụ:
```c
 #include <stdio.h>
 
 int main()
 {
    int i = 0;
 
    // Đặt nhãn
    start:
       if (i >= 5)
       {
          goto end;  // Chuyển control đến nhãn "end"
       }
 
       printf("%d ", i);
       i++;
 
       goto start;  // Chuyển control đến nhãn "start"
 
    // Nhãn "end"
    end:
       printf("\n");
    return 0;
 }

```

</details>
