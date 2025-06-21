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
&nbsp;+ protected: Không thể truy cập từ bên ngoài nhưng class bên trong và class kế thừa được phép truy cập.<br>
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
```cpp
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
          User(int _a = 2,double _b = 4,char _c = 'd') : a(_a), b(_b), (_c){}   // dấu : báo sẽ dùng danh sách khởi tạo
          */

          void create()    // trong class, create() gọi là phương thức(method)
          {
              User user1(1, 2, 3); //user1 không bị trùng tên vì nó nằm ở phạm vị khác (biến cục bộ trong class)
              
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
      User user1(1, 2, 4), user2(2, 3, 4);  // Với constructor có tham số nhưng không có giá trị mắc định thì khi khai báo phải có kèm tham số
  
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
```cpp
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
```cpp
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
```cpp
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
```cpp
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
```cpp
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
            // Constructor
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
## 3. Tính kế thừa
- **Tính kế thừa ( Inheritance)** là khả năng sử dụng lại các property và method của một class trong một class khác và có thể mở rộng thêm tính năng.
- Ta chia chúng làm 2 loại:<br>
&nbsp;+ **class cha (base class)**: Chứa thông tin class khác muốn sử dụng lại.  
&nbsp;+ **class con (derived class)**: Sử dụng lại thông tin class có sẵn.<br>
- Để kế thừa từ class khác, ta dùng ký tự ":".
- Có 3 kiểu kế thừa là public, private và protected. Những property và method được kế thừa từ class cha sẽ nằm ở quyền truy cập của class con tương ứng với kiểu kế thừa.
- **protected**: Những biến và hàm nằm ở phạm vi này sẽ không thể truy cập từ đối tượng bên ngoài nhưng có thể truy cập trực tiếp từ các class bên trong và class kế thừa
- Phạm vị:<br>
&nbsp;+ Đối với kế thừa theo kiểu Public: Giữ nguyên toàn bộ những đặc tính ban đầu nó có thể kế thừa được gồm **protected** và **public** (**private** không thể kế thừa được vì nó không cho phép truy xuất từ bên ngoài).<br>
&nbsp;+ Đối với kế thừa theo kiểu protected: Nó sẽ kế thừa được **protected** và **public** nhưng nó sẽ thay đổi tính chất nó kế thừa và chuyền về **protected**.<br>
&nbsp;+ Đối với kế thừa theo kiểu private: Nó sẽ kế thừa được **protected** và **public** nhưng nó sẽ thay đổi tính chất nó kế thừa và chuyền về **private**.<br>
- **Đa kế thừa** là 1 class kế thừa từ nhiều class khác
- Ví dụ:
```cpp
#include <iostream>
#include <string>
using namespace std;

class DoiTuong
{
    /* protected: 
        - Những biến và hàm nằm ở phạm vi này sẽ không thể truy cập 
        từ đối tượng bên ngoài nhưng có thể truy cập trực tiếp từ các class 
        bên trong và class kế thừa
     */
    protected:
        string name;
        int age;
        int id;

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
        DoiTuong()          // Constructor
        {
            static int ID = 1;
            DoiTuong::id = ID;
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

class SinhVien : public DoiTuong  // 1 class sử dụng lại thông tin class khác => tính kế thừa
{
    private:
        string chuyenNganh;

    public:
        void setchuyennganh(string newCN)
        {
            /*
            SinhVien sv;
            sv.setName("Hieu");
            sv.name = "Hieu";  // protected chỉ được truy xuất từ bên trong không truy xuất 
                                 từ bên ngoài
            */
            chuyenNganh = newCN;
        }

        string getCN()
        {
            return chuyenNganh;
        }

        void display(int x = 2)  // overload: những hàm trung tên với nhau - Dùng mở rộng tính năng của hàm không nhất thiết phải giống hàm cũ hoàn toàn
        {
            DoiTuong::display();
            cout << "Chuyen nganh: " << getCN() << endl ;
             cout << "x= " << x << endl << endl;  // Hàm khi định nghĩa lại có thể thêm tham số
        }
  
};

class HocSinh : protected DoiTuong  // khi 1 class đu
{
    private:
        string lophoc;

    public:
        void setlophoc(string newLN)
        {
            /*
            SinhVien sv;
            sv.setName("Hieu");
            sv.name = "Hieu";  // protected chỉ được truy xuất từ bên trong không truy xuất 
                                 từ bên ngoài
            */
            lophoc = newLN;
        }

        string getLH()
        {
            return lophoc;
        }

        void display()  // overload: những hàm trung tên với nhau - Dùng mở rộng tính năng của hàm
        {
            DoiTuong::display();
            cout << "Lop hoc: " << getLH() << endl << endl;
        }
  
};

// Đa kế thừa là 1 class kế thừa từ nhiều class khác
class test : public HocSinh, protected SinhVien, private DoiTuong
{};

int main()
{
    DoiTuong dt;

    SinhVien sv1, sv2;

    sv1.setName("Hieu");
    sv1.setAge(25);
    sv1.setchuyennganh("DTD59DH");
    sv1.display();

    HocSinh hs1;
    /* Do kế thừa kiểu protected lên không thể truy xuất từ bên ngoài như dưới
    hs1.setName("Trung");
    hs1.setAge(25);
    */
    hs1.setlophoc("12A9");
    hs1.display();          // Hàm này truy xuất được vì nó được định nghĩa lại

    return 0;
}
```
 </details>

--------------------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 3: Virtual Function</b></summary>

## 1. Tính đa hình - Polymorphism
- **Tính đa hình (Polymorphism)** có nghĩa là "nhiều dạng" và nó xảy ra khi chúng ta có nhiều class có liên quan với nhau thông qua tính kế thừa.
- Tính đa hình có thể được chia thành hai loại chính:<br>
&nbsp;+ Đa hình tại thời điểm biên dịch (Compile-time Polymorphism).<br>
&nbsp;+ Đa hình tại thời điểm chạy (Run-time Polymorphism).<br>
## 2. Hàm ảo
- **Hàm ảo** là một hàm thành viên được khai báo trong **class cha** với từ khóa **virtual**.
- Khi một hàm là virtual, nó có thể được ghi đè (override) trong class con để cung cấp cách triển khai riêng.
- Khi gọi một hàm ảo thông qua một con trỏ hoặc tham chiếu đến lớp con, hàm sẽ được quyết định dựa trên đối tượng thực tế mà con trỏ hoặc tham chiếu đang trỏ tới chứ không dựa vào kiểu của con trỏ.
- Ví dụ:
```cpp
#include <iostream>
#include <string>
using namespace std;

class DoiTuong
{
    protected:
        string name;
        int age;
        int id;

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
        DoiTuong()          // Constructor
        {
            static int ID = 1;
            DoiTuong::id = ID;
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
        virtual void display()  // Hàm ảo - hàm virtual giúp gọi hàm dựa vào đối tượng thực tế
        {
            cout << "Ten: " << getName() << endl;
            cout << "Tuoi: " << getAge() << endl;
            cout << "MSV: " << getID() << endl;
        }
    
};

class SinhVien : public DoiTuong  // 1 class sử dụng lại thông tin class khác => tính kế thừa
{
    private:
        string chuyenNganh;

    public:
        void setchuyennganh(string newCN)
        {
            chuyenNganh = newCN;
        }

        string getCN()
        {
            return chuyenNganh;
        }

        void display() const ;// những class con định nghĩa lại hàm gọi là override
  
};

int main()
{

    SinhVien sv1, sv2;

    sv1.setName("Hieu");
    sv1.setAge(25);
    sv1.setchuyennganh("DTD59DH");
    sv1.display();
    
    // Tính đa hình sử dụng down - casting -> dễ gây lỗi bộ nhớ
    DoiTuong *dt;

    dt = &sv1;   
    dt->display();
    
    //Muốn in đầy đủ thông tin ta ép ngược lại về kiểu Sinh Vien
    ((SinhVien*)dt)->display(); // down - casting: là ép kiểu từ class cha xuống class con

     // Tính đa hình sử dụng up - casting -> đây là dạng ép kiểu an toàn
    SinhVien *sv = &sv1;   
    sv->display();
    
    ((DoiTuong*)sv)->display(); // up - casting: là ép kiểu từ class con lên class cha

    return 0;
}
```
## 3. Override và Tính đa hình run-time
- **Override** là việc ghi đè hàm ảo ở class con bằng cách định nghĩa lại nó. 
- Khi một hàm ảo được ghi đè, hành vi của nó sẽ **phụ thuộc vào kiểu của đối tượng thực tế**, chứ không phải kiểu của con trỏ hay tham chiếu.
- **Tính đa hình run-time** xảy ra khi quyết định gọi hàm nào (phiên bản của class cha hay class con) được đưa ra tại **thời điểm chạy**, không phải lúc biên dịch, giúp mở rộng chức năng. Điều này giúp chương trình linh hoạt hơn, giúp xác định đối tượng để gọi hàm cho hợp lý.

|Overload|Override|
|:------------------------|:------------------------|
|- Định nghĩa lại hàm  có thể mở rộng thêm tham số của hàm không có virtual|- Định nghĩa lại hàm phải giống hoàn toàn về đặc điểm|
|- Liên quan đến kế thừa|- Liên quan đến đa hình|
|- Không có virtual|- Có virtual|
## 4. Pure Virtual Function - Hàm thuần ảo
- **Hàm thuần ảo** là một **hàm ảo** không có phần định nghĩa trong class cha, được khai báo với **cú pháp = 0** và khiến class cha trở thành **class trừu tượng (abstract class)**, nghĩa là không thể tạo đối tượng từ class này.
- Abstract Class: Có ít nhất một hàm thuần ảo và các hàm khác không phải thuần ảo.
- Interface: Là class mà hàm bên trong là hàm thuần ảo.
- Ví dụ:
```cpp
#include <iostream>
using namespace std;

class cha    // class trừu tượng
{
    public:
        virtual void display() = 0; // Hàm ảo thuần túy
};

class con : public cha{
    public:
        void display() override{   // Ghi đè hàm ảo thuần túy
            cout << "display from class con" << endl;
        }
};

int main(){
    // cha ptr; // wrong - không thể tạo đối tượng vì là class trìu tượng
    cha *ptr;
    con obj;

    ptr = &obj;
    ptr->display();

    return 0;
}
```
 </details>
 
--------------------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>📖BÀI 4: Compile-time Polymorphism</b></summary>
  
## 1. Khái niệm
- **Compile-time Polymorphism**: là đa hình xảy ra ở quá trình biên dịch
- Chia làm 2 dạng:<br>
&nbsp;+ Nạp chồng hàm (Function Overloading).<br>
&nbsp;+ Nạp chồng toán tử (Operator Overloading).
## 2. Function Overloading
- **Nạp chồng hàm (Function Overloading)** là việc định nghĩa **nhiều hàm cùng tên** nhưng **khác tham số** trong cùng một phạm vi. 
- Trình biên dịch sẽ chọn hàm phù hợp dựa trên **kiểu** và **số lượng đối số** khi gọi hàm.
- ví dụ:
```cpp
#include <iostream>
#include <string>
using namespace std;

// 1 method có thể có nhiều input parameter, return type khác nhau
class TinhToan{
    private:
        int a;
        int b;
    public:
        // Method cùng tên không bắt buộc khác kiểu trả về nhưng bắt buộc khác tham số
        int tong(int a, int b)
        {
            return a+b;
        }

        // Method lỗi vì chỉ khác kiểu trả về
        //double tong(int a, int b)  
        //{                      
         //    return a+b;
        //}

        // Method cùng tên nhưng khác số lượng tham số
        double tong(int a, int b, int c, double d){
            return (double)a+b+c+d;
        }

        // Method cùng tên nhưng khác tham số
        double tong(int a, double b){
            return (double)a+b;
        }
};

int main(int argc, char const *argv[])
{
    TinhToan th, th1, th2;
    cout << th.tong(2, 5) << endl;
    cout << th1.tong(2, 5, 7, 6.7) << endl;
    cout << th2.tong(2, 3.5) << endl;
    return 0;
}
```
## 3. Operator Overloading
- Tất cả toán tử đều không mặc định dùng cho kiểu dữ liệu tự định nghĩa ra, nó chỉ mặc định cho toán tử nguyên thủy
=> Muốn sử dụng kiểu dữ liệu tự định nghĩa phải định nghĩa lại toán tử cho class tham số
- **Nạp chồng toán tử (Operator Overloading)** là việc định nghĩa lại cách hoạt động của các toán tử (+, -, , =, ==, <<, >>,...) cho các kiểu dữ liệu do người dùng định nghĩa (class/struct).
- Cú pháp:
```cpp
<return_type> operator symbol (parameter)
{
    // logic của toán tử
}
```
- Các toán tử có thể dịnh nghĩa:
```cpp
+	–	*	/	%	^	&	|	~	!	=	<	>	+=	-=	*=
/=	%=	^=	&=	|=	<<	>>	>>=		<<=	==	!=	<=	>=	&&	||	++
—	->*	,	->	[]	()	new	delete	new[]	delete[]
```
- Các toán tử không thể định nghĩa lại:<br>
&nbsp;+ Toán tử . (chấm) <br>
&nbsp;+ Toán tử phạm vi :: <br> 
&nbsp;+ Toán tử điều kiện ?: <br>
&nbsp;+ Toán tử sizeof <br>

</details>

--------------------------------------------------------------------------------------------------------------------------------------------------------

