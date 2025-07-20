📓AUTOSAR CLASSIC📓

<details> 
<summary><b>📖 1. Khái niệm</b></summary>

- **AUTOSAR (Automotive Open System Architecture)** là một tiêu chuẩn quốc tế về **kiến trúc phần mềm** cho các hệ thống điện tử trong ô tô
- Ưu điểm:<br> 
&nbsp;+ Có sẵn các tiêu chuẩn để dựa vào.<br>
&nbsp;+ Khả năng tái sử dụng phần mềm cao với các dự án khác nhau.<br>
&nbsp;+ Dễ dàng thay đổi để tương thích với các dòng MCU khác nhau.<br>
&nbsp;+ Phần mềm và phần mềm được tách biệt với nhau.<br>
&nbsp;+ Dễ quản lý và bảo trì phần mềm.<br>
- Có hai phiên bản AUTOSAR:<br>
&nbsp;+  Classic Platform: Dành cho các hệ thống thời gian thực, ECU truyền thống (C, embedded).<br>
&nbsp;+ Adaptive Platform: Dành cho các hệ thống hiệu suất cao (Linux, POSIX), như xe tự lái
 </details>
 
------------------------------------------------------------------------------------------------------------------------------------------------

<details>

<summary><b>📖 2. Kiến trúc Classic AUTOSAR</b></summary>

- **Classic AUTOSAR** gồm có 3 lớp chính:
<img width="952" height="411" alt="image" src="https://github.com/user-attachments/assets/c9990915-060d-453c-b44c-3dd5a4115f1c" />

### 2.1. SWC - Software Components
- **SWC (Software-Components)** là các khối phần mềm ứng dụng nằm trong **Application Layer** đại diện cho các chức năng cụ thể trong hệ thống hay MCU ( nó tương đương như 1 task).
- Các SWC là thành phần độc lập, giao tiếp với nhau và với các thành phần khác trong hệ thống thông qua RTE: <br>
&nbsp;+ Mỗi SWC thực hiện một chức năng cụ thể trong hệ thống ECU.<br>
&nbsp;+ Chỉ cần quan tâm đến các logic, không cần quan tâm đến phần cứng

### 2.2. RTE - Runtime Enviroment
- **RTE (Runtime Enviroment)** là lớp trung gian, đảm nhiệm việc truyền các SWC và giữa SWC với BSW. Nó đảm bảo rằng các SWC có thể giao tiếp với nhau một các trong suốt, không cần biết về các cơ chế truyền thông thực tế. RTE có 2 chức năng chính:<br>
&nbsp;+ Giúp các SWC giao tiếp với nhau và là lớp trung gian với BSW.<br>
&nbsp;+ Phân chia lịch trình và quản lý việc gọi các chức năng.<br>

### 2.3. BSW - Basic Software
- **BSW (Basic Software)** là lớp phần mềm nền tảng để hỗ trợ phần mềm ứng dụng(SWC) hoạt động dựa trên phần cứng.
- BSW cung cấp các dịch vụ cơ bản như quản lý phần cứng, giao tiếp, chuẩn đoán và các dịch vụ hệ thống:<br>
&nbsp;- **Service:** Cung cấp các dịch vụ hệ thống, tiện ích và quản lý cần thiết để hỗ trợ các lớp phần mềm ứng dụng và BSW khác:<br>
&nbsp;&nbsp;&nbsp;+ **System Service:** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Chức năng: Là nơi chứa hệ điều hành và nó cung cấp dịch vụ hệ thống cơ bản để ECU hoạt động ổn định và đồng bộ.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Gồm các module:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**OS (Operating System):** Quản lý task, tài nguyên, sự kiện, interrupt.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**WDGM (Watchdog Manager):** Giám sát hệ thống, tránh treo chương trình và tương tác với watchdog driver (Wdg) ở tầng MCAL.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**ECUM (ECU Manager):** Quản lý trạng thái hoạt động của ECU (sleep, startup, shutdown).<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**BSWM (BSW Mode Manager):** Điều phối chế độ hoạt động giữa các module BSW mà không dùng OS task.<br>
&nbsp;&nbsp;&nbsp;+ **Memory service:** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Chức năng: Cung cấp dịch vụ quản lý các thao tác đọc/ghi/xóa dữ liệu trong bộ nhớ không mất (non-volatile memory) như EEPROM, Flash.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Gồm các module:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**NvM (Non-Volatile RAM Manager):** Quản lý đọc/ghi dữ liệu lưu trữ không mất (non-volatile).<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**EA (EEPROM Abstraction):** Cung cấp giao diện truy cập đến EEPROM..<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FEE	(Flash EEPROM Emulation):** Mô phỏng EEPROM trên Flash (dùng khi không có EEPROM thật).<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**MemIf	(Memory Abstraction Interface):** Cung cấp một giao diện thống nhất để truy cập giữa NvM và các tầng lưu trữ bên dưới (Ea hoặc Fee)..<br>
&nbsp;&nbsp;&nbsp;+ **Crypto service:** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Chức năng: Cung cấp các dịch vụ mã hóa, xác thực, kiểm tra tính toàn vẹn dữ liệu, đảm bảo an toàn thông tin.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Gồm các module:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Csm	(Crypto Service Manager):** Điều phối các dịch vụ mật mã, cung cấp API chung cho các module khác.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Crypto Stack (Crypto Software Stack):** Thực hiện thực tế các thuật toán mã hóa (AES, RSA, SHA, ECC...), có thể là phần mềm hoặc phần cứng (HSM).<br>
&nbsp;&nbsp;&nbsp;+ **Off-Board Communication:** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Chức năng: Quản lý truyền tải không dây tới module khác.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Gồm các module:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**DCM	(Diagnostic Communication Manager):** Quản lý giao tiếp chẩn đoán UDS (ISO 14229), xử lý yêu cầu từ máy chẩn đoán ngoài xe.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**DoIP	(Diagnostics over IP):** Giao tiếp chẩn đoán qua giao thức IP/Ethernet thay vì CAN/LIN.<br>
&nbsp;&nbsp;&nbsp;+ **Communication Service:** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Chức năng: Cung cấp dịch vụ quản lý truyền thông nội bộ ECU và giữa các ECU thông qua các giao thức như CAN, LIN, FlexRay, Ethernet...<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;* Gồm các module:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Com	:** Quản lý giao tiếp tín hiệu giữa Application và PDU.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**PduR	(PDU Router):** Định tuyến PDU (Protocol Data Unit) giữa các module như Com, DCM, CanTp, SoAd...<br>
&nbsp;- **EAL (ECU Abstraction Layer):** Cung cấp giao diện trừu tượng cho tất cả các thiết bị ngoại vi và phần cứng cụ thể của ECU như các cảm biến mà ECU sử dụng.<br>
&nbsp;&nbsp;&nbsp;+ **Onboard Device Abstraction:** Trừu tượng hóa các thiết bị phần cứng tích hợp trên ECU.<br>
&nbsp;&nbsp;&nbsp;+ **Memory Hardware Abstraction:** Cung cấp lớp trừu tượng để giao tiếp với phần cứng bộ nhớ như EEPROM, Flash, RAM, giúp phần mềm cấp cao (như NvM, Fee, Ea…) không cần biết chi tiết phần cứng.<br>
&nbsp;&nbsp;&nbsp;+ **Crypto Hardware Abstraction:** Cung cấp lớp trừu tượng hóa phần cứng cho các chức năng mã hóa, giải mã, ký số, xác minh, băm (hash),...<br>
&nbsp;&nbsp;&nbsp;+ **Wireless Communication Hardware Abstraction:** Trừu tượng hóa phần cứng truyền thông không dây như Bluetooth, Wi-Fi,... giúp các tầng cao hơn (Protocol stacks, Diagnostic, Telematics) không cần quan tâm đến phần cứng cụ thể..<br>
&nbsp;&nbsp;&nbsp;+ **Communication Hardware Abstraction:** Trừu tượng phần cứng cho các giao thức truyền thông như CAN, LIN, FlexRay, Ethernet, để các tầng phần mềm bên trên (như Com, PduR, SoAd...) không cần quan tâm đến chi tiết phần cứng hoặc driver..<br>
&nbsp;&nbsp;&nbsp;+ **I/O Hardware Abstraction:**  trừu tượng hóa việc truy cập các chân I/O phần cứng (digital & analog) như cảm biến, công tắc,... Giúp phần mềm ứng dụng không phụ thuộc trực tiếp vào phần cứng MCU.<br>
&nbsp;- **MCAL (Microcontroller abstraction Layer):** Cung cấp giao diện trừu tượng để tương tác trực tiếp với các thành phần phần cứng của vi điều khiển như GPIO, ADC, PWM, ...<br>
&nbsp;&nbsp;&nbsp;+ **Microcontroller Drivers:**  Cung cấp các driver trực tiếp cho phần cứng mà không cần thao tác trực tiếp với thanh ghi như: GPIO, ADC, PWM, SPI,...<br>
&nbsp;&nbsp;&nbsp;+ **Memory Drivers:** Dùng để giao tiếp trực tiếp với bộ nhớ vật lý: Flash, RAM, EEPROM.<br>
&nbsp;&nbsp;&nbsp;+ **Crypto Drivers:** Giao tiếp trực tiếp với phần cứng mã hóa.<br>
&nbsp;&nbsp;&nbsp;+ **Wireless Communication Drivers:** Giao tiếp với các thiết bị truyền thông không dây như: Wifi, Bluetooth,...<br>
&nbsp;&nbsp;&nbsp;+ **Communication Drivers:** Giao tiếp trực tiếp với phần cứng truyền thông: CAN, Ethernet,...<br>
&nbsp;&nbsp;&nbsp;+ **I/O Drivers:** Truy cập trực tiếp các chân I/O của vi điều khiển, giúp hệ thống điều khiển các thiết bị ngoại vi.<br>
&nbsp;- **Complex Drivers:** Dùng cho các ngoại lệ/phần cứng phức tạp không chuẩn AUTOSAR
