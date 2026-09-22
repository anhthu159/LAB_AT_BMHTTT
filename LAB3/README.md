# LAB3 
Nguyễn Thị Anh Thư
MSSV: 1150080159

## 2. Môi trường thực hành

- Nền tảng ảo hóa: VMware Workstation
- Máy ảo: Windows 11 25H2
- OS Build: 26200.8037
- Kiến trúc: 64-bit
- Network Adapter: Host-only
- Microsoft Defender Antivirus: Enabled
- Windows Firewall: Enabled

### Công cụ sử dụng

- Python: 3.14.7
- Wireshark / TShark: 4.6.8
- Sysmon: 15.22
- Autoruns: 14.3
- Process Explorer: 17.14
- Windows PowerShell 5.1
- Windows Event Viewer

## 3. Cách dựng môi trường

1. Tạo máy ảo Windows 11 trên VMware Workstation.
2. Cấu hình Network Adapter ở chế độ Host-only để cô lập môi trường thực hành.
3. Tạo thư mục làm việc:

   C:\LAB3

4. Tạo các thư mục con:

   - C:\LAB3\Evidence
   - C:\LAB3\Tools
   - C:\LAB3\Downloads
   - C:\LAB3\Assets

5. Sao chép và giải nén gói LAB3_Threats_Assets do giảng viên cung cấp.
6. Cài đặt Python 3.14.7 và Wireshark 4.6.8.
7. Tải bộ Microsoft Sysinternals gồm:
   - Sysmon
   - Autoruns
   - Process Explorer
8. Kiểm tra phiên bản các công cụ trước khi bắt đầu thực hành.
9. Giữ Microsoft Defender và Windows Firewall hoạt động trong suốt quá trình thực hành.

## 4. Các tình huống đã thực hiện

### Baseline hệ thống

Đã thu thập trạng thái ban đầu của máy ảo trước khi tạo các tình huống thử nghiệm.

Các file đã tạo gồm:

- baseline_os.txt
- baseline_defender.txt
- baseline_firewall.txt
- baseline_network.txt
- baseline_processes.txt

Kết quả:

- Microsoft Defender: Enabled
- Real-time Protection: Enabled
- Tamper Protection: Enabled
- Windows Firewall: Enabled

Trạng thái: PASS

### TH1 - Xác định tài sản, lỗ hổng, mối đe dọa và rủi ro

Đã xây dựng Risk Register dựa trên các tài sản trong máy ảo LAB và phân biệt các khái niệm:

- Asset
- Vulnerability
- Threat
- Risk
- Control

Đã phân loại các nguồn đe dọa gồm:

- Hành động vô ý
- Hành động cố ý
- Thảm họa tự nhiên
- Lỗi kỹ thuật
- Lỗi quản lý

Trạng thái: PASS

### TH3 - Tấn công mật khẩu và nguy cơ Keylogging

Đã thực hiện:

- Bật Audit Logon Success/Failure.
- Tạo tài khoản thử nghiệm `lab3user`.
- Thực hiện đăng nhập đúng bằng `runas`.
- Thực hiện đăng nhập sai có kiểm soát.
- Kiểm tra Windows Security Event Log.
- Quan sát Event ID 4624, 4625 và 4648.
- Đổi mật khẩu tài khoản thử nghiệm và kiểm tra lại credential.

File bằng chứng:

- auth_events_before_rotation.txt

Trạng thái: PASS

### TH4 - Nhận diện Persistence và Backdoor

Đã bắt đầu cấu hình Sysmon và kiểm tra khả năng ghi nhận sự kiện hệ thống.

Đã thực hiện:

- Kiểm tra Sysmon 15.22.
- Nạp cấu hình `sysmon-lab.xml`.
- Kiểm tra Sysmon Event Log.
- Chuẩn bị Autoruns baseline phục vụ phát hiện persistence.

Trạng thái: ĐANG THỰC HIỆN / CHƯA HOÀN THÀNH

## 5. Lỗi gặp phải và cách khắc phục

### Lỗi máy ảo không có Internet

Hiện tượng:

- Network Adapter đã chuyển sang NAT nhưng Windows báo `Media disconnected`.
- Không thể tải trực tiếp Sysinternals bằng Invoke-WebRequest.

Cách khắc phục:

- Tải các file Sysinternals từ đúng máy chủ Microsoft trên máy thật.
- Sao chép các file ZIP vào máy ảo.
- Giải nén vào C:\LAB3\Tools.
- Kiểm tra lại version trước khi sử dụng.

### Lỗi auditpol

Hiện tượng:

- Lệnh auditpol ban đầu báo `The parameter is incorrect`.

Cách khắc phục:

- Đặt GUID của subcategory trong dấu ngoặc kép.
- Sau khi sửa, lệnh thực thi thành công.

### Lỗi tạo tài khoản lab3user

Hiện tượng:

- Biến `$pw` chưa có giá trị nên New-LocalUser báo Password is null.
- Sau đó chạy lại New-LocalUser khiến hệ thống báo user đã tồn tại.

Cách khắc phục:

- Tạo SecureString bằng Read-Host.
- Kiểm tra tài khoản bằng Get-LocalUser thay vì tạo lại.

### Lỗi runas không nhận mật khẩu

Hiện tượng:

- `RUNAS ERROR: Unable to acquire user password`.

Cách khắc phục:

- Kiểm tra dịch vụ Secondary Logon.
- Sử dụng tên máy đầy đủ với tài khoản `lab3user`.
- Sau khi điều chỉnh, runas hoạt động và mở được Command Prompt bằng tài khoản thử nghiệm.

### Lỗi Sysmon đã được cài trước đó

Hiện tượng:

- Sysmon báo service đã được đăng ký.

Cách khắc phục:

- Không gỡ Sysmon.
- Sử dụng tùy chọn `-c` để nạp/cập nhật file cấu hình `sysmon-lab.xml`.

