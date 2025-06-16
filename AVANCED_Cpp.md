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
&nbsp;+ Public: Có thể truy cập từ bên ngoài và bên trong.<br>
&nbsp;+ Private: Không thể truy cập từ bên ngoài.<br>
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
## 3. Constructor
- **Constructor** trong C++ là một method (hàm) sẽ được tự động gọi khi khởi tạo object.
- **Đặc điểm:** <br>
&nbsp;+ Constructor sẽ có tên trùng với tên của class.<br>
&nbsp;+ Constructor không có kiểu trả về.<br>
&nbsp;+ Tự động gọi khi mình khởi tạo class. <br>
&nbsp;+ Thường dùng để khởi tạo giá trị ban đầu cho các biến trong class.<br>
- **Các dạng Constructor:** <br>
&nbsp;+ Constructor không tham số.<br>
&nbsp;+ Có tham số nhưng không có giá trị mặc định => khi khai báo object phải có kèm tham số.<br>
&nbsp;+ Có tham số nhưng có giá trị mặc định => Khi khởi tạo object không cần thiết truyền tham số. <br>
&nbsp;+ initialization list - danh sách khởi tạo.<br>
- Ví dụ:
```c
  class User
  {
      public:
          int a;     // trong C++ a, b, c là thuộc tính ( property)
          double b;
          char c;

          // Constructor có tham số nhưng không có giá trị mặc định 
          User(int _a,double _b,char _c) 
          {
            a = _a;
            b = _b;
            c = _c;
          }

          /* Constructor có tham số có giá trị mắc định
          User(int _a = 2,double _b = 4,char _c = 'd') 
          {
            a = _a;
            b = _b;
            c = _c;
          }
          */

           /* Constructor danh sách khởi tạo (initialization list)
          User(int _a = 2,double _b = 4,char _c = 'd') 
               : a(_a), b(_b), (_c){}   // dấu : báo sẽ dùng danh sách khởi tạo
          */

          void create()    // trong class, create() gọi là phương thức(method)
          {
              User user1(int 1, 2, 3); //user1 không bị trùng tên vì nó nằm ở phạm vị khác (biến cục bộ trong class)
              
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
      User user1(int 1, 2, 4), user2(int 2, 3, 4);  // Với constructor có tham số nhưng không có giá trị mắc định thì khi khai báo phải có kèm tham số
  
      user1.a = 10;
      user1.b = 20.5;
  
      user1.display();
      user1.create();
      return 0;
  }
```
## 4. Destructor
- Destructor trong C++ là một method sẽ được tự động gọi trước khi object được giải phóng.
- Destructor sẽ có tên trùng với tên của class và thêm ký tự ~ ở phía trước tên.
- Không được phép viết tham số
- Thường dùng để xóa dữ liệu biến
- Ví dụ:
```c
  class User
  {
      public:
          int a;     // trong C++ a, b, c là thuộc tính ( property)
          double b;
          char c;

           //Constructor danh sách khởi tạo (initialization list)
          User(int _a = 2,double _b = 4,char _c = 'd') 
               : a(_a), b(_b), (_c){}   // dấu : báo sẽ dùng danh sách khởi tạo

          // Destructor
          ~User() //Không có tham số
          {
            count << "This is destructor\n";
          }

          void create()    // trong class, create() gọi là phương thức(method)
          {
              User user1 //user1 không bị trùng tên vì nó nằm ở phạm vị khác (biến cục bộ trong class)
              
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

    void display1()   
  {
     cout << a << endl;
     cout << b << endl;
  }
  
  void User::display1()    //khi muốn khai báo 1 hàm ngoài class ta dùng toán tử ::
  {
     cout << a << endl;  
     cout << b << endl;
  }

  int main()
  {
      User user1, user2;  
  
      user1.a = 10;
      user1.b = 20.5;
  
      user1.display();
      user1.create();
      return 0;
  }
```
## 5. Static property
- Khi một property trong class được khai báo với từ khóa static, thì tất cả các object sẽ dùng chung địa chỉ của property này
```c
  class User
  {
      public:
          int a;     // trong C++ a, b, c là thuộc tính ( property)
          double b;
          char c;

          static int x; 

  };

// Do x khai báo static nên vùng nhớ ban đầu sẽ chưa tạo ra cần phải khởi tạo cho nó và khởi tạo ngoài main và class
int User::x = 0; // Khỏi tạo cho biến x

int main()
  {
      User user1, user2;  
  
      // user1.x, user2.x  cùng chung địa chỉ vì x khai báo static
      user1.x =100;
      user2.x =200;
      return 0;
  }
```
## 6. Static method - Hàm static
- Khi một method trong class được khai báo với từ khóa static:
&nbsp;+ Method này độc lập với bất kỳ đối tượng nào của lớp.
&nbsp;+ Method này có thể được gọi ngay cả khi không có đối tượng nào của class tồn tại.
&nbsp;+ Method này có thể được truy cập bằng cách sử dụng tên class thông qua toán tử :: 
&nbsp;+ Method này có thể truy cập các static property và các static method bên trong hoặc bên ngoài class.
&nbsp;+ Method có phạm vi bên trong class và không thể truy cập con trỏ đối tượng hiện tại.
- ví dụ:
```c
  class User
  {
      public:
          int a;     // trong C++ a, b, c là thuộc tính ( property)
          double b;
          char c;

          static int x; 

          static void test()
          {
            cout << a << b << c; //báo lỗi vì Method có phạm vi bên trong class và không thể truy cập con trỏ đối tượng hiện tại
            cout << x; // Method này có thể truy cập các static property và các static method bên trong hoặc bên ngoài class.
          }
  };

int User::x = 0; // Khỏi tạo cho biến x

int main()
  {
      User user1, user2;  
  
      // user1.x, user2.x  cùng chung địa chỉ vì x khai báo static
      user1.x =100;
      user2.x =200;

      User :: test(); // Gọi trực tiếp bằng class không thông qua object
      
      return 0;
  }
```
 </details>
 
-----------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 2: OPP</b></summary>

## 1. Tính đóng gói
- **Tính đóng gói (Encapsulation)** là ẩn đi các property “mật” khỏi người dùng. Và để làm được điều này, ta sẽ khai báo các property ở quyền truy cập **private/protected** (tức là không thể truy cập trực tiếp tới các property này thông qua object bên ngoài).
- Trong trường hợp ta muốn đọc hoặc ghi các property này, thì ta sẽ truy cập gián tiếp thông qua các method ở quyền truy cập public.
- Ví dụ:
```c
#include <iostream>
#include <string>
using namespace std;

class SinhVien{
    private:
        string name;    // tính đóng gói
        int age;        // tính đóng gói
        int id;         // tính đóng gói
};
```
## 2. Tính trừu tượng
- **Tính trừu tượng** đề cập đến việc ẩn đi các chi tiết cụ thể của một đối tượng và chỉ hiển thị những gì cần thiết để sử dụng đối tượng đó ( ẩn đi các hàm). Và để làm được điều này, ta sẽ khai báo các method ở quyền truy cập private/protected.
- Ví dụ:
```c
#include <iostream>
#include <string>
using namespace std;

class SinhVien{
    private:
        string name;    // tính đóng gói
        int age;        // tính đóng gói
        int id;         // tính đóng gói

        // Hàm kiểm tra tên sinh viên có hợp lệ không
        bool checkName(string str)        // Tính trừu tượng
        {
            for (int i = 0; i < str.length(); i++)
            {
                char c = str[i];
                if (!((c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z') || (c == ' ')))
                {
                    return false;
                }
            }
            return true;
        }

        // Hàm kiểm tra tuổi sinh viên có hợp lệ không
        bool checkAge(int age)            // Tính trừu tượng
        {
            if (age <= 0) return false;
            return true;
        }

    public:
            SinhVien(){
                static int ID = 1;
                id = ID;
                ID++;
            }

            // setter method: Hàm để cài đặt dữ liệu
            void setName(string newName)
            {
                if (checkName(newName))
                {
                    cout << "Ten hop le!\n";
                    name = newName;
                }
                else
                {
                    cout << "Ten khong hop le!\n";
                    name = "";
                }
            }

            // setter method: Hàm để cài đặt dữ liệu
            void setAge(int newAge)
            {
                if (checkAge(newAge))
                {
                    cout << "Tuoi hop le!\n";
                    age = newAge;
                }
                else
                {
                    cout << "Tuoi hop le!\n";
                    age = 0;
                }
            }

            // getter method: Hàm để lấy dữ liệu
            string getName()
            {   
                return name;
            }

            // getter method: Hàm để lấy dữ liệu
            int getAge()
            {   
                return age;
            }

            // getter method: Hàm để lấy dữ liệu
            int getID()
            {
                return id;
            }
    
           // Hàm hiển thị
          void display()
          {
              cout << "Ten: " << getName() << endl;
              cout << "Tuoi: " << getAge() << endl;
              cout << "MSV: " << getID() << endl;
          }
};

int main()
{
    SinhVien sv1, sv2;

    sv1.setName("Trung");
    sv1.setAge(18);
    sv1.display();

    sv2.setName("Anh");
    sv2.setAge(20);
    sv2.display();

    return 0;
}

```
 </details>
