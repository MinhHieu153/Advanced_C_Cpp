**Đề bài: Viết thuật toán tìm kiếm theo tên hoặc SĐT**<br>
**Bài làm**

```c
#include <stdio.h> 
#include <stdlib.h>
#include <string.h>

#define DATABASE_PATH "D:\\C_C++\\baitap\\Advanced_C\\Algorithm\\DataBase.csv"

/**
 * @struct  User
 * @prief   Cấu trúc lưu thông tin người dùng
 * @details Cấu trúc chứa tên, tuổi và số điện thoại của người dùng
 */
typedef struct
{
    char *name;
    int age;
    char *addr;
    char *phone;
} User;

/**
 * @brief   Giải phóng bộ nhớ cho các trường của cấu trúc User
 * @details Hàm này giải phóng phóng bộ nhớ cáp phát động cho thành viên của cấu trúc User
 * @param   user: Con trỏ đến cấu trúc 'User' cần giải phóng
 */
void free_user(User *user)
{
    free(user->name);
    free(user->addr);
    free(user->phone);
}

/**
 * @brief   Đọc dữ liệu từ file CSV và in thông tin người dùng.
 * @details Hàm này mở file CSV theo đường dẫn cung cấp, đọc từng dòng dữ liệu,
 *          phân tích và lưu vào cấu trúc `User`, sau đó in ra thông tin. Bộ nhớ
 *          cấp phát động sẽ được giải phóng ngay sau khi sử dụng.
 * @param   filename: Đường dẫn đến file CSV cần đọc.
 */
void readCSV(const char *filename)
{
    FILE *file = fopen(filename, "r");

    /* Kiểm tra xem file có mở thành công không */
    if (file == NULL)
    {
        printf("Cannot open file!\n");
        return;
    }

    char line[100]; /**< Bộ đệm để lưu từng dòng từ file CSV */

    /* Bỏ qua dòng tiêu đề */
    fgets(line, sizeof(line), file);

    /* In tiêu đề bảng dữ liệu */
    printf("%-20s %-5s\t  %-20s\t  %-15s\n", "Name", "Age", "Address", "Phone Number");

    /* Đọc từng dòng dữ liệu từ file CSV */
    while (fgets(line, sizeof(line), file))
    {
        User user;  /**< Biến lưu thông tin người dùng tạm thời */
        
        /* Tách tên từ dòng hiện tại */
        char *token = strtok(line, ",");
        user.name = (char*)malloc(strlen(token)+1);     /**< Cấp phát bộ nhớ cho tên */
        strcpy(user.name, token);

        /* Tách tuổi */
        token = strtok(NULL, ",");
        user.age = atoi(token);

        /* Tách địa chỉ */
        token = strtok(NULL, ",");
        user.addr = (char*)malloc(strlen(token)+1);     /**< Cấp phát bộ nhớ cho địa chỉ */
        strcpy(user.addr, token);

        /* Tách số điện thoại */
        token = strtok(NULL, ",");
        user.phone = (char*)malloc(strlen(token)-1);    /**< Cấp phát bộ nhớ cho số điện thoại */
        strcpy(user.phone, token);

        /* In thông tin người dùng */
        printf("%-20s %-5d\t %-20s\t %-4s", user.name, user.age, user.addr, user.phone);
        /* Giải phóng bộ nhớ của các trường trong cấu trúc User */
        free_user(&user); 
    }

    fclose(file);   /**< Đóng file sau khi đọc xong */
}


/**
 * @brief   Ghi dữ liệu vào file CSV 
 * @details Hàm này mở file CSV theo đường dẫn cung cấp, người dùng sẽ nhập dữ liệu vào,
 *          dữ liệu này sẽ được lưu vào cấu trúc `User`, sau đó ghi vào file CSV. Bộ nhớ
 *          cấp phát động sẽ được giải phóng ngay sau khi sử dụng.
 * @param   filename: Đường dẫn đến file CSV cần đọc.
 */
void WriteFile(const char *filename)
{   
    int quantity = 0;

    // Mở file để ghi, nếu chưa có sẽ tạo file mới
    FILE *file = fopen(filename, "w+");

    // Kiểm tra nếu file không mở được
    if (file == NULL) {
        printf("Cannot create file.\n");
        return;
    }

    char line[100]; /**< Bộ đệm để lưu từng dòng từ file CSV */

    /* Bỏ qua dòng tiêu đề */
    fgets(line, sizeof(line), file);

    // Kiểm tra số lượng user
    while (fgets(line, sizeof(line), file))
    {
        quantity++;
    }

    if (quantity == 0)
    {
        // Ghi dòng tiêu đề vào file CSV
        fprintf(file, "Name, Age, Address, Phone Number\n");
    } 

    printf("Nhap so luong user them: ");
    scanf("%d", &quantity);
    getchar();
    User *user = (User*)calloc(quantity, sizeof(User));
    for (int i=0;i<quantity;i++)
    {   
        (user+i)->name= malloc(50*sizeof(char));
        (user+i)->addr= malloc(50*sizeof(char));
        (user+i)->phone= malloc(50*sizeof(char));

        printf("*** Infor User %d ***\n",i+1);
        printf("Name: ");
        gets((user + i)->name);
        printf("Age: ");
        scanf("%d",&(user+i)->age);
        printf("Address: ");
        getchar();
        gets((user + i)->addr);
        printf("Phone Number: ");
        gets((user+i)->phone);
    }

    // Ghi các dòng thông tin vào file CSV
    for (int i=0;i<quantity;i++)
    {
        fprintf(file,"%s, %d, %s, %s\n",(user+i)->name,(user+i)->age,(user+i)->addr,(user+i)->phone);
    }


    for (int i=0;i<quantity;i++)
    { 
        free_user(user+i);
    }
    free(user);

    // Đóng file sau khi ghi
    fclose(file);

    printf("File created successfully\n");
    
}

/**
 * @brief  Tìm thông tin file CSV 
 * @details Hàm này mở file CSV theo đường dẫn cung cấp, người dùng sẽ nhập dữ liệu cần tìm là số điện thoại hoặc tên ,
 *          dữ liệu này sẽ được lưu vào cấu trúc `User`, Hàm này sẽ tách ra thông tin và số điện thoại trong file CSV
 *          sau đó so sánh với thông tin được nhập vào và in ra thông tin đó. Bộ nhớ cấp phát động sẽ được giải phóng ngay sau khi sử dụng.
 * @param   filename: Đường dẫn đến file CSV cần đọc.
 */
void FindUser(const char *filename)
{
    int found = 0;
    FILE *file = fopen(filename, "r");

    /* Kiểm tra xem file có mở thành công không */
    if (file == NULL)
    {
        printf("Cannot open file!\n");
        return;
    }

    char line[100]; //Bộ đệm để lưu từng dòng từ file CSV 

    //Bỏ qua dòng tiêu đề 
    fgets(line, sizeof(line), file);
    
    User user;
    user.name = malloc(10*sizeof(char));
    user.phone = malloc(10*sizeof(char));
    printf("Nhap ten: ");
    gets(user.name);
    printf("Phone Number: ");
    gets(user.phone);

    //Đọc từng dòng dữ liệu từ file CSV 
    while (fgets(line, sizeof(line), file))
    {
        User user1;  // Biến lưu thông tin người dùng tạm thời 

        //Tách tên từ dòng hiện tại 
        char *token = strtok(line, ", ");
        user1.name = (char*)malloc(strlen(token)+1);     //< Cấp phát bộ nhớ cho tên 
        strcpy(user1.name, token);

        // Tách tuổi 
        token = strtok(NULL, ", ");
        user1.age = atoi(token);
        
        // Tách địa chỉ 
        token = strtok(NULL, ", ");
        user1.addr = (char*)malloc(strlen(token)+1);     //< Cấp phát bộ nhớ cho địa chỉ 
        strcpy(user1.addr, token);
        
        // Tách số điện thoại 
        token = strtok(NULL, ", ");
        token = strtok(token, "\n");
        user1.phone = (char*)malloc(strlen(token)-3);    // Cấp phát bộ nhớ cho số điện thoại 
        strcpy(user1.phone, token);

        if ((strcmp(user.name, user1.name) == 0) || (strcmp(user.phone, user1.phone) == 0))
        {   
            if (found == 0)
            {
                // In tiêu đề bảng dữ liệu 
                printf("%-20s %-5s\t %-20s\t  %-15s\n", "Name", "Age", "Address", "Phone Number");
            }
            // In thông tin người dùng 
            printf("%-20s %-5d\t %-20s\t  %-5s\n", user1.name, user1.age, user1.addr, user1.phone);

            found = 1;
        }
        // Giải phóng bộ nhớ của các trường trong cấu trúc User 
        free_user(&user1);

    }

    if (found == 0)
    {
        printf("No user information found!\n");
    }

    free(user.name);
    free(user.phone);

    fclose(file);   /**< Đóng file sau khi đọc xong */
}

int main()
{   
    int n;

    do
    {
        printf("******** Menu ********\n");
        printf("1. Ghi thong tin User\n");
        printf("2. Doc thong tin User\n");
        printf("3. Tim thong tin User\n");
        printf("4. Nhap 0 de thoat\n");
        printf("Chon thong tin can sua: ");
        scanf("%d",&n);
        getchar();

        switch (n)
        {
        case 1: // Ghi thong tin User
                WriteFile(DATABASE_PATH);
                break;

        case 2: // Doc thong tin User
                readCSV(DATABASE_PATH);
                break;

        case 3: // Tim thong tin User  
               FindUser(DATABASE_PATH);
               break;
        default:
            break;
        }
    }
    while(n!=0);

    return 0;
}           
```
