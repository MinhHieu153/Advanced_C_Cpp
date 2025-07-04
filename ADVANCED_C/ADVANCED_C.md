📓Advanced_C📓
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
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;+ &nbsp;Liên kết các file.o lại với nhau.<br>
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
int name()                           \
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
|**`va_list`**|**`va_list ap`**|- Là 1 kiểu dữ liệu đẫ được định nghĩa lại để đại diện cho danh sách các đối số biến đổi.<br> - Khai báo biến dùng để truy cập các đối số biến.<br> - Có thể viết lại: `typedef char* va_list`.<br> - Ví dụ: **`va_list args`**|
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
- Ví dụ 1: Khai báo như 1 biến
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
- Khái niệm: Là cách định nghĩa một con trỏ không thể thay đổi giá trị tại địa chỉ mà nó trỏ đến thông qua dereference nhưng địa chỉ đó có thể thay đổi.
- Cú pháp:<br>
  ```c
  <data type> const *ptr_const;
  const <data type> *ptr_const;
  ```
- Ứng dụng: Giữ lại dữ liệu trước đó 
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
  ## 8. Con trỏ đến con trỏ -  Pointer to Pointer (Con trỏ cấp 2)
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
- Nhược điểm:<br>
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
- Hầu hết các ngôn ngữ lập trình hiện đại như C++, Java, Python, C# đều hỗ trợ xử lý ngoại lệ thông qua các từ khóa chính như:<br>
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
<summary><b>📖BÀI 6: Storage Classes </b></summary>
 
## 1. Từ khóa Extern
- **Extern:** là từ khóa được sử dụng để thông báo rằng một biến hoặc hàm đã được khai báo ở một nơi khác trong chương trình hoặc trong một file nguồn khác. Giúp chương trình hiểu rằng biến hoặc hàm đã được định nghĩa và sẽ được sử dụng từ một vị trí khác.
- Ví dụ:<br>

&nbsp;+ File test.c.<br>
```c
 #include <stdio.h>

int var_global = 50; // 0x01

void display()
{
    printf("%d\n",var_global);
}
```
&nbsp;+ File test.h.<br>
```c
#ifndef TEST_H
#define TEST_H

extern int var_global;

extern void display();

endif
```
&nbsp;+ File main.c.<br>
```c
#include <stdio.h>
#include "test.h"

int main(int argc, char const *argv[])
{
    display();
    return 0;
}
```
## 2. Từ khóa Static local
- Khi **Static** Khi static được sử dụng với biến cục bộ (khai báo biến trong một hàm):<br>
&nbsp;+ Giữ phạm vi của biến chỉ trong hàm đó.<br>
&nbsp;+ Giữ giá trị của biến qua các lần gọi hàm.
<br>
- Ví dụ:<br>

```c
#include <stdio.h>
int *ptr =NULL;
void counnt()
{
    static int count = 0;  // Biến static giữ giá trị qua các lần gọi hàm
    ptr = &a; //dùng con trỏ thay đổi biến stactic a từ bên ngoài
    int count1 = 0 // giá trị tự reset sau mỗi lần gọi hàm
    printf("Count: %d\n", ++count);
    printf("Count1: %d\n", ++count);
}

int main()
{
    count();  // In ra "Count: 1"
    count();  // In ra "Count: 2"
    count();  // In ra "Count: 3"

    *ptr =99;
    count();  // In ra "Count: 3"
    return 0;
}
```

```c
Kq:
Count: 1 
Count1: 0
Count: 2
Count1: 0
Count: 3
Count1: 0
Count: 100
Count1: 0
}
```
## 3. Từ khóa Static global
- Khi static được sử dụng với biến, hàm toàn cục, nó hạn chế phạm vi của biến, hàm đó chỉ trong file nguồn hiện tại.
- Ứng dụng: dùng để thiết kế các file thư viện.
- Ví dụ:<br>
```c
#include <stdio.h>
#include "test.h>

static int a = 10 // gắn biến static chỉ sử dụng trong hàm này

static void display(){
  printf(this is test1.c\n");
int *ptr = &a;
}
void test(){  
  printf("Hello Word\n);
}
```
## 4. Từ khóa Volatile
- Dùng để báo hiệu cho trình biên dịch rằng một biến có thể thay đổi ngẫu nhiên, ngoài sự kiểm soát của chương trình.
- Giúp ngăn chặn trình biên dịch tối ưu hóa hoặc xóa bỏ các thao tác trên biến đó, giữ cho các thao tác trên biến được thực hiện như đã được định nghĩa.<br>
```c
#include "stm32f10x.h"

uint8_t *addr = (uint8_t*)0x20000000;
volatile uint8_t var = 0;  //giúp a cập nhật khi bị thay đổi ngẫu nhiên

int main()
{
   while(1)
   {
     var = *addr;
     if (var != 0) break;
   }
}
```
## 5. Register - thanh ghi
- Giúp cho biến lưu trực tiếp vào thanh ghi không qua Ram làm tăng tốc độ xử lý
- Tuy nhiên, lưu ý rằng việc sử dụng register chỉ là một đề xuất cho trình biên dịch và không đảm bảo rằng biến sẽ được lưu trữ trong thanh ghi. Trong thực tế, trình biên dịch có thể quyết định không tuân thủ lời đề xuất này.
- Không dùng cho biến toàn cục vì:<br>
&nbsp;+ Thanh ghi không có địa chỉ nên việc truy cập khó.<br>
&nbsp;+ Số lượng thanh ghi hạn chế.<br>
- Ví dụ: <br>
```c
#include <stdio.h>
#include <time.h>

int main()
{
   // Lưu thời điểm bắt đầu
   clock_t start_time = clock();
   register int i;

   // Đoạn mã của chương trình
   for (i = 0; i < 2000000; ++i){}

   // Lưu thời điểm kết thúc
   clock_t end_time = clock();

   // Tính thời gian chạy bằng miligiây
   double time_taken = ((double)(end_time - start_time)) / CLOCKS_PER_SEC;

   printf("Thoi gian chay cua chuong trinh: %f    giay\n", time_taken);
   return 0;
}

```
</details>

-----------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 7: Struct - Union </b></summary>

## 1. Struct
### 1.1. Khái niệm
- **Struct:** là một cấu trúc dữ liệu cho phép lập trình viên tự định nghĩa một kiểu dữ liệu mới bằng cách nhóm các biến có các kiểu dữ liệu khác nhau lại với nhau
### 1.2. Cú pháp
- **Khai báo tường mình**.<br>
```c
struct name_struct
{
   <data type 1> <member 1>;
   <data type 2> <member 2>;
    // ...
};
```
- **Khai báo không tường mình**.<br>

```c
typdef struct 
{
   <data type 1> <member 1>;
   <data type 2> <member 2>;
    // ...
}name_struct;
```
- **Ví dụ:** <br>

&nbsp;+ Khai báo tường mình.<br>
```c
struct User
{
   char *name;
   int age;
   char *addr;
};
struct User user1, user2, *user3;
```
&nbsp;+ Khai báo không tường mình.<br>
```c
typdef struct 
{
   char *name;
   int age;
   char *addr;
} User;
User user1, user2, *user3;
```
### 1.3. Truy xuất dữ liệu
- Sử dụng ".": Toán tử truy xuất tới thành viên khi khai báo biến bình thường.<br>
- Sử dụng "->": Toán tử truy xuất tới thành viên khi khai báo biến là con trỏ.<br>
&nbsp;+ Ví dụ:<br>
```c
user1,name = "Hieu";
user3->name = "Hieu";
```
### 1.4. Kích thước của struct
- **Data Alignment:** là quá trình sắp xếp biến thành viên của struct sao cho các biến nằm ở địa chỉ phù hợp với yêu cầu căn chỉnh của CPU. Nó sẽ sắp xếp sao cho địa chỉ các biến là số chẵn và phù hợp với hệ thống
- **Data padding:** Là byte dữ liệu trống 
- **Kích thước Struct:** là bội kích thước của phần tử có kích thước lớn nhất
- Ví dụ:<br>
```c
typdef struct 
{  // cấp phát theo thành viên lớn nhất là char* addr ( kích thước con trỏ phụ thuộc vào kiến trúc của hệ thống: 32bit, 64bit)=> cấp phát 8byte
   char *name; // 0xa0, 0xa1, 0xa2, 0xa3 ( padding: 0xa1, 0xa2, 0xa3 )
   int age;    // 0xa4, 0xa5, 0xa6, 0xa7
   char *addr; // 0xa8, 0xa9, 0xaa, 0xab. 0xac, 0xad, 0xae, 0xaf
} User;
// cấp phát địa chỉ dựa trên kích thước member lớn nhất
// 0xa0, 0xa1, 0xa2, 0xa3, 0xa4, 0xa5, 0xa6, 0xa7 (lần 1)
// 0xa8, 0xa9, 0xaa, 0xab . 0xac, 0xad, 0xae, 0xaf (lần 2)
//Tổng 16 byte
// dùng: 13 byte
// dư: 3 byte -> 3 padding
```
&nbsp;+ kích thước mảng struct:<br>
```c
struct Example1 
{
    uint8_t  arr1[5];
    uint16_t arr2[0];  
    uint16_t arr2[1]; 
    uint16_t arr2[2]; 
    uint16_t arr2[3];
    uint32_t arr3[2];
};
```
=> Cấp phát bộ nhớ:<br>
![image](https://github.com/user-attachments/assets/2864a73a-aa37-441e-9ca1-8e9d8f357079)
### 1.5. Bit Field
- **Bit Field:** là một thành phần đặc biệt của cấu trúc (struct) cho phép bạn chỉ định số lượng bit cụ thể dùng để lưu trữ một biến số nguyên. Thay vì sử dụng toàn bộ kích thước của một kiểu dữ liệu, bạn có thể “cắt nhỏ” bộ nhớ theo số bit cần thiết, giúp tiết kiệm không gian bộ nhớ và mô tả chính xác hơn ý nghĩa của dữ liệu (ví dụ: lưu trạng thái bật/tắt chỉ cần 1 bit)
- Cú pháp:<br>

```c
struct name_struct 
{
  <data type 1> <member 1> : <number of bits>;
  <data type 2> <member 2> : <number of bits>;
  //...
};
```
- Ví dụ: <br>

```c
struct Example
{
   int32_t flag :1;  // sử dụng 1 trong 32 bit
   int64_t count :4; // sử dụng 4 trong 64 bit
};
```
- Số bit chỉ định trực tiếp giới hạn phạm vi giá trị có thể lưu. Ví dụ: một bit field khai báo với : 3 có thể lưu các giá trị từ 0 đến 7 (đối với unsigned).
- Không thể sử dụng toán tử lấy địa chỉ (&) trên  các thành viên bit field.
- **Chú ý:**

&nbsp;+ Không dùng cho kiểu float.<br>
&nbsp;+ Không thể truy cập địa chỉ.<br>
## 2. Union
### 2.1. Khái niệm
- **Union:** là một cấu trúc dữ liệu giúp lập trình viên kết hợp nhiều kiểu dữ liệu khác nhau vào cùng một vùng nhớ.
- **Union** giúp tiết kiệm bộ nhớ bằng cách chia sẻ cùng một vùng nhớ cho các thành viên của nó. Điều này có nghĩa là, trong một thời điểm, chỉ một thành viên của union có thể được sử dụng.
### 2.2. Cú pháp
- **Khai báo tường mình**.<br>
```c
union name_union
{
   <data type 1> <member 1>;
   <data type 2> <member 2>;
    // ...
};
```
- **Khai báo không tường mình**.<br>
```c
typdef union 
{
   <data type 1> <member 1>;
   <data type 2> <member 2>;
    // ...
}name_union;
```
- **Ví dụ:** <br>

&nbsp;+ Khai báo tường mình.<br>
```c
union Data 
{
   uint8_t  arr1[5];
   uint16_t arr2[9];
   uint32_t arr3[3];
};
union Data data1, data2, *data3;
```
&nbsp;+ Khai báo không tường mình.<br>
```c
typdef union 
{
   uint8_t  arr1[5];
   uint16_t arr2[9];
   uint32_t arr3[3];
} Data;
Data data1, data2, *data3;
```
### 2.3. Kích thước Union
- Trong union, tất cả các thành viên cùng chia sẻ một vùng nhớ.
- Kích thước của union sẽ bằng với kích thước của thành viên lớn nhất + padding.
- Chỉ một thành viên lưu trữ giá trị tại một thời điểm nếu không dữ liệu sẽ bị ghi đè.
- Ví dụ: <br>
```c
union Data 
{
  uint8_t a1[5]; // 5 byte
  uint8_t a2[3]; // 3 byte
  uint8_t a3[6]; // 6 byte
};
```
=> Cấp phát bộ nhớ:<br>
![image](https://github.com/user-attachments/assets/7f4ea88c-cbfb-4cae-beea-407f459280bd)

### 2.5. So sánh Union - Struct
- Giống:<br>
&nbsp;+ là kiểu dữ liệu tự định nghĩa.<br>
&nbsp;+ cách truy xuất dữ liệu. <br>
- Khác:
  ||Struct|Union|
  |:------------------------:|:------------------------:|:------------------------:|
  |Kích thước|Tổng kích thước thành viên + Padding|Tổng của thành viên lớn nhất + padding|
  |Vùng nhớ - memory|Mỗi thành viên đều có vùng nhớ riêng|Dùng chung vùng nhớ|
  |Truy xuất vùng nhớ|Không ảnh hưởng khi biến thành viên thay đổi|Ảnh hưởng khi biến thành viên thay đổi|
  
</details>

-----------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 8: Memory Layout</b></summary>
 
- **RAM** có 5 vùng nhớ:<br>
&nbsp;&nbsp;+ Text segment (Code segment). <br>
&nbsp;&nbsp;+ Data segment (Initialized Data). <br>
&nbsp;&nbsp;+ Bss segment (Uninitialized Data). <br>
&nbsp;&nbsp;+ Stack. <br>
&nbsp;&nbsp;+ Heap .
- Ban đầu khai báo biến sẽ được lưu như nào thì sau nó vẫn đc lưu như thế không thay đổi.<br>

  |**Các vùng nhớ**|**Lưu trữ**|**Quyền truy cập**|
  |:------------------------:|------------------------|------------------------|
  |**Text segment (Code segment)**|&nbsp;+ Các mã máy (mã máy: các câu lệnh thực thi trong chương trình).<br>&nbsp;+ Compiler Clang (macOS) lưu trữ biến hằng số toàn cục **(const)** và chuỗi hằng **(string literal)**.<br>|&nbsp;+ Chỉ có quyền đọc và thực thi, nhưng không có quyền ghi.<br>&nbsp;+ Biến hằng số toàn cục **(const)** và chuỗi hằng **(string literal)** chỉ đọc.|
  |**Data segment (Initialized Data- Dữ liệu khởi tạo)**|&nbsp;+ Các biến toàn cục được khởi tạo với giá trị khác 0.<br>&nbsp;+ Lưu trữ cá biến static (global + local) được khởi tạo với giá trị khác 0.<br>&nbsp;+ Với Compiler GCC/G++ (Windows) lưu trữ biến hằng số toàn cục **(const)** và chuỗi hằng **(string literal)**.|&nbsp;+ Có thể đọc, ghi và thay đổi giá trị biến.<br>&nbsp;+ Tất cả các biến được cấp phát sẽ bị thu hồi khi chương trình kết thúc.<br>&nbsp;+ Biến hằng số toàn cục **(const)** và chuỗi hằng **(string literal)** chỉ đọc.|
  |**Bss segment (Uninitialized Data- Dữ liệu không khởi tạo)**|&nbsp;+ Các biến toàn cục khởi tạo với **giá trị bằng 0** hoặc **không gắn giá trị**. <br>&nbsp;+ Lưu trữ cá biến static với **giá trị bằng 0** hoặc **không gắn giá trị**. <br>|&nbsp;+ Có thể đọc, ghi và thay đổi giá trị biến.<br>&nbsp;+ Tất cả các biến được cấp phát sẽ bị thu hồi khi chương trình kết thúc.|
  |**Stack**|&nbsp;+ Các biến cục bộ (trừ static cục bộ), tham số truyền vào. <br>&nbsp;+ Hằng số cục bộ, có thể thay đổi thông qua con trỏ. <br>|&nbsp;+ Có thể đọc, ghi và thay đổi giá trị biến.<br>&nbsp;+ Tất cả các biến được cấp phát sẽ bị thu hồi khi chương trình kết thúc.|

- Ví dụ:<br>

&nbsp; + **Data segment (Initialized Data- Dữ liệu khởi tạo)**
```c
int a = 1; // lưu trong Data segment
static int var = 5 // lưu trong Data segment
int *ptr = &a; // lưu trong Data segment

const int b = 10; // lưu trong Data segment - read only
char *ptr1 = "hello"; // ptr1: lưu trong Data segment - "hello" - là chuỗi hằng nên lưu trong vùng read only ( chuỗi hằng không quan tâm giá trị khởi tạo lưu hết vào data hoặc text tùy trình biên dịch)
int main()
{
  ...
}
```
&nbsp; + **Bss segment (Uninitialized Data- Dữ liệu không khởi tạo)**
```c
int a; // lưu trong bss segment
static int var = 0 // lưu trong bss segment
int *ptr = NULL; // lưu trong bss segment
const int b = 0; // lưu trong data segment ( hằng số toàn cục không quan tâm giá trị khởi tạo lưu hết vào data hoặc text tùy trình biên dịch) - read only
char *ptr1 = "hello"; // lưu trong Data segment - read only

typedef struct
{
  int x;  // vùng địa chỉ x và y phụ thuộc vào cách mình khai báo biến 
  int y;
} Data;
static Data p1 = {0, 0}; // khai báo biến static và p1 khai báo bằng 0 --> x,y,p1 cùng nằm phân vùng bss
Data p2; // khai báo ko gắn giá trị --> x,y,p1 cùng nằm phân vùng bss
Data p3 = {0, 1};  // p3 khơi tạo khác 0 --> x, y, p3 nằm ở phần vùng data mặc dù x = 0 nhưng nó theo phân vùng p3

int main() 
{
  ...
}
```
&nbsp; + **Stack**
```c
// Vì khi viết char ptr1[] = "hello"; trình biên dịch sẽ sao chép nội dung chuỗi literal này vào phân vùng có thể đọc và ghi được
char ptr1[] = "hello"; // ptr1 là tên mảng ko phải biến nó khồng nằm trong phân vùng nào nó phụ thuộc vào phân tử đầu còn "hello" được lưu trong phân vùng có thể đọc và ghi được

// Vì con trỏ này chỉ trỏ thẳng vào biến này
char *ptr = "hello";// ptr1 là biến con trỏ nó nằm trong phân vùng data còn "hello" lưu trong vùng read only

void swap(int *a, int *b) // các biến a,b lưu trong Stack
{
 //&a = 0x01 nó sẽ bị thu hồi địa chỉ khi ra khỏi hàm
  //&b = 0x0a nó sẽ bị thu hồi địa chỉ khi ra khỏi hàm
  const int c = 10; //const local
  ptr = &c;
*ptr = 100;
printf("%d\n", c);
}

int main() 
{
 swap(10,20);
 return 0;
}
```
## 4. Vùng nhớ Heap
### 4.1. Đặc điểm
- Dùng để cấp phát bộ nhớ động trong quá trình thực thi chương trình giúp vùng nhớ cấp phát ra được linh động thay đổi theo đầu vào
- Cho phép chương trình tạo ra và giải phóng bộ nhớ theo nhu cầu của dữ liệu trong quá trình chạy.
- Các hàm như malloc(), calloc(), realloc() được sử dụng để cấp phát và và free() để giải phóng bộ nhớ trên heap và được lưu trong thư viện **stdlib**
- Đệ nguyên: Gọi lai chính bản thân nó và không kết thúc được hàm
### 4.2. Các hàm cấp phát và giải phóng vùng nhớ

 |**Các kiểu cấp phát**|**Đặc điểm**|**cú pháp**|
 |:------------------------:|-------------------|------------------------|
 |**malloc**<br>**calloc**|&nbsp;+ Cấp phát bộ nhớ với kích thước được chỉ định trước.<br>&nbsp;+ Kích thước:<br>&nbsp;&nbsp;* Phụ thuộc vào số lượng x kích thước từng phần tử.<br>&nbsp;&nbsp;* Phụ thuộc vào ép kiểu<br>&nbsp;+ Sau khi cấp phát cần kiểm tra xem cấp phát thành công chưa|&nbsp;+ void *malloc(size_t size)<br>&nbsp;+ void *calloc(size_t num, size_t size)|
 |**realloc**|Thay đổi kích thước vùng nhớ đã được cấp phát|void *realloc(void *_Memory. size_t _NewSize)|
 |**free**|Thu hồi vùng nhớ cấp phát khi không dùng nữa nếu không sẽ không còn vùng nhớ cấp phát và báo lỗi memoryleak|free(ptr)|

- Ví dụ:<br>

```c
#include <stdlib.h>

int main()
{
   // Sử dụng malloc 
   scanf("%d", &size);
   int *ptr = (int*)malloc(size * sizeof(int)); //để truy suất con trỏ kiểu void ta ép kiểu đúng kiểu của từng phần tử để nó đọc đúng giữ liệu

   // kiểm tra có cấp phát thành công không
   if(ptr == NULL)  
   {
      printf("Cấp phát bộ nhớ thất bại\n");
      return 1;  
   }

   // Sử dụng calloc
   arr_calloc = (int*)calloc(size, sizeof(int));

   // Sử dụng realloc
   ptr = (int*)realloc(ptr, 10*sizeof(int)); // cấp phát thêm 10 phần tử kiểu int nữa cho con trỏ ptr

   // Giải phóng bộ nhớ
   free(ptr);
   free(arr_calloc);
   // Sau khi giải phóng xong phải gắn con trỏ = NULL
   return 0;
}
```
## 5. Memoryleak - Overflow
- **Các lỗi:** <br>
&nbsp;+ **overflow:** Ghi dữ liệu vượt quá vùng nhớ (không đủ bộ nhớ).<br>
&nbsp;+ **memory leak:** Xảy ra khi mình cấp phát vùng nhớ mới nhưng không đủ vùng nhớ cấp phát do vùng nhớ lúc trước mình chưa thu hồi nên .
- **Stack:** <br>
&nbsp;+ Nếu chương trình sử dụng quá nhiều bộ nhớ vượt quá khả năng lưu trữ của Stack chắc chắn sẽ xảy ra tình trạng tràn bộ nhớ Stack (Stack overflow).<br>
&nbsp;+ Các trường hợp xảy ra như bạn khởi tạo quá nhiều biến cục bộ, hàm đệ quy vô hạn,...
- **Heap:** <br>
&nbsp;+ Nếu bạn liên tục cấp phát vùng nhớ mà không giải phóng thì sẽ bị lỗi tràn vùng nhớ Heap (Heap overflow).<br>
&nbsp;+ Nếu khởi tạo một vùng nhớ quá lớn mà vùng nhớ Heap không thể lưu trữ một lần được sẽ bị lỗi khởi tạo vùng nhớ Heap thất bại.
## 6. So sánh malloc - calloc
|                          |**malloc**|**calloc**|**realloc**|
|:------------------------:|------------------------|------------------------|------------------------|
|Cú pháp|void* malloc(size_t size)<br>|void *calloc(size_t num, size_t size)|void* realloc(void *_Memory, size_t _NewSize)|
|Chức năng|&nbsp;Memory allocation: Cấp phát một khối bộ nhớ động có kích thước size byte.|&nbsp;Contiguous allocation: Cấp phát vùng nhớ động gồm num phần tử, mỗi phần tử có kích thước size byte.|&nbsp;Reallocation: Thay đổi kích thước của vùng nhớ đã được cấp phát trước đó bởi malloc() hoặc calloc().|
|Giá trị trả về|&nbsp;+ Hàm trả về con trỏ tới vùng nhớ nếu cấp phát thành công với giá trị rác.<br>&nbsp;+ Trả về NUL nếu cấp phát FAIL|&nbsp;+ Hàm trả về con trỏ trỏ tới vùng nhớ được cấp phát và vùng nhớ được khởi tạo giá trị = 0 nếu cấp phát thành công.<br>&nbsp;+ Trả về NUL nếu cấp phát FAIL|&nbsp;+ Hàm trả về con trỏ trỏ tới vùng nhớ đã thay đổi kích thước nếu cấp phát thành công với giá trị rác và giữ lại giá trị cũ.<br>&nbsp;+ Trả về NUL nếu cấp phát FAIL|
## 7. So sánh Stack - Heap
|                          |**Stack**|**Heap**|
|:------------------------:|----------------------|------------------------|
|Chức năng|Lưu trữ các biến cục bộ trong hàm (trừ static cục bộ), tham số truyền vào.|Lưu trữ vùng nhớ cho những biến được cấp phát động bởi các hàm malloc - calloc - realloc|
|Vị trí trong RAM|Do hệ điều hành quản lý tự động.|Quản lý bởi lập trình viên|
|Cách cấp phát|&nbsp;+ Cấp phát trước khi chạy chương trình.<br>&nbsp;+ Không thể cấp phát hay phân bổ lại bộ nhớ khi chạy chương trình.<br>&nbsp;+ Tự động cấp phát.<br>|&nbsp;+ Cấp phát trong quá trình chạy chương trình.<br>&nbsp;+ Có thể điều chỉnh hay giải phóng bộ nhớ khi chạy chương trình.<br>&nbsp;+ Quản lý bộ nhớ thông quá các hàm ```malloc```,```calloc```,```realloc```,```free```|
|Cách thu hồi vùng nhớ|Tự động giải phóng khi hàm thực hiện xong công việc của mình.|Tự tay giải phóng vùng nhớ thông qua hàm **free()**|
|Lỗi thường gặp|Stack overflow|Memory leak|
|Ứng dụng|Biến đơn giản, mảng nhỏ, cố định kích thước |Dữ liệu lớn, mảng có nhiều phần tử, không biết trước kích thước tại thời điểm biên dịch.|

 </details>

-----------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 9: Stack - Queue</b></summary>
 
## 1. Cấu trúc dữ liệu
- **Cấu trúc dữ liệu** là cách **tổ chức**, và **lưu trữ** dữ liệu trong 1 vùng nhớ và từng phần tử có địa chỉ liền kề nhau để chúng có thể được truy cập và sử dụng một cách hiệu quả, đóng vai trò quan trọng trong việc giải quyết các bài toán và tối ưu hóa thuật toán, vì nó ảnh hưởng trực tiếp đến tốc độ thực thi và tính phức tạp của chương trình.
- Cấu trúc dữ liệu chia làm 2 loại chính:<br>
&nbsp;+ **Cấu trúc dữ liệu tuyến tính (Linear Data Structure):** mảng (Array), ngăn xếp (Stack), hàng đợi (Queue), danh sách liên kết (Linked List).<br>
&nbsp;+ **Cấu trúc dữ liệu phi tuyến tính (Non-linear Data Structure):** đồ thị (Graphs), cây (Trees).
## 2. Ngăn xếp - Stack
- **Stack (ngăn xếp)** là một cấu trúc dữ liệu tuân theo nguyên tắc **"Last In, First Out" (LIFO)**, nghĩa là phần tử cuối cùng được thêm vào stack sẽ là phần tử đầu tiên được lấy ra.
- Các thao tác cơ bản trên stack bao gồm:<br>
&nbsp;+ **push:** Thêm một phần tử vào đỉnh của stack và mỗi lần thêm **top+1**.<br>
&nbsp;+ **pop:** Xóa một phần tử ở đỉnh của stack và mỗi lần xóa **top-1**.<br>
&nbsp;+ **peek/top:** Lấy giá trị phần tử ở đỉnh của stack và giá trị khởi tạo ban đầu **top=-1**.<br>
&nbsp;+ Kiểm tra stack đầy: **top = size-1**.<br>
&nbsp;+ Kiểm tra stack rỗng: **top = -1**.<br>
- Khỏi tạo thư viện stack - ngăn xếp.<br>

&nbsp;+ **Thư viện stack.h**

      #ifndef STACK_H
      #define STACK_H
      
      #include <stdio.h>
      #include <stdlib.h>
      #include <stdbool.h>
      
      //Khai báo cáu trúc dữ liệu để khởi tạo ngăn xếp
      typedef struct 
      {
          int *items;      //Mảng lưu trữ giá trị các phần tử
          int size;        //Số lượng phần tử tối đa lưu trữ được
          int top;         //Chỉ số đọc giá trị ở đỉnh ngăn xếp
      } Stack;
      
      //Khởi tạo các thông số ban đầu
      void stack_init(Stack *stack, int newSize);
      
      //Kiểm tra stack rỗng
      bool isEmpty(Stack stack);
      
      //Kiểm tra stack đầy
      bool isFull(Stack stack);
      
      //Thêm phần tử
      void push(Stack *stack, int data);
      
      //Xóa phần tử
      int pop(Stack *stack);
      
      //đọc giá trị phần tử ở đỉnh
      int top(Stack stack);
      
      //giải phóng bộ nhớ
      void stack_free(Stack *stack);
      
      #endif
  
&nbsp;+ **stack.c**

      #include "stack.h"

      void stack_init(Stack *stack, int newSize)
      {
          stack->items = (int*)malloc(newSize * sizeof(int));
          stack->size = newSize;
          stack->top = -1;  // Khởi tạo giá trị ban đầu = -1
      }
      
      bool isEmpty(Stack stack)
      {
          return (stack.top == -1 ? true : false);
      }
      
      bool isFull(Stack stack)
      {
          return (stack.top == stack.size-1) ? true : false;
      }
      
      void push(Stack *stack, int data)
      {
          if (isFull(*stack))
          {
              printf("Stack đầy!\n");
          }
          else
          {
              //stack->top++;
              //stack->items[stack->top] = data;
              printf("Added element: %d\n",data);
              stack->items[++stack->top] = data;       
          }
      }
      
      int pop(Stack *stack)
      {
          if (isEmpty(*stack))
          {
              printf("Stack rỗng!\n");
              return -1;
          }
          else
          {   
              int value = stack->items[stack->top];
              //stack->items[stack->top] = 0;
              //stack->top--;
              stack->items[stack->top--] = 0;
              printf("Removed element: %d\n", value);
              return value;
          }
      
      }
      
      int top(Stack stack)
      {
          if (isEmpty(stack))
          {
              printf("Stack rỗng!\n");
              return -1;
          }
          else
          {   
              return stack.items[stack.top];
          }
      
      }
      
      void stack_free(Stack *stack)
      {
          free(stack->items);
          stack->items = NULL;
      }
      
&nbsp;+ **main.c**

     #include "stack.h"

     int main()
     {
         Stack stack1;
     
         //Khởi tạo ngân xếp
         stack_init(&stack1, 5);
     
         //thêm phần tử vào đỉnh
         push(&stack1, 1);
         push(&stack1, 2);
         push(&stack1, 3);
         push(&stack1, 4);
         push(&stack1, 5);
         push(&stack1, 6);
     
     
         for(int i=0; i<stack1.size; i++)
         {
             printf("Element: %d - Addr: %p\n", stack1.items[i], &(stack1.items[i]));
         }
     
         //Xóa phần tử
         //pop(&stack1);
         //pop(&stack1);
         //pop(&stack1);
         
     
         for(int i=0; i<stack1.size; i++)
         {
             printf("Top Element: %d - Addr: %p\n", top(stack1), &(stack1.items[stack1.top]));
             pop(&stack1);
         }
         return 0;
     } 
## 3. Queue - Hàng đợi
## 3.1. Khái niệm
- **Queue** là một cấu trúc dữ liệu tuân theo nguyên tắc **"First In, First Out" (FIFO)**, nghĩa là phần tử đầu tiên được thêm vào hàng đợi sẽ là phần tử đầu tiên được lấy ra. 
- Các thao tác cơ bản trên hàng đợi bao gồm:<br>
&nbsp;+ **enqueue:** Thêm phần tử vào **cuối** hàng đợi.<br>
&nbsp;+ **dequeue:** Xóa phần tử từ **đầu** hàng đợi.<br>
&nbsp;+ **front** Đọc giá trị phần tử đứng **đầu** hàng đợi.<br>
&nbsp;+ **rear** Đọc giá trị phần tử đứng **cuối** hàng đợi.<br>
&nbsp;+ Kiểm tra hàng đợi đầy/rỗng.<br>
 ![image](https://github.com/user-attachments/assets/7b624f3e-dbb9-4961-aa2c-304a1c029aa5)
-  Có 3 loại hàng đợi:<br>
&nbsp;+ **Linear Queue:** Hàng đợi tuyến tính.<br>
&nbsp;+ **Circular Queue:** Hàng đợi vòng tròn.<br>
&nbsp;+ **Priority Queue:** Hàng đợi ưu tiên.<br>
## 3.2. Linear Queue - Hàng đợi tuyến tính 
### 3.2.1. Đặc điểm
- Trong Linear Queue, nếu ‘rear’ đã đạt tới max (số lượng phần tử tối đa - 1), thì queue sẽ được coi là đầy và không thể thêm phần tử mới, ngay cả khi phía trước còn khoảng trống do các phần tử đã bị xóa.
- Chỉ có thể thêm phần tử mới khi đã dequeue toàn bộ các phần tử hiện có (tức là queue rỗng hoàn toàn và front được reset về vị trí ban đầu).
### 3.2.2. Các thao tác trên hàng đợi
- **Khởi tạo giá trị ban đầu:** <br>
  &nbsp;+ rear = -1.<br>
  &nbsp;+ front = -1
- **Khi thêm phần tử - enqueue:** <br>
&nbsp;+ Khi thêm phần tử đầu tiên.<br>
&nbsp;&nbsp;&nbsp;&nbsp;* front = 0.<br>
&nbsp;&nbsp;&nbsp;&nbsp;* rear = 0.<br>
&nbsp;&nbsp;&nbsp;&nbsp;**enqueue = 3**.<br>
![image](https://github.com/user-attachments/assets/b91eef48-090c-45bd-8611-5dd1fba6cef0)<br>
&nbsp;+ Khi thêm phần tử tiếp theo.<br>
&nbsp;&nbsp;&nbsp;&nbsp;* front = 0.<br>
&nbsp;&nbsp;&nbsp;&nbsp;* rear = rear + 1.<br>
&nbsp;&nbsp;&nbsp;&nbsp;* rear = size - 1 => Hàng đợi đầy(Queue Full).<br>
&nbsp;&nbsp;&nbsp;&nbsp;**enqueue = 7**.<br>
![image](https://github.com/user-attachments/assets/6ae3c0f1-08bc-4b00-8321-6c7ac048b6c2)<br>
- **Khi xóa phần tử - dequeue:** <br>
&nbsp;+ front = front + 1.<br>
&nbsp;+ front > rear => Hàng đợi rỗng(Queue Empty) => reset front = rear = -1.<br>
&nbsp;&nbsp;&nbsp;&nbsp;**dequeue = 3**.<br>
![image](https://github.com/user-attachments/assets/fa99dc54-6516-42c5-8e3b-5439267339a7)<br>
&nbsp;&nbsp;&nbsp;&nbsp;**dequeue = 5**.<br>
![image](https://github.com/user-attachments/assets/52ea84e1-acb4-408a-baf4-97aaed60315d)<br>
&nbsp;&nbsp;&nbsp;&nbsp;**dequeue = 7** => khi lấy hết phần tử front > rear => Hàng đợi rỗng và sẽ reset về -1.<br>
![image](https://github.com/user-attachments/assets/b0d10796-73d8-4cd7-91d3-4229a113da57)<br>
### 3.2.3. Khởi tạo thư viện Linear Queue
- Thư viện linear_queue.h
  ```c
  #ifndef LINEAR_QUEUE_H
  #define LINEAR_QUEUE_H
  
  #include <stdio.h>
  #include <stdlib.h>
  #include <stdbool.h>
  
  typedef struct 
  {
      int *items;
      int size;
      int front;
      int rear;
  } Queue;
  
   //Khởi tạo những thông số ban đầu
   void queue_init(Queue *queue, int newSize);
  
   //Kiểm tra hàng đợi rỗng
  bool queue_isEmpty(Queue queue);
  
   //Kiểm tra hàng đợi đầy
   bool queue_isFull(Queue queue);
  
   //Thêm phần tử vào cuối hàng đợi
   void enqueue(Queue *queue, int value);
  
  //Xóa phần tử ở đầu hàng đợi
  int dequeue(Queue *queue);
  
  //Đọc giá trị phần tử ở đầu hàng đợi
  int front(Queue queue);
  
  //Đọc giá trị phần tử ở cuối hàng đợi
  int rear(Queue queue);
  
  //Giải phóng bộ nhớ
  void queue_free(Queue *queue);
  
  // Hiển thị toàn bộ hàng
  void display(Queue queue);
  
  #endif
  ```
- File linear_queue.c
  ```c
  #include "linear_queue.h"
  
  //Khởi tạo những thông số ban đầu
  void queue_init(Queue *queue, int newSize)
  {
     queue->items = (int*)malloc(newSize*sizeof(int));
     queue->size = newSize;
     queue->front = queue->rear = -1;
  
  }
  
  //Kiểm tra hàng đợi rỗng
  bool queue_isEmpty(Queue queue)
  {
     return (queue.front == -1 || queue.front > queue.rear) ? true : false;
  }
  
  //Kiểm tra hàng đợi đầy
  bool queue_isFull(Queue queue)
  {
     return (queue.front == queue.size - 1 ) ? true : false;
  }
  
  //Thêm phần tử vào cuối hàng đợi
  void enqueue(Queue *queue, int value)
  {
     if(queue->rear == queue->size -1)
     {
         printf("Hang doi day!\n");
     }
     else
     {
         if(queue->front == -1)
         {
             queue->front = queue->rear = 0;
         }
         else
         {
             queue->rear++;
         }
  
         queue->items[queue->rear] = value;
         printf("Enqueue: %d\n", value);
     }
  }
  
  //Xóa phần tử ở đầu hàng đợi
  int dequeue(Queue *queue)
  {
     if(queue_isEmpty(*queue))
     {
         printf("Hang doi rong\n");
         return -1;
     }
     else
     {
         int dequeue_value = queue->items[queue->front];
         queue ->items[queue->front] = 0;
         if (queue->front == queue->rear && queue->rear == queue->size - 1)    //kiểm tra đang ở hàng đợi cuối
         {
             queue->front = queue->rear = -1;    // reset về -1
         }
         else
         {
             queue->front++;    // nếu không thì cộng 1
         }
  
         printf("Dequeue: %d\n", dequeue_value);
         return dequeue_value;
     }
  }
  
  //Đọc giá trị phần tử ở đầu hàng đợi
  int front(Queue queue)
  {
     if(queue_isEmpty(queue))
     {
         printf("Hang doi rong\n");
         return -1;
     }
     
     return queue.items[queue.front];
  }
  
  //Đọc giá trị phần tử ở cuối hàng đợi
  int rear(Queue queue)
  {
     if(queue_isEmpty(queue))
     {
         printf("Hang doi rong\n");
         return -1;
     }
     
     return queue.items[queue.rear];
  }
  //Giải phóng bộ nhớ
  void queue_free(Queue *queue)
  {   
     if( queue->items != NULL)
     {
         free(queue->items);
         queue->items =NULL;
     }
  }
  
  // Hiển thị toàn bộ hàng
  void display(Queue queue)
  {
     if(queue_isEmpty(queue))
     {
         printf("Hang doi rong\n");
     }
     else
     {
         printf("Queue: ");
  
         for (int i = queue.front; i <= queue.rear; i++)
         {
             printf("%d ", queue.items[i]);
         }
         printf("\n");
     }
  }
  ```
- File main.c
  ```c
  #include "linear_queue.h"
  
  int main()
  {
      Queue liqueuel;
  
      //Khởi tạo hàng đợi
      queue_init(&liqueuel, 5);
  
      //Thêm phần tử hàng đợi
      enqueue(&liqueuel, 1);
      enqueue(&liqueuel, 2);
      enqueue(&liqueuel, 3);
      enqueue(&liqueuel, 4);
      enqueue(&liqueuel, 5);
      enqueue(&liqueuel, 6);
      
      // In phần tử đàu (front) và cuối (rear)
      printf("Front: %d\n", front(liqueuel));
      printf("Rear: %d\n", rear(liqueuel));
      
      //Xóa phần tử đầu
      dequeue(&liqueuel);
      dequeue(&liqueuel);
      dequeue(&liqueuel);
  
      //phải xóa hết phần tử trong hàng đợi mới thêm được phần tử mới không nó cứ báo đầy
      
      //Hiện thi phần tử trong hàng đợi
      display(liqueuel);
      return 0;
  }
  ```
  ## 3.3. Circular Queue - Hàng đợi vòng tròn
  ### 3.3.1. Đặc điểm
- Khi rear đạt tới size - 1 và không còn chỗ trống từ phía cuối, nếu front đã di chuyển (nghĩa là đã có các phần tử được dequeue), rear có thể "quay vòng" về vị trí 0 để tận dụng khoảng trống <br>
=> Hàng đợi vòng tròn sẽ tự động xoay vòng tròn để tận dụng byte địa chỉ trống tránh gây lãng phí bộ nhớ
### 3.3.2. Các thao tác trên hàng đợi
- Công thức kiểm tra hàng đợi đầy (queue full): **front == (rear + 1) % SIZE**
- Kiểm tra hàng đợi rỗng (queue empty): **front == -1**
### 3.3.3. Khởi tạo thư viện Circular Queue
- **Circular.h**
  ```c
  #ifndef CIRCULAR_QUEUE_H
  #define CIRCULAR_QUEUE_H
  
  #include <stdio.h>
  #include <stdlib.h>
  #include <stdbool.h>
  
  typedef struct 
  {
      int *items;
      int size;
      int front;
      int rear;
  } Queue;
  
   //Khởi tạo những thông số ban đầu
   void queue_init(Queue *queue, int newSize);
  
   //Kiểm tra hàng đợi rỗng
  bool queue_isEmpty(Queue queue);
  
   //Kiểm tra hàng đợi đầy
   bool queue_isFull(Queue queue);
  
   //Thêm phần tử 
   void enqueue(Queue *queue, int value);
  
  //Xóa phần tử ở đầu hàng đợi
  int dequeue(Queue *queue);
  
  //Đọc giá trị phần tử ở đầu hàng đợi
  int front(Queue queue);
  
  //Đọc giá trị phần tử ở cuối hàng đợi
  int rear(Queue queue);
  
  //Giải phóng bộ nhớ
  void queue_free(Queue *queue);
  
  // Hiển thị toàn bộ hàng
  void display(Queue queue);
  
  #endif
  ```
- **Circular.c**
  ```c
  #include "circular_queue.h"

  //Khởi tạo những thông số ban đầu
  void queue_init(Queue *queue, int newSize)
  {
      queue->items = (int*)malloc(newSize*sizeof(int));
      queue->size = newSize;
      queue->front = queue->rear = -1;
  }
  
  //Kiểm tra hàng đợi rỗng
  bool queue_isEmpty(Queue queue)
  {
      return (queue.front == -1) ? true : false;
  }
  
  //Kiểm tra hàng đợi đầy
  bool queue_isFull(Queue queue)
  {
      return (queue.front == (queue.rear + 1) % queue.size) ? true : false;
  }
  
  //Thêm phần tử 
  void enqueue(Queue *queue, int value)
  {
      if(queue_isFull(*queue))
      {
          printf("Hang doi day!\n");
      }
      else
      {
          if(queue->front == -1)
          {
              queue->front = queue->rear = 0;
          }
          else
          {   
              queue->rear = (queue->rear + 1) % queue->size;  
          }
          queue->items[queue->rear] = value;
          printf("Enqueue: %d\n", value);
      }
  }
  
  //Xóa phần tử ở đầu hàng đợi
  int dequeue(Queue *queue)
  {
      if(queue_isEmpty(*queue))
      {
          printf("Hang doi rong\n");
          return -1;
      }
      else
      {
          int dequeue_value = queue->items[queue->front];
          queue ->items[queue->front] = 0;
          if (queue->front == queue->size - 1)    //kiểm tra đang ở hàng đợi cuối
          {
              queue->front = 0;    // reset về đàu
          }
          else
          {
              queue->front++;    // nếu không thì cộng 1
          }
          printf("Dequeue: %d\n", dequeue_value);
          return dequeue_value;
      }
  }
  
  //Đọc giá trị phần tử ở đầu hàng đợi
  int front(Queue queue)
  {
      if(queue_isEmpty(queue))
      {
          printf("Hang doi rong\n");
          return -1;
      }
      return queue.items[queue.front];
  }
  
  //Đọc giá trị phần tử ở cuối hàng đợi
  int rear(Queue queue)
  {
      if(queue_isEmpty(queue))
      {
          printf("Hang doi rong\n");
          return -1;
      }
      return queue.items[queue.rear];
  }
  //Giải phóng bộ nhớ
  void queue_free(Queue *queue)
  {   
      if( queue->items != NULL)
      {
          free(queue->items);
          queue->items =NULL;
      }
  }
  // Hiển thị toàn bộ hàng
  void display(Queue queue)
  {
      if(queue_isEmpty(queue))
      {
          printf("Hang doi rong\n");
      }
      else
      {
          printf("Queue: ");
          if(queue.front < queue.rear )
          {
              for (int i = queue.front; i <= queue.rear; i++)
              {
              printf("%d ", queue.items[i]);
              }
          }
          else
          {
              for (int i = queue.front; i < queue.size; i++)
              {
                  printf("%d ", queue.items[i]);
              }       
              for (int i = 0; i <= queue.rear; i++)
              {
                  printf("%d ", queue.items[i]);
              }
          }
          printf("\n");
      }
  } 
  ```
- **main.c**
  ```c
  #include "circular_queue.h"

  int main()
  {
      Queue queuel;
  
      //Khởi tạo hàng đợi
      queue_init(&queuel, 5);
  
      //Thêm phần tử hàng đợi
      enqueue(&queuel, 1);
      enqueue(&queuel, 2);
      enqueue(&queuel, 3);
      enqueue(&queuel, 4);
      enqueue(&queuel, 5);
      printf("\n");
  
      // In phần tử đàu (front) và cuối (rear)
      printf("Front: %d\n", front(queuel));
      printf("Rear: %d\n", rear(queuel));
      printf("\n");
  
      //Xóa phần tử đầu
      dequeue(&queuel);
      dequeue(&queuel);
      
      
      //Hiện thi phần tử trong hàng đợi
      display(queuel);
  
      printf("\n");
      enqueue(&queuel, 7);
      enqueue(&queuel, 8);
      dequeue(&queuel);
      dequeue(&queuel);
      enqueue(&queuel, 9);
      enqueue(&queuel, 10);
      printf("\n");
      display(queuel);
      printf("\n");
  
      return 0;
  }
  ```
- **Kết quả:**
  
![image](https://github.com/user-attachments/assets/af537df0-18a2-45e0-b479-662bc4f4a1bc)

 </details>
 
-----------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 10: Linked List</b></summary>
 
 ## 1. Khái  niệm
- **Linked list** :là một cấu trúc dữ liệu trong lập trình máy tính, được sử dụng để tổ chức và lưu trữ dữ liệu.
- là danh sách liên kết gồm tập hợp nhiều node liên kết với nhau
- Một linked list bao gồm một chuỗi các "nút" (nodes), mỗi nút chứa 2 thành phần:<br>
&nbsp;+ Giá trị dữ liệu.<br>
&nbsp;+ Con trỏ chứa địa chỉ của node tiếp theo trong chuỗi.

![image](https://github.com/user-attachments/assets/b598c8d2-4358-4885-b3cf-2c4800ff217a)

## 2.Xây dựng thư viện linker list
- **linker_list.h**
  ```c
  #ifndef LIST_H
  #define LIST_H
  
  #include <stdio.h>
  #include <stdbool.h>
  #include <stdlib.h>
  #include <stdint.h>
  
  typedef struct Node
  {
      int data;   // 4 bytes + 4 padding
      struct Node *next; // 8 bytes
  } Node;
  
  //1. Khai báo kiểu dữ liệu
  //2. Thành phần đầu tiên
  //3. Thành phần thứ 2
  //4. Xác định được hoàn toàn Node là gì
  
  
  void create_node(Node *mode, int newData);      // Cách khởi tạo 1
  Node* create_node1(int newData);                // Cách khỏi tạo 2
  
  void push_back(Node *head, int value);         // thêm 1 node vào cuối list
  void push_front(Node **head, int value);       // thêm 1 node vào đầu list
  void insert(Node **head, int value, int pos);  // thêm 1 node vào vị trí bất kỳ
  
  void pop_back(Node **array);                    // xóa 1 node cuối list
  void pop_front(Node **array);                   // xóa 1 node đầu list
  void erase(Node **array, int pos);              // xóa 1 node vị trí bất kỳ
  void clear(Node **head);                        // xóa toàn bộ node
  
  int front(Node *array);                         // đọc giá trị node đầu tiên
  int back(Node *array);                          // đọc giá trị node cuối cùng
  int get(Node *array, int pos);                  // đọc giá trị node bất kỳ
  
  int size(Node *head);                          // đếm số lượng node hiện tại
  bool empty(Node *array);                        // kiểm tra list rỗng 
  
  //vì không giới hạn node lên không kiểm tra đầy
  
  void display(Node *head);                       // Hiển thị node
  
  #endif
  ```
- **linker_list.c**
  ```c
  #include "linker_list.h"

  //Cách 1
  //Khởi tạo thông tin và thông tin vào biến cục bộ
  void create_node(Node *node, int newData) // Cách khỏi tạo 1
  {
      node->next = NULL;
      node->data = newData;
  }
  
  //Cách 2
  //Cấp phát động 1 vùng nhớ và đẩy thông tin vào vùng nhớ
  //khi sử dụng sẽ khai báo con trỏ để trỏ đến vùng nhớ được cấp phát
  Node* create_node1(int newData)   // Cách khỏi tạo 2
  {
      Node *node = (Node*)malloc(sizeof(Node));
      node->next = NULL;
      node->data = newData;
      return node;
  }
  
  
  //Thêm node vào đầu list:
  //B1: Tạo con trỏ trỏ tới node đầu tiền
  //B2: Duyệt qua từng node
  //B3: Tự khởi tạo chỉ số
  
  void push_front(Node **head, int value)
  {
     Node *new_node = create_node1(value);
      if(*head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          *head = new_node;   //Rỗng thì thêm node mới
      }
      else
      {
          new_node->next = *head;   //Thay đổi con trỏ node->next trong node mới để trỏ tới địa chị hiện tại node đầu
          *head = new_node;         //Thay đổi con trỏ node đầu tiên trỏ tới node mới  
      }
  }
  
  
  //Thêm node vào cuối list:
  //B1: Tạo con trỏ trỏ tới node cuối  
  //B2: Duyệt qua từng node
  //B3: Tự khởi tạo chỉ số
  
  void push_back(Node *head, int value)
  {
     Node *new_node = create_node1(value);
      if(head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          head = new_node;   //Rỗng thì thêm node mới
      }
      else
      {   
          Node* temp = head;
          while((temp)->next != NULL)
          {
              temp = (temp)->next;       //duyệt con trỏ trỏ đến node cuối
          }
          (temp)->next = new_node;        //cho next trỏ đến địa chỉ mới
      }
  }
  
   // kiểm tra list rỗng
  bool empty(Node *head)
  {
      return (head == NULL)?true:false;
  }
  
  //Tạo biến đếm
  int size(Node *head)
  {
      unsigned int count = 0;
  
      if(head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          return 0;
      }
      else
      {
          while(head != NULL)
          {
              count ++;
              head = head->next;
          } 
      }
      return count;
  }
  
  //Chèn 1 node vào vị trí bất kỳ
  void insert(Node **head, int value, int pos)
  {
      Node *new_node = create_node1(value);
      if(*head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          *head = new_node;   //Rỗng thì thêm node mới
      }
      else
      {   
          //vị trí đầu list
          if(pos == 0)
          {
              push_front(head, value);
          }
          //Vị trí cuối list
          else if(pos >= size(*head)-1)
          {
              push_back(*head, value);
          }
          //Vị trí bất kỳ
          else
          {
              Node *temp = *head;
              uint8_t index = 0;
  
              while(temp != NULL && index != pos - 2) //Tìm vị trí sẽ trừ đi 2 đơn vị
              {
                  index++;
                  temp = temp->next;
              }
  
              if(index == pos -2)
              {
                  new_node->next = temp->next;
                  temp->next = new_node;
              }
          }
      }
  }
  
  // xóa 1 node cuối list
  void pop_back(Node **head)
  {
      Node *new_node = *head;
      if(*head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          return ;   
      }
      else
      {    
          while(new_node->next->next != NULL)   // Tìm node sau node kế tiếp của new_node
          {
              new_node = new_node->next;
          }
          free(new_node->next);
          new_node->next = NULL;
      }
  }  
   
   // xóa 1 node đầu list
  void pop_front(Node **head)
  {   
      if(*head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          return ;   
      }
      else
      {    
          Node *new_node = (*head)->next;   //tạo nốt mới trỏ dến node thứ 2
          free(*head);                      //Xóa node đầu
          *head = new_node;                 //Gắn con trỏ trỏ đến node thứ 2
      }
  }
  
  // xóa 1 node vị trí bất kỳ                  
  void erase(Node **head, int pos)
  {   
      if(*head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          return ;   
      }
      else
      {
          //vị trí đầu list
          if(pos == 0)
          {
              pop_front(head);
          }
          //Vị trí cuối list
          else if(pos >= size(*head)-1)
          {
              pop_back(head);
          }
          //Vị trí bất kỳ
          else
          {
              Node *temp = *head;
              uint8_t index = 0;
  
              while (temp != NULL && index != pos - 2) //Tìm vị trí sẽ trừ đi 2 đơn vị
              {
                  index++;
                  temp = temp->next;
              }
  
              if(index == pos -2)
              {  
                  Node *new_node = temp->next;
                  temp->next = new_node->next;
                  free(new_node);
              }
          }
      }      
  }
  
  // xóa toàn bộ node
  void clear(Node **head)
  {
      if(*head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          return ;   
      }
      else
      {   
          Node *new_node;
          while (*head != NULL)
          {
              new_node = (*head)->next;
              free(*head);
              *head = new_node;
          }
      }
  }
  
  // đọc giá trị node đầu tiên
  int front(Node *head)
  {
      if(head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          printf("Khong co node!\n"); 
          return -1; 
      }
      else
      {
          return head->data;
      }
  } 
  
  // đọc giá trị node cuối cùng
  int back(Node *head)
  {
      if(head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          printf("Khong co node!\n");
          return -1;
      }
      else
      {   
          Node *new_node = head;
          while(new_node->next != NULL)   // Tìm node sau node kế tiếp của new_node
          {
              new_node = new_node->next;
          }
          return new_node->data;
      }
  }
  
   // đọc giá trị node bất kỳ                          
  int get(Node *head, int pos)                 
  {   
      int value;
      if(head == NULL)   //Kiểm tra trạng thái hiện tại rỗng không
      {
          printf("Khong co node!\n");
          return -1;
      }
      else
      {
          //vị trí đầu list
          if(pos == 0)
          {
              value = front(head);
          }
          //Vị trí cuối list
          else if(pos >= size(head)-1)
          {
              value = back(head);
          }
          //Vị trí bất kỳ
          else
          {
              Node *new_node = head;
              uint8_t index = 0;
  
              while (new_node != NULL && index != pos - 1) //Tìm vị trí sẽ trừ đi 2 đơn vị
              {
                  index++;
                  new_node = new_node->next;
              }
  
              if(index == pos -1)
              {  
                  value =  new_node->data;
              }
          }
          return value;
      }      
  }
  
  // Hiển thị các node
  void display(Node *head)
  {
      int index = 0;
  
      if (head == NULL)
      {
          printf("Khong co node!\n");
      }
      else
      {
          while(head != NULL)
          {
              printf("Node %d: %d\n", index, head->data);
              head = head->next;
              index++;
          }
      }
  }
  ```
- **main,c**
  ```c
  #include "linker_list.h"
  
  int main()
  {
      //Cách khởi tạo 1
      //Node node1, node2;
      //create_node(&node1, 5);
  
      //Cách khởi tạo 2
      Node *node1 = create_node1(5);
      Node *node2 = create_node1(6);
      Node *node3 = create_node1(7);
  
      node1->next = node2;
      node2->next = node3;
  
      display(node1);
      
      printf("\n");
      push_front(&node1, 10);
      push_back(node1, 20);
      display(node1);
      
      printf("\n");
      insert(&node1, 30, 0);
      insert(&node1, 50, 3);
  
      display(node1);
      
      printf("\n");
      pop_back(&node1);
      display(node1);
  
      printf("\n");
      pop_front(&node1);
      display(node1);
  
      printf("\n");
      erase(&node1, 2);
      display(node1);
  
      int value;
      printf("\n");
      value = front(node1);
      printf("Node dau: %d\n", value);
      value = back(node1);
      printf("Node cuoi: %d\n", value);
      value = get(node1, 2);
      printf("Node : %d\n", value);
  
      printf("\n");
      clear(&node1);
      display(node1);
      
      return 0;
  }
  ```                                         
 </details>
 
-----------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 11: JSON</b></summary>
 
## 1. Khái niệm
- **JSON ("JavaScript Object Notation" - Ghi chú về Đối tượng JavaScript)**. Đây là một **định dạng truyền tải dữ liệu (định dạng là cách lưu trữ dữ liệu)** phổ biến trong lập trình và giao tiếp giữa các máy chủ và trình duyệt web, cũng như giữa các hệ thống khác nhau.
- JSON được thiết kế để dễ đọc và dễ viết cho con người, cũng như dễ dàng để phân tích và tạo ra cho máy tính. Nó sử dụng một cú pháp nhẹ dựa trên cặp **key - value**, tương tự như các đối tượng và mảng trong JavaScript. 
- Có 2 định dạng JSON:<br>
&nbsp;+ **Định dạng theo kiểu đối tượng** là **tập hợp các cặp "key" và "value"**.<br>
&nbsp;+ **Định dạng theo mảng** là ngoài **tập hợp các cặp "key" và "value"** nó còn tập hợp các giá trị khác như con số, chuỗi, struct,... => Nó là **một tập hợp các giá trị**.
- Ứng dụng: khi mà thông tin mỗi đối tượng là khác nhau và ta không thể dùng struct cho trường hợp đó
## 2. Các định dạng
## 2.1. Object - Đối tượng
- **key:** là dạng chuỗi
- **value:** Không bắt buộc là chuỗi
- Ví dụ:
  ```c
  { 
    "name": "John Doe",
    "age": 30,
    "city": "New York",
    "isStudent": false,
    "grades": [85, 90, 78]
  }
  ```
 &nbsp;+ Các **key:** "name", "age", "city", "isStudent","grades".<br>
 &nbsp;+ Các **value:** "John Doe", 30, "New York", false, [85, 90, 78].<br>
 &nbsp;+ Các cặp **"key" và "value"** ngăn cách nhau bởi dấu ",".<br>
## 2.2. Mảng JSON
|**Mảng thường**|**Mảng JSON**|
|----------------------|------------------------|
|- Các phần tử nằm bao bọc trong cặp ngoặc nhọn.|- Các phần tử nằm bao bọc trong cặp ngoặc vuông.|
|- Các phần tử giống nhau về kiểu dữ liệu.|- Các phần tử không nhất thiết phải giống nhau về kiểu dữ liệu.|
- Ví dụ:
```c
[
  // Phần tử thứ 1 là 1 đối tượng
  {
    "name": "John Doe",
    "age": 30,
    "city": "New York",
    "occupation": "Software Engineer",
    "isStudent": false
  },
    // Phần tử thứ 2 là 1 đối tượng
  {
    "name": "Jane Smith",
    "age": null,
    "city": "Los Angeles",
    "contact": {
      "email": "jane.smith@example.com",
      "phone": "555-1234"
    }
  },
 // Phần tử thứ 3 là 1 đối tượng
  {
    "name": "Bob Johnson",
    "age": 35,
    "city": "Chicago"
  },
  // Các phần tử kiểu khác: kiểu số nguyên, số thực, chuỗi, boolean,...
  20, 3.14, "Hello World", true, null, [80, 70, 90]
]
```
## 3. Xử lý JSON
- Dấu ```\"``` dùng cho bắt đầu và kết thúc để nhận biết chuỗi con trong chuỗi lớn.<br>
&nbsp;+ ví dụ:
```c
const char *str = "\"key1\": \"value\"";  //key1 và value1 là chuỗi con trong chuỗi lớn
```
- Kí tự ```"{"``` và ```"}"``` là kí tự bắt đầu và kết thúc chuỗi JSON.<br>
&nbsp;+ ví dụ:
```c
const char *json_str = "{"
                          "\"1001\":{"
                          "\"SoPhong\": 3,"
                          "\"NguoiThue\":{"
                               "\"Ten\":\"Nguyen Van A\","
                               "\"CCCD\":\"1920517781\","
                               "\"Tuoi\\\":26,"
                               "\"ThuongTru\\\":{"
                               "\"Duong\":\"73 Ba Huyen Thanh Quan\","
                               "\"Phuong_Xa\":\"Phuong 6\","
                               "\"Tinh_TP\":\"Ho Chi Minh\""
                               "}"
                          "},"
                          "\"SoNguoiO\":{"
                               "\"1\":\"Nguyen Van A\","
                               "\"2\":\"Nguyen Van B\","
                               "\"3\":\"Nguyen Van C\""
                          "},"
                       "}"
                          
```
- Dữ liệu dù tách ra nhiều dòng nhưng không có ngăn cách trình biên dịch vẫn tự hiểu đây là 1 chuỗi và tự gộp cho mình
 </details>
 
-----------------------------------------------------------------------------------------------------------------------------------------------
<details>
 
<summary><b>📖BÀI 12: Bubble Sort, Binary Search, Binary Search Tree</b></summary>

## 1. Bubble Sort - Sắp xếp nổi bọt
- Thuật toán **sắp xếp nổi bọt (Bubble Sort)** hoạt động dựa trên nguyên tắc hoán đổi các phần tử liền kề để đưa phần tử lớn hơn về cuối dãy (hoặc phần tử nhỏ hơn về đầu dãy)
- Thuật toán gồm các bước sau:<br>
&nbsp;1. Duyệt qua danh sách từ đầu đến cuối.<br>
&nbsp;2. So sánh hai phần tử liền kề, nếu phần tử trước lơn hơn phần tử sau, thì hoán đồi vị trí.<br>
&nbsp;3. Lặp lại quá trình cho đến khi không còn sự hoán đổi nào xảy ra (mảng đã được sắp xếp).<br>
- Đặc điểm:
&nbsp;+ Số lượt = số phần tử - 1.<br>
&nbsp;+ Số lần giao hoán: số phần tử - i - 1 (i: số lượt lặp).<br>  
- Ví dụ:
  ```c
  void bubbleSort(int arr[], int n)
  {
      for (int i=0; i<=n-2; i++)
      {
          for (int j=0; j<=n-i-2; j++)
          {
              if (arr[j] > arr[j+1])
              {
                  int temp = arr[j]; 
                  arr[j]   = arr[j+1];
                  arr[j+1] = temp;
              }
          }
      }  
  }
  ```
## 2. Linear Search - Thuật toán tìm kiếm tuyến tính
- **Thuật toán tìm kiếm tuyến tính (Linear Search)** là phương pháp đơn giản nhất để tìm kiếm một phần tử trong mảng.
- Nguyên tắc hoạt động:<br>
&nbsp;1. Duyệt từng phần tử trong mảng từ trái sang phải.<br>
&nbsp;2. Nếu phần tử đang xét trùng với giá trị cần tìm, trả về vị trí của nó.<br>
&nbsp;3. Nếu duyệt hết mảng mà không tìm thấy, trả về kết quả không tồn tại.<br>
- Ưu điểm: Dễ thực hiện.
- Nhược điểm: Vừa phải duyệt vừa phải so sánh tìm nếu mảng có số lượng phần tử lớn thì tốc độ sẽ chậm vì số lượng phần tử lớn  
## 2. Binary Search - Thuật toán tìm kiếm nhị phân
- Thuật toán tìm kiếm nhị phân (Binary Search) hoạt động bằng cách chia đôi mảng để tìm kiếm, thay vì duyệt tuần tự như Linear Search.
- Nguyên tắc hoạt động:<br>
&nbsp;1. Sắp xếp mảng (tăng dần hoặc giảm dần).<br>
&nbsp;2. So sánh phần tử ở giữa mảng với giá trị cần tìm:<br>
&nbsp;&nbsp;&nbsp;+ Nếu trùng → Trả về vị trí.<br>
&nbsp;&nbsp;&nbsp;+ Nếu phần tử giữa mảng nhỏ hơn 	→ Tiếp tục tìm trong nửa phải.<br>
&nbsp;&nbsp;&nbsp;+ Nếu phần tử giữa mảng lớn hơn 	→ Tiếp tục tìm trong nửa trái.<br>
&nbsp;3. Lặp lại bước 2 cho đến khi tìm thấy phần tử hoặc không còn phần tử nào để tìm.<br>
- Ví dụ:
  ```c
  #define NO_FOUND -1    
  
  int binarySearch(int* arr, int l, int r, int x)
  {
      if (r >= l)
      {
          int mid = (r + l) / 2;
  
          if (arr[mid] == x){
              return mid;
          }
          else if (arr[mid] > x){
              return binarySearch(arr, l, mid - 1, x);
          }
          else{
              return binarySearch(arr, mid + 1, r, x);
          }
      }
      return NO_FOUND;
  }
  ```
## 3. Binary Search Tree - Cây tìm kiếm nhị phân
- **Cấu trúc dữ liệu phân cấp (Tree)** là một **cấu trúc dữ liệu phi tuyến tính**, trong đó các phần tử (được gọi là nút, hay node) được tổ chức theo một thứ bậc phân cấp.
- Cây là một trong những cấu trúc dữ liệu quan trọng, được sử dụng rộng rãi trong khoa học máy tính để biểu diễn các quan hệ phân cấp, tìm kiếm, sắp xếp, và lưu trữ.
  
![image](https://github.com/user-attachments/assets/e4aaca32-2f03-4e0a-b2fd-6797811e1246)

- Nốt gốc: i - Phải là phần tử chính giữa trong mảng
- chỉ số node nhánh trái: 2i + 1
- chỉ số node nhánh phải: 2i + =-0
- **Cây Tìm Kiếm Nhị Phân (BST - Binary Search Tree)** là một cấu trúc dữ liệu dạng cây, trong đó:<br>
&nbsp;+ Mỗi nút có tối đa 2 con (gọi là cây con trái và cây con phải).<br>
&nbsp;+ Dữ liệu trong cây tuân theo quy tắc:<br>
&nbsp;&nbsp;&nbsp;* Nút con trái chứa giá trị nhỏ hơn nút gốc.<br>
&nbsp;&nbsp;&nbsp;* Nút con phải chứa giá trị lớn hơn nút gốc.<br>
&nbsp;&nbsp;&nbsp;* Quy tắc này áp dụng đệ quy cho toàn bộ cây.<br>
- Các bước xây dựng dạng cây:.<br>
&nbsp;+ Sắp xếp mảng theo thứ tự tăng dần.<br>
&nbsp;+ Tìm phần tử ở giữa.<br>
## 4. File operations
- **File CSV (Comma-Separated Values)** là một loại file văn bản được sử dụng để lưu trữ và truyền tải dữ liệu có cấu trúc dưới dạng bảng, trong đó các dữ liệu của các cột được phân tách bằng dấu phẩy (,) hoặc một ký tự ngăn cách khác.
- Để mở một file, bạn có thể sử dụng hàm fopen(). Hàm này trả về một con trỏ FILEm trỏ đến file đó, và cần được kiểm tra để đảm bảo file đã mở thành công.
  ```c
  FILE *file = fopen(const char *file_name, const char *access_mode);
  + const char *file_name: Địa chỉ và tên file
  + const char *access_mode: chế độ như đọc, ghi
  ```
- Các chế độ

|**Chế độ**|**Mô tả**|
|:----------------------:|------------------------|
|r|Mở file với chế độ chỉ đọc file. Nếu mở file thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không thì trả về NULL.|
|rb|Mở file với chế độ chỉ đọc file theo định dạng binary. Nếu mở file thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không thì trả về NULL.|
|w|Mở file với chế độ ghi vào file. Nếu file đã tồn tại, thì sẽ ghi đè vào nội dung bên trong file. Nếu file chưa tồn tại thì sẽ tạo một file mới. Nếu không mở được file thì trả về NULL.|
|wb|Mở file với chế độ ghi vào file theo định dạng binary. Nếu file đã tồn tại, thì sẽ ghi đè vào nội dung bên trong file. Nếu file chưa tồn tại thì sẽ tạo một file mới. Nếu không mở được file thì trả về NULL.|
|a|Mở file với chế độ nối. Nếu mở file thành công thì trả về địa chỉ của phần tử cuối cùng trong file. Nếu file chưa tồn tại thì sẽ tạo một file mới. Nếu không mở được file thì trả về NULL.|
|ab|Mở file với chế độ nối dưới định dạng binary. Nếu mở file thành công thì trả về địa chỉ của phần tử cuối cùng trong file. Nếu file chưa tồn tại thì sẽ tạo một file mới. Nếu không mở được file thì trả về NULL.|
|r+|Mở file với chế độ đọc và ghi file. Nếu mở file thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không thì trả về NULL.|
|rb+|Mở file với chế độ đọc và ghi file dưới định dạng binary. Nếu mở file thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không thì trả về NULL.Mở file với chế độ đọc và ghi file dưới định dạng binary. Nếu mở file thành công thì trả về địa chỉ của phần tử đầu tiên trong file, nếu không thì trả về NULL.|
|w+|Mở file với chế độ ghi và đọc file. Nếu file đã tồn tại thì trả về địa chỉ của phần tử đầu tiên của file. Nếu file chưa tồn tại thì sẽ tạo một file mới.|
|wb+|Mở file với chế độ ghi và đọc file dưới định dạng binary. Nếu file đã tồn tại thì trả về địa chỉ của phần tử đầu tiên của file. Nếu file chưa tồn tại thì sẽ tạo một file mới.|
|a+|Mở file với chế độ nối và đọc file. Nếu file đã tồn tại thì trả về địa chỉ của phần tử cuối cùng của file. Nếu file chưa tồn tại thì sẽ tạo một file mới.|
|ab+|Mở file với chế độ nối và đọc file dưới định dạng binary. Nếu file đã tồn tại thì trả về địa chỉ của phần tử cuối cùng của file. Nếu file chưa tồn tại thì sẽ tạo một file mới.|
- Các thao tác với file
  |**Tên hàm**|**Mô tả**|
  |:----------------------:|:----------------------|
  |fscanf()|Sử dụng chuỗi được định dạng và danh sách đối số biến để lấy đầu vào từ một File|
  |fgets()|Copy nội dung trong File vào mảng dùng để lưu trữ với tối đa số lượng phần tử của mảng hoặc tới khi gặp ký tự xuống dòng.|
  |fgetc()|Lấy giá trị tại địa chỉ hiện tại của file, sau đó di chuyển tới địa chỉ tiếp theo. Kiểu trả về là char |
  |fread()|Đọc một số lượng byte được chỉ định từ File .bin|
  |fprintf()|Ghi chuỗi vào File, và có thể thêm danh sách các đối số|
  |fputs()|Ghi chuỗi vào File|
  |fputc()|Ghi một ký tự vào File|
  |fwrite()|Ghi một số byte được chỉ định vào File .bin|
  |fclose()|Đọc một số lượng byte được chỉ định từ File .bin|
  |feof()|Để kiểm tra địa chỉ hiện tại có phải ký tự cuối cùng của File hay chưa| 
 </details>
 
-----------------------------------------------------------------------------------------------------------------------------------------------
