<details>
<summary><b>JSONParser.h</b></summary>
  
  ```c
  #ifndef JSONPARSER_H
  #define JSONPARSER_H
  
  #include <stdio.h>
  #include <string.h>
  #include <stdlib.h>
  #include <stddef.h>
  #include <ctype.h>
  #include <stdbool.h>
  
  
  typedef enum {
      JSON_NULL,
      JSON_BOOLEAN,
      JSON_NUMBER,
      JSON_STRING,
      JSON_ARRAY,
      JSON_OBJECT
  } JsonType;
   
  /* 
      nếu JSON là type = JSON_NUMBER -> value => double number;
      nếu JSON là type = JSON_BOOLEAN -> value => int boolean;
  
      nếu JSON là type = JSON_ARRAY 
      - value có là:
          + Số lượng phần tử mảng: size_t count (size_t tương ứng kiểu unsigned int)
          + Từng phần tử (các phần tử không nhất thiết cùng kiểu dữ liệu): xác định kiểu + giá trị
  
      nếu JSON là type = JSON_OBJECT
      - value có là:
          + Số lượng căp 'key - value': size_t count
          + Kiểu dữ liệu của value: struct JsonValue *values;
          + key là chuỗi: char **keys (là con trỏ bậc 2 để có thể trỏ đến các key khác)
  */
  
  typedef struct JsonValue {
      JsonType type;
      union {
          int boolean;
          double number;
          char *string;
  
          struct {
              struct JsonValue *values;
              size_t count;
          } array;
  
          struct {
              char **keys;
              struct JsonValue *values;
              size_t count;
          } object;
      } value;
  } JsonValue;
  
  void free_json_value(JsonValue *json_value);
  void display(JsonValue* json_value);
  JsonValue *parse_json(const char **json);

  #endif  // JSONPARSE_H
  ```
</details>

--------------------------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>JSONParser.c</b></summary>
  
```c
#include "JSONParser.h"

/** 
*  @brief   Bỏ qua các ký tự khoảng trắng trong JSON
*  @param   json [in, out] Con trỏ đến chuỗi JSON
*  @return  void
*/
static void skip_whitespace(const char **json)
{   
    /*
        isspace: hàm kiểm tra xem ký tự có phải khoảng trắng không
            + Trả về 0: Không phải khoảng trắng
            + Trả về 1: Ký tự là khoảng trắng
    */
    while (isspace(**json))                             
    {
        (*json)++;         // Nếu là khoảng trắng con trỏ sẽ nhảy đến ký tự tiếp theo
    }
}

/** 
*  @brief   Phân tích giá trị NULL từ chuỗi JSON
*  @param   json [in, out] Con trỏ trỏ đến chuỗi JSON
*  @return  JsonValue* Con trỏ trỏ đến giá trị "JSON_NULL" hoặc NULL nếu lỗi
*/
JsonValue *parse_null(const char **json) 
{
    skip_whitespace(json);                                          // Bỏ qua khoảng trắng
    JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));     // Cấp phát động vùng nhớ cho con trỏ value có kiểu dữ liệu JsonValue
    
    // strncmp so sánh 2 chuỗi giống nhau về độ dài và độ dài mình quy định
    if (strncmp(*json, "null", 4) == 0)                             // So sánh 4 kí tự đầu trong chuôi JSON có bằng NULL không --> nếu bằng trả về 0
    {
        value->type = JSON_NULL;                                    // Gắn kiểu là "JSON_NULL"
        *json += 4;                                                 // dịch chuyển json qua 4 ký tự và bắt đầu vị trí mới
        return value;                                               // Trả về value có kiểu "JSON_NULL"
    }

    free(value);                                                   // Nếu không phải NULL thu hồi lại vùng nhớ đã cấp phát    
    return NULL;                                                   // Trả về giá trị NULL
}

/** 
*  @brief   Phân tích giá trị boolean(true/false) từ chuỗi JSON
*  @param   json [in, out] Con trỏ trỏ đến chuỗi JSON
*  @return  JsonValue* Con trỏ trỏ đến giá trị "JSON_BOOLEAN" hoặc NULL nếu lỗi
*/
JsonValue *parse_boolean(const char **json) 
{
    skip_whitespace(json);                                         // Bỏ qua khoảng trắng 
    JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));    // Cấp phát động vùng nhớ cho con trỏ value có kiểu dữ liệu JsonValue 

    if (strncmp(*json, "true", 4) == 0) {                          // So sánh 4 kí tự đầu trong chuôi JSON có bằng "true" không --> nếu bằng trả về 0 
        value->type = JSON_BOOLEAN;                                // Gắn kiểu là "JSON_BOOLEAN" 
        value->value.boolean = true;                               // Gắn giá trị là "true"
        *json += 4;                                                // dịch chuyển json qua 4 ký tự và bắt đầu vị trí mới 
    } 
    else if (strncmp(*json, "false", 5) == 0)                      // So sánh 5 kí tự đầu trong chuôi JSON có bằng "false" không --> nếu bằng trả về 0 
    {                      
        value->type = JSON_BOOLEAN;                                // Gắn kiểu là "JSON_BOOLEAN"  
        value->value.boolean = false;                              // Gắn giá trị là "false"
        *json += 5;                                                // dịch chuyển json qua 5 ký tự và bắt đầu vị trí mới  
    } 
    else 
    {
        free(value);                                               // Nếu không phải kiểu boolean thì thu hồi lại vùng nhớ đã cấp phát  
        return NULL;                                               // Trả về giá trị NULL   
    }
    return value;                                                  // Trả về value có kiểu "JSON_BOOLEAN"          
}

/** 
*  @brief   Phân tích giá trị số từ chuỗi JSON
*  @param   json [in, out] Con trỏ trỏ đến chuỗi JSON
*  @return  JsonValue* Con trỏ trỏ đến giá trị "JSON_NUMBER" hoặc NULL nếu lỗi
*/
JsonValue *parse_number(const char **json) 
{
    skip_whitespace(json);                                         // Bỏ qua khoảng trắng  
    char *end;                                                     // Tạo con trỏ lưu vị trí kết thúc của chuỗi 
    /*
        Hàm strtod: Chuyển 1 chuỗi sang số thực
        - Cú pháp: double  strtod(const char *str, char **endptr)
          + const char *str: tham số mình cần chuyển đổi
          + char **endptr: con trỏ cấp 2 lưu trữ vị trí kết thúc
        - Nếu chuỗi mình cần chuyển đổi hợp lệ (đều là số) thì con trỏ endptr trỏ tới ký tự ngay sau
        - Nếu không hợp lệ trỏ đến ký tự đầu tiên
        - vd: + nếu chuỗi cần chuyền "1234" -> Hợp lệ thì con trỏ endptr sẽ trỏ tới sau số "4" 
              + nếu chuỗi cần chuyền "1234a" -> Không hợp lệ thì con trỏ endptr sẽ trỏ tới số "1" 
    */
    double num = strtod(*json, &end);                              // Chuyển chuỗi json sang số thực     

    if (end != *json)                                              // Kiểm tra địa chỉ con trỏ end trỏ đến có giống json không --> Nếu giống chuỗi không hợp lệ  
    {
        JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));// Cấp phát động vùng nhớ cho con trỏ value có kiểu dữ liệu JsonValue 
        value->type = JSON_NUMBER;                                 // Nếu chuỗi hợp lệ --> Gắn kiểu là "JSON_NUMBER" 
        value->value.number = num;                                 // Gắn giá trị là num vào phần tử number
        *json = end;                                               // Cho con trỏ json trỏ đên vị trí mới
        return value;                                              // Trả về value có kiểu "JSON_NUMBER" 
    } 

    return NULL;                                                   // Trả về giá trị NULL nếu không hợp lệ  
}

/** 
*  @brief   Phân tích giá trị chuỗi từ chuỗi JSON
*  @param   json [in, out] Con trỏ trỏ đến chuỗi JSON
*  @return  JsonValue* Con trỏ trỏ đến giá trị "JSON_STRING" hoặc NULL nếu lỗi
*/
JsonValue *parse_string(const char **json)  
{
    skip_whitespace(json);                                               // Bỏ qua khoảng trắng

    /*
        Dấu \" dùng cho bắt đầu và kết thúc để nhận biết chuỗi con trong chuỗi lớn.
        VD: const char *str = "\"key1\": \"value\"";  //key1 và value1 là chuỗi con trong chuỗi lớn
    */
    if (**json == '\"')                                                  // Kiểm tra ký tự json có phải là dấu \" 
    {                               
        (*json)++;                                                       // Nếu đúng bỏ qua dấu đó  
        const char *start = *json;                                       // Lưu vị trí hiện tại vào con trỏ start  

        while (**json != '\"' && **json != '\0')                    
        {
            (*json)++;                                                   // Duyệt qua từng ký tự cho đén khi gặp dấu kết thúc chuỗi con hoặc NULL   
        }

        if (**json == '\"')                                              // Nếu lầ dấu kết thúc chuỗi con     
        {
            size_t length = *json - start;                               // Tính độ dài chuỗi con đó
            char *str = (char *) malloc((length + 1) * sizeof(char));    // Cấp phát động (length + 1 )byte cho con trỏ str có kiểu dữ liệu char (vì thêm phần tử "\0" để kết thúc chuỗi)
            strncpy(str, start, length);                                 // Sao chép chuỗi có độ dài length từ vị trí start vào chuỗi str   
            str[length] = '\0';                                          // Gắn ký tự kết thúc là NULL            
            JsonValue *value = (JsonValue *) malloc(sizeof(JsonValue));  // Cấp phát động vùng nhớ cho con trỏ value có kiểu dữ liệu JsonValue 
            value->type = JSON_STRING;                                   // Gắn kiểu là "JSON_STRING"   
            value->value.string = str;                                   // Gắn giá trị str vào phần tử string   
            (*json)++;                                                   // Trỏ đến vị trí mới   
            return value;                                                // Trả về value có kiểu "JSON_STRING"   
        }
    }

    return NULL;                                                        // Trả về giá trị NULL nếu chuỗi không hợp lệ     
}

/** 
*  @brief   Phân tích giá trị mảng JSON
*  @details Hàm này phân tích cú pháp của 1 mảng JSON bằng cách duyệt 
            qua các phần tử và gọi 'parse_json()' để xử lý từng phần tử
*  @param   json [in, out] Con trỏ trỏ đến chuỗi JSON
*  @return  JsonValue* Con trỏ trỏ đến giá trị "JSON_ARRAY" hoặc NULL nếu lỗi
*/
JsonValue *parse_array(const char **json) 
{
    skip_whitespace(json);                                                // Bỏ qua khoảng trắng  
    if (**json == '[') {                                                  // Kiểm tra ký tự hiện tại có là dấu ngoặc '[' --> để nhận biết bất đầu mảng JSON  
        (*json)++;                                                        // Trỏ đến địa chỉ tiếp theo
        skip_whitespace(json);                                            // Bỏ qua khoảng trắng 

        JsonValue *array_value = (JsonValue *)malloc(sizeof(JsonValue));  // Cấp phát động vùng nhớ cho con trỏ array_value có kiểu dữ liệu JsonValue
        array_value->type = JSON_ARRAY;                                   // Gắn kiểu là "JSON_NUMBER"  
        array_value->value.array.count = 0;                               // Gắn số lượng phần tử là 0  
        array_value->value.array.values = NULL;                           // Gắn giá trị bằng NULL  

        while (**json != ']' && **json != '\0')                           // Duyệt qua từng phần tử của mảng đến ký tự kết thúc mảng ']' hoặc ký tự NULL
        {
            JsonValue *element = parse_json(json);                        // Gọi đến hàm parse_json để kiểm tra phần tử là kiểu gì và lưu vào con trỏ element
            if (element != NULL) 
            {
                array_value->value.array.count++;                         // Nếu phần tử khác NULL --> tăng số lượng phần tử  
                array_value->value.array.values = (JsonValue *)realloc(array_value->value.array.values, array_value->value.array.count * sizeof(JsonValue)); // Thay đổi lại kích thước vùng nhớ cấp phát ra của mảng
                                                                                                                                                             // theo phần tử của nó 
                array_value->value.array.values[array_value->value.array.count - 1] = *element;     // Gắn phần tử ứng với vị trí của nó
                free(element);                                            // Khi gắn giá trị xong sẽ giải tham chiếu cho con trỏ  element     
            } 
            else {
                break;                                                    // Nếu bằng NULL thì thoát khỏi vòng lặp while
            }
            skip_whitespace(json);                                        // Bỏ qua khoảng trắng   
            if (**json == ',')                                            // Xem ký tự tiếp theo là dấu ',' không 
            {
                (*json)++;                                                // Nếu có trỏ đến phần tử tiếp theo  
            }
        }
        if (**json == ']')                                                // Kiểm tra có phải là ký tự kết thúc mảng Json   
        {
            (*json)++;                                                    // Nếu đúng thì cho con trỏ trỏ đến vị trí mới          
            return array_value;                                           // Trả về value có kiểu "JSON_ARRAY"  
        } 
        else 
        {
            free_json_value(array_value);                                 // Thu hồi lại vùng nhớ   
            return NULL;                                                  // Trả về giá trị NULL nếu không hợp lệ                   
        }
    }

    return NULL;
}

/** 
*  @brief   Phân tích object JSON
*  @param   json [in, out] Con trỏ trỏ đến chuỗi JSON
*  @return  JsonValue* Con trỏ trỏ đến giá trị "JSON_OBJECT" hoặc NULL nếu lỗi
*/
JsonValue *parse_object(const char **json) 
{
    skip_whitespace(json);                                                 // Bỏ qua khoảng trắng
    if (**json == '{') {                                                   // Kiểm tra kí tự đầu có phải là {         
        (*json)++;                                                         // Bỏ qua ký tự đầu
        skip_whitespace(json);                                             // Bỏ qua khoảng trắng     

        JsonValue *object_value = (JsonValue *)malloc(sizeof(JsonValue));  // Cấp phát động vùng nhớ cho con trỏ object_value có kiểu dữ liệu JsonValue
        object_value->type = JSON_OBJECT;                                  // Gắn kiểu là "JSON_OBJECT" 
        object_value->value.object.count = 0;                              // Gắn số lượng cặp key - value là 0  
        object_value->value.object.keys = NULL;                            // Gắn giá trị key là NULL
        object_value->value.object.values = NULL;                          // Gắn giá trị value là NULL     

        while (**json != '}' && **json != '\0') {                         // Duyệt qua từng cặp key - value đến ký tự kết thúc mảng '}' hoặc ký tự NULL   
            JsonValue *key = parse_string(json);                          // Phân tích key có phải chuỗi JSON không
            if (key != NULL) {                                            // Nếu không phải NULL thì thực hiện câu lệnh trong ngoặc
                skip_whitespace(json);                                    // Bỏ qua khoảng trắng       
                if (**json == ':')                                        // Kiểm tra ký tự hiện tại có phải : không -> để kiểm tra ký tự bắt đầu của value  
                {                                        
                    (*json)++;
                    JsonValue *value = parse_json(json);                 // Phân tích value thuộc kiểu JSON gì   
                    if (value != NULL)                                   // Nếu không phải NULL thì thực hiện câu lệnh trong ngoặc
                    {    
                        object_value->value.object.count++;             // Tăng thêm số lượng cặp key - value
                        object_value->value.object.keys = (char **)realloc(object_value->value.object.keys, object_value->value.object.count * sizeof(char *)); // Thay đổi lại kích thước vùng nhớ cấp phát ra của object
                                                                                                                                                                // theo key của nó
                        object_value->value.object.keys[object_value->value.object.count - 1] = key->value.string;  // Gắn giá trị chuỗi vào thứ tự key tương ứng

                        object_value->value.object.values = (JsonValue *)realloc(object_value->value.object.values, object_value->value.object.count * sizeof(JsonValue));  // Thay đổi lại kích thước vùng nhớ cấp phát ra của object
                                                                                                                                                                            // theo values của nó
                        object_value->value.object.values[object_value->value.object.count - 1] = *value;   // Gắn giá trị value vào thứ tự mảng values tương ứng
                        free(value);        // giải phóng con trỏ value
                    } else {
                        free_json_value(key); // Giải phóng key nếu value không hợp lệ
                        break;                // Thoát khỏi vòng lăp  
                    }
                } 
                else 
                {
                    free_json_value(key);   // Giải phóng key nếu json không hợp lệ    
                    break;                  // Thoát khỏi vòng lăp
                }
            } else {
                break;
            }

            skip_whitespace(json);          // Bỏ qua khoảng trắng
            if (**json == ',')              // Kiểm tra ký tự hiện tại có phải dấu ',' không ( Các cặp "key" và "value" ngăn cách nhau bởi dấu ",")
            {
                (*json)++;                  // Trỏ đến  ký tự tiếp theo
            }
        }
        if (**json == '}')                  // Kiểm tra xem đến ký tự kết thúc object chưa    
        {
            (*json)++;                     // Trỏ đến  ký tự tiếp theo                       
            return object_value;           // Trả về con trỏ object_value       
        } 
        else 
        {
            free_json_value(object_value);// Nếu không hợp lệ thì giải phóng object_value
            return NULL;                  // Trả về giá trị NULL 
        }
    }
    return NULL;                         // Trả về giá trị NULL nếu không thấy kí tự bắt đầu
}

/** 
*  @brief   Phân tích tổng quát chuỗi JSON
*  @details Hàm này gọi các hàm parson tương ứng từng loại Json
*  @param   json [in, out] Con trỏ trỏ đến chuỗi JSON
*  @return  JsonValue* Con trỏ trỏ đến giá trị Json hoặc NULL nếu lỗi
*/
JsonValue *parse_json(const char **json)                                 // Con trỏ cấp 2 json giúp thay đổi vị trí nó trỏ đến
{ 
    skip_whitespace(json);                                               // Bỏ qua khoảng trắng

    switch (**json) {                                                    // Kiểm tra ký tự mình trỏ đến là ký tự nào
        case 'n':                                                        // null
            return parse_null(json);                                     // Gọi đến hàm parse_null để phân tích 
        case 't':                                                        // true
        case 'f':                                                        // false    
            return parse_boolean(json);                                  // Gọi đến hàm parse_boolean để phân tích   
        case '\"':                                                        
            return parse_string(json);                                   // Gọi đến hàm parse_string để phân tích   
        case '[':                                                           
            return parse_array(json);                                    // Gọi đến hàm parse_array để phân tích         
        case '{':                                                           
            return parse_object(json);                                   // Gọi đến hàm parse_object để phân tích
        default:
            if (isdigit(**json) || **json == '-')                        // Kiểm tra ký tự đàu là số thực hoặc dấu '-'
            {
                return parse_number(json);                               // Gọi đến hàm parse_number để phân tích   
            } else {
                return NULL;                                             // Trả về NULL nếu lỗi phân tích cú pháp ví dụ gặp kí tự ! @ # $  
            }
    }
}

/** 
*  @brief   Giải phóng bộ nhớ cấp phát cho một JSON
*  @param   json [in] Con trỏ trỏ đến chuỗi JSON
*  @return  void
*/
void free_json_value(JsonValue *json_value) 
{
    if (json_value == NULL) 
    {
        return;                                                              // Nếu json_value = NULL thì không làm gì cả                                               
    }

    switch (json_value->type) 
    {
        case JSON_STRING:
            free(json_value->value.string);                                 // Giải phóng bộ nhớ cấp phát cho con trỏ value.string trong trường hợp là JSON_STRING
            break;                                                          // Thoát khỏi vòng lăp

        case JSON_ARRAY:                                                    // Trong trường hợp là kiểu mảng JSON
            for (size_t i = 0; i < json_value->value.array.count; i++)      // Duyệt qua từng phần tử trong mảng JSON
            {
                free_json_value(&json_value->value.array.values[i]);       // Giải phóng bộ nhớ của các phần tử là con trỏ trong mảng Json
            }
            free(json_value->value.array.values);                          // Giải phóng bộ nhớ của toàn bộ mảng con trỏ sau khi tất cả các phần tử đã được giải phóng. 
            break;                                                         // Thoát khỏi vòng lăp

        case JSON_OBJECT:                                                  // Trong trường hợp là kiểu OBJECT     
            for (size_t i = 0; i < json_value->value.object.count; i++)    // Duyệt qua từng cặp key - value trong JSON
            {
                free(json_value->value.object.keys[i]);                    // Giải phóng các phần tử trong key 
                free_json_value(&json_value->value.object.values[i]);      // Giải phóng các phần tử trong value
            }
            free(json_value->value.object.keys);                           // Giải phóng mảng key lớn
            free(json_value->value.object.values);                         // Giải phóng mảng value lớn      
            break;                                                         // Thoát khỏi vòng lăp

        default:
            break;                                                         // Thoát khỏi vòng lăp các trường hợp khác 
    }
}

/** 
*  @brief   In ra chuỗi JSON
*  @param   json [in] Con trỏ trỏ đến chuỗi JSON
*  @return  void
*/
void display(JsonValue* json_value)
{
    if (json_value != NULL && json_value->type == JSON_OBJECT)                               // Kiểm tra có phải đối tượng JSON không
    {
        // Truy cập giá trị của các trường trong đối tượng JSON
        size_t num_fields = json_value->value.object.count;                                 // Gắn số lượng cặp "key-value" trong đối tượng hiện tại.
        size_t num_fields2 = json_value->value.object.values->value.object.count;           // Gắn số lượng cặp "key-value" hiện tại của đối tượng con trong đối tượng lớn.
        for (size_t i = 0; i < num_fields; ++i)                                             // Duyệt qua từng cặp key - value
        {
            char* key = json_value->value.object.keys[i];                                   // Lấy giá trị của từng phần tử key 
            JsonValue* value = &json_value->value.object.values[i];                         // Lấy giá trị của từng phần tử value        

            JsonType type = (int)(json_value->value.object.values[i].type);                 // Lấy kiểu JSON của value

            if(type == JSON_STRING)
            {
                printf("%s: %s\n", key, value->value.string);                              // Nếu nó là kiểu JSON_STRING thì in ra chuỗi trong value.string                
            }

            if(type == JSON_NUMBER)
            {
                printf("%s: %f\n", key, value->value.number);                             // Nếu nó là kiểu JSON_NUMBER thì in ra số trong value.number  
            }

            if(type == JSON_BOOLEAN)
            {
                printf("%s: %s\n", key, value->value.boolean ? "True":"False");          // Nếu nó là kiểu JSON_BOOLEAN thì in ra true hoặc false
            }

            if(type == JSON_OBJECT)
            {
                printf("%s: \n", key);                                                  // Nếu là đối tượng thì gọi lại hàm display và in ra đối tượng con
                display(value);
            }

            if(type == JSON_ARRAY)
            {
                printf("%s: ", key);
                for (int i = 0; i < value->value.array.count; i++)
                {
                   display(value->value.array.values + i);                            // Nếu nó là kiểu JSON_ARRAY thì in lần lượt từng phần tử mảng  
                } 
                printf("\n");
            }
        }
    }
    else 
    {   
         // Truy cập giá trị của các trường không phải đối tượng JSON
        if(json_value->type == JSON_STRING)
        {
            printf("%s ", json_value->value.string);
        }

        if(json_value->type == JSON_NUMBER)
        {
            printf("%f ", json_value->value.number);
        }

        if(json_value->type == JSON_BOOLEAN)
        {
            printf("%s ", json_value->value.boolean ? "True":"False");
        }

        if(json_value->type == JSON_ARRAY)
            {
                for (int i = 0; i < json_value->value.array.count; i++)
                {
                   display(json_value->value.array.values + i);                            
                } 
                printf("\n");
            }
    }
}
```
</details> 

--------------------------------------------------------------------------------------------------------------------------------------------------------------

<details>
<summary><b>main.c</b></summary>
  
```c
#include "JSONParser.h"

int main(int argc, char const *argv[])
{
     // Chuỗi JSON đầu vào
    
    const char* json_str = "{"
                              "\"1001\":{"
                                "\"SoPhong\":3,"
                                "\"NguoiThue\":{"
                                  "\"Ten\":\"Nguyen Van A\","
                                  "\"CCCD\":\"1920517781\","
                                  "\"Tuoi\":26,"
                                  "\"ThuongTru\":{"
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
                                "\"TienDien\": [24, 56, 98],"
                                "\"TienNuoc\":30.000"
                              "},"
                              "\"1002\":{"
                                "\"SoPhong\":5,"
                                "\"NguoiThue\":{"
                                  "\"Ten\":\"Phan Hoang Trung\","
                                  "\"CCCD\":\"012345678912\","
                                  "\"Tuoi\":24,"
                                  "\"ThuongTru\":{"
                                    "\"Duong\":\"53 Le Dai Hanh\","
                                    "\"Phuong_Xa\":\"Phuong 11\","
                                    "\"Tinh_TP\":\"Ho Chi Minh\""
                                  "}"
                                "},"
                                "\"SoNguoiO\":{"
                                  "\"1\":\"Phan Van Nhat\","
                                  "\"2\":\"Phan Van Nhi\","
                                  "\"2\":\"Phan Van Tam\","
                                  "\"3\":\"Phan Van Tu\""
                                "},"
                                "\"TienDien\":23.000,"
                                "\"TienNuoc\":40.000"
                              "}"
                            "}";
    
    // Phân tích cú pháp chuỗi JSON
    JsonValue* json_value = parse_json(&json_str);

   display(json_value);

    // Kiểm tra kết quả phân tích cú pháp

       // Giải phóng bộ nhớ được cấp phát cho JsonValue
    free_json_value(json_value);
    
    return 0;
}
```
</details> 
  
