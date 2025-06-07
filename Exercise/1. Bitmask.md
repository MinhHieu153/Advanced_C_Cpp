```c
#include <stdio.h>
#include <stdint.h>

// Định nghĩa màu sắc xe
#define COLOR_RED 0	
#define COLOR_BLUE 1
#define COLOR_BLACK 2
#define COLOR_WHITE 3

// Định nghĩa công suất động cơ xe
#define POWER_100HP 0
#define POWER_150HP 1
#define POWER_200HP 2

// Định nghĩa dung tích động cơ xe
#define ENGINE_1_5L 0
#define ENGINE_2_0L 1

// Đặt tên mới cho uint8_t
typedef uint8_t CarColor;       // CarColor là tên của kiểu uint8_t
typedef uint8_t CarPower;       // CarPower là tên của kiểu uint8_t
typedef uint8_t CarEngine;      // CarEngine là tên của kiểu uint8_t     

// Định nghĩa thêm các tính năng khác
#define SUNROOF_MASK 1 << 0     // 0001
#define PREMIUM_AUDIO_MASK 1 << 1 // 0010
#define SPORTS_PACKAGE_MASK 1 << 2 // 0100
// Thêm các bit masks khác tùy thuộc vào tùy chọn


// Khai báo cấu trúc struct CarOptions với các thành phần: color, power, engine, và additionalOptions 
typedef struct { 
    uint8_t additionalOptions : 3; // 3 bits cho các tùy chọn bổ sung
    CarColor color : 2;            // 2 bits chọn màu xe (4 màu: 0 - 3)
    CarPower power : 2;            // 2 bits chọn công suất (4 loại: 0 - 3)
    CarEngine engine : 1;          // 2 bits chọn động cơ (4 loại: 0 - 3)  
} CarOptions;

/**
 * @brief   Cấu hình cho xe .
 * @details Hàm này sẽ truy cập từng thành phần trong cấu trúc của car
 *          và gắn các giá trị muốn thay
 * @param   car: con trỏ có kiểu dữ liệu CarOptions trỏ đến các tùy chọn của mỗi xe 
 *          color: Màu xe
 *          power: công xuất xe
 *          engine: màu xe
 *          options: Tùy chọn khác
 */
void configureCar(CarOptions *car, CarColor color, CarPower power, CarEngine engine, uint8_t options) 
{
    car->color = color;               // Gắn màu muốn cấu hình  
    car->power = power;               // Gắn công suất muốn cấu hình
    car->engine = engine;             // Gắn động cơ muốn cấu hình
    car->additionalOptions = options; // Gắn các tùy chọn muốn thêm
}

/**
 * @brief   Cài thêm các tùy chọn cho xe .
 * @details Hàm sẽ dùng toán tử OR (bitwise OR) để bật các bit tương ứng với mỗi tính năng trong additionalOptions.
 * @param   car: con trỏ có kiểu dữ liệu CarOptions trỏ đến các tùy chọn của mỗi xe 
 *          optionMask: option mình muốn thêm
 */
void setOption(CarOptions *car, uint8_t optionMask) {
    car->additionalOptions |= optionMask;
}

/**
 * @brief   Bỏ cài thêm các tùy chọn cho xe .
 * @details Hàm sẽ dùng toán tử AND (bitwise AND) để tắt các bit tương ứng với mỗi tính năng trong additionalOptions.
 * @param   car: con trỏ có kiểu dữ liệu CarOptions trỏ đến các tùy chọn của mỗi xe 
 *          optionMask: option mình muốn bỏ
 */
void unsetOption(CarOptions *car, uint8_t optionMask) {
    car->additionalOptions &= ~optionMask;
}

/**
 * @brief   Hiển thị các tùy chọn của xe.
 * @details Hàm sẽ in ra các tùy chọn và tính năng được cấu hình trên xe.
 * @param   car: biến hằng có kiểu dữ liệu CarOptions  
 */
void displayCarOptions(const CarOptions car) {
    const char *colors[] = {"Red", "Blue", "Black", "White"};
    const char *powers[] = {"100HP", "150HP", "200HP"};
    const char *engines[] = {"1.5L", "2.0L"};

    printf("Car Configuration: \n");
    printf("Color: %s\n", colors[car.color]);
    printf("Power: %s\n", powers[car.power]);
    printf("Engine: %s\n", engines[car.engine]);
    printf("Sunroof: %s\n", (car.additionalOptions & SUNROOF_MASK) ? "Yes" : "No");
    printf("Premium Audio: %s\n", (car.additionalOptions & PREMIUM_AUDIO_MASK) ? "Yes" : "No");
    printf("Sports Package: %s\n", (car.additionalOptions & SPORTS_PACKAGE_MASK) ? "Yes" : "No");
}

int main() {
    CarOptions myCar;                                                   // Khai báo biến myCar kiểu dữ liệu CarOptions.
    configureCar(&myCar, COLOR_BLACK, POWER_150HP, ENGINE_2_0L, 0);     // Cấu hình cho myCar
	

    setOption(&myCar, SUNROOF_MASK);                                    // Thêm tính năng SUNROOF_MASK
    setOption(&myCar, PREMIUM_AUDIO_MASK);                              // Thêm tính năng PREMIUM_AUDIO_MASK                
    
    displayCarOptions(myCar);                                           // Hiển thị các tùy chọn và tính năng của xe

    unsetOption(&myCar, PREMIUM_AUDIO_MASK);                            // Bỏ tính năng PREMIUM_AUDIO_MASK
    displayCarOptions(myCar);                                           // Hiển thị các tùy chọn và tính năng của xe

    printf("size of my car: %d\n", sizeof(CarOptions));                 

    return 0;
}
```
