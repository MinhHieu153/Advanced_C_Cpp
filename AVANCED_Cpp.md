📓Advanced_Cpp📓
----

<details>
<summary><b>📖BÀI 1: CLASS</b></summary>

## 1. Khái niệm
- Trong C++, **"class"** được sử dụng để định nghĩa một lớp, là một cấu trúc dữ liệu tự định nghĩa có thể chứa biến và các hàm thành viên liên quan.
- **class  - lớp** là nền tảng của lập trình hướng đối tượng (OOP) trong C++.
- class không thể truy xuất biến thành viên ra
- Cú pháp: <br>
  ```cpp
  class ClassName
  {
    Access_specifier:    // Phạm vi truy cập. Nó quy định cách mình có thể truy xuất hoặc không truy xuất được các biến thành viên
    Data member;         // những biến thành viên
    Member funtions(){}  // hàm thành viên 
  };
  ```
|Class|Struct|
|:------------------------:|:------------------------:|
|- Chứa biến và các hàm thành viên liên quan|- Chỉ chứa kiểu dữ liệu|
|- Mắc định không truy suất biến thành viên|- Mắc định truy xuất biến thành viên|
## 2. Access_specifier - Phạm vi truy cập
- **Access_specifier - Phạm vi truy cập tronmg class** là cách quy định mức độ truy cập của các thành viên (biến và phương thức) trong một lớp.
- C++ cung cấp ba phạm vi truy cập chính:<br>
&nbsp;+ Public.<br>
&nbsp;+ Private.<br>
&nbsp;+ protected.<br>
- Mỗi phạm vi truy cập sẽ có đặc điểm riêng biệt và liên quan đến các tính chất hướng đối tượng khác nhau.
- Ví dụ:
  ```cpp                                      
  #include <iostream>
  using namespace std;
  
  /*
      Public: 
      + Truy cập từ bên ngoài class
      + Truy cập từ bên trong class
  */
  
  class User
  {
      public:
          int a;     // trong C++ a, b, c là thuộc tính ( property)
          double b;
          char c;
  
          void create()    // trong class, create() gọi là phương thức(method)
          {
              User user1; //user1 không bị trùng tên vì nó nằm ở phạm vị khác (biến cục bộ trong class)
              
              user1.a = 100;
              user1.b = 200.5;
  
              user1.display();
          }

           void display()        // định nghĩa hàm bên trong class
          {
              cout << a << endl;
              cout << b << endl;
          }
          void display1();  // phạm vi bị giới hạn trong class
  };

    void display1()    //khác hàm display1() trong class do phạm vi là hàm toàn cục
  {
     cout << a << endl;
     cout << b << endl;
  }
  
  void User::display1()    //khi muốn khai báo 1 hàm ngoài class ta dùng toán tử ::
  {
     cout << a << endl;  // Hàm nằm cùng phạm vi class với biến a, b, c nên ta có thể dùng được
     cout << b << endl;
  }

  int main()
  {
      User user1, user2;  // trong Class, user1, user2 gọi là đối tượng(biến cục bộ trong main)
  
      user1.a = 10;
      user1.b = 20.5;
  
      user1.display();
      user1.create();
      return 0;
  }
  ```  
## 3. Access_specifier - Phạm vi truy cập


  
 </details>

  
-----------------------------------------------------------------------------------------------------------------------------------------------
