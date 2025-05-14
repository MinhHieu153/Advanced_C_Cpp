Bài 1:
- **Mục tiêu:**
Sửa đổi hàm THROW để nó chấp nhận một thông điệp lỗi dưới dạng chuỗi ký tự, bên cạnh mã lỗi.
Thông điệp lỗi nên được lưu trữ ở một nơi mà có thể truy cập được sau khi longjmp được gọi.
- **Bài làm:**
```c
#include <stdio.h>
#include <setjmp.h>
#include <string.h>
#include <math.h>

jmp_buf buf;

int exception_code;
char error_code[200];

typedef enum
{
    NO_ERROR,
    REMAINDER_DIVISION,
    NO_EXIT,
    DIVIDE_BY_0,
    NEGATIVE_SQRT
} ErrorCodes;  

#define TRY if ((exception_code = setjmp(buf)) == 0)
#define CATCH(x) else if (exception_code == x)
#define THROW(x, msg)                    \
        strcpy(error_code, msg);         \
        longjmp(buf, x);                 \
                

double divide(int a, int b)
{
    if (a == 0 && b == 0)
    {
        THROW(NO_EXIT,"Không ton tai");
    }
    else if (b == 0)
    {
        THROW(DIVIDE_BY_0, "Khong chia cho 0");
    }
    else if (a % b != 0)
    {

        THROW(REMAINDER_DIVISION, "Phep chi du");
    }

    return (double)a/b;
}

double square(int a)
{
    if (a < 0)
    {
        THROW(NEGATIVE_SQRT, "Can bac hai cua so am");
    }

    return sqrt(a);
}

int main(int argc, char const *argv[])
{   
    int a, b;
    while(1)
    {   
        printf("Nhap a:");
        scanf("%d", &a);
        printf("Nhap b:");
        scanf("%d", &b);

        exception_code = NO_ERROR;
        
        printf("Kiem tra phep chia\n");

        TRY
        {
            printf("Ket qua: %0.3f\n", divide(a,b));
        }
        CATCH(REMAINDER_DIVISION)
        {   
            printf("Phep chia du\n", error_code);
            printf("Ket qua: %0.3f\n", (double)a/b);
        }
        CATCH(NO_EXIT)
        {
            printf("ERROR: %s\n", error_code);
        }
        CATCH(DIVIDE_BY_0)
        {
            printf("ERROR! %s\n", error_code);
        }

        printf("Kiem tra phep nhan\n");

        TRY
        {
            printf("Ket qua: %0.3f\n", square(a));
        }
        CATCH(NEGATIVE_SQRT)
        {   
        printf("ERROR! %s\n", error_code);
        }

    }
    
    return 0;
}
```
Bài 2:
```c
#include <stdio.h>
#include <setjmp.h>
#include <string.h>

jmp_buf buf;
int exception_code;
char error_code[200];

#define TRY if ((exception_code = setjmp(buf)) == 0)
#define CATCH(x) else if (exception_code == x)
#define THROW(x, msg)                    \
        strcpy(error_code, msg);         \
        longjmp(buf, x);                 \

typedef enum
{
    NO_ERROR, 
    FILE_ERROR, 
    NETWORK_ERROR, 
    CALCULATION_ERROR 
} ErrorCodes;  

void readFile() 
{    
    printf("Doc file...\n");   
    FILE *file = fopen("D:\\setjmp.txt", "r");
    if (file == NULL) 
    {   
        fclose(file) ; 
        THROW(FILE_ERROR, "Loi doc file: File khong ton tai.");
    } 
    fclose(file) ;   
}

void networkOperation() 
{
    int isConnected = 0;  // Giả định không có kết nối
    printf("Doc ket noi...\n");  
    if (!isConnected) 
    {
        THROW(NETWORK_ERROR, "Loi ket noi mang: Khong co ket noi.");
    }
}

void calculateData() 
{
    printf("Tinh data...\n");
    int a = 10;
    int b = 0;  

    // Kiểm tra lỗi chia cho 0
    if (b == 0) {
        THROW(CALCULATION_ERROR, "Loi tinh toan: Chia cho 0.");
    }

    // Thực hiện phép tính
    int result = a / b;
    printf("Ket qua: %d\n", result);
}

int main()
{   
    exception_code = NO_ERROR;
    TRY 
    {
        readFile();
        networkOperation();
        calculateData();

        printf("Chuong trinh ket thuc an toan.\n");
    } 
    CATCH(FILE_ERROR) 
    {
        printf("%s\n", error_code);
    }
    CATCH(NETWORK_ERROR) 
    {
        printf("%s\n", error_code);
    }
    CATCH(CALCULATION_ERROR) 
    {
        printf("%s\n", error_code);
    }
    
    printf("Ket thuc chuong trinh.\n");
    return 0;
}
```
