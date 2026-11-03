# BÀI 6: BẢO MẬT TOÀN DIỆN CHO MÁY TÍNH LẬP TRÌNH VIÊN

## 1. THIẾT LẬP KÉT SẮT SỐ BITWARDEN

### 1.1. Mục đích

Bitwarden là công cụ quản lý mật khẩu giúp lưu trữ, tạo và quản lý mật khẩu an toàn. Người dùng chỉ cần ghi nhớ một Master Password để truy cập kho mật khẩu.

### 1.2. Các bước thực hiện

Bước 1: Truy cập [https://bitwarden.com/](https://bitwarden.com/) và đăng ký tài khoản.

Bước 2: Tạo Master Password có độ dài tối thiểu 20 ký tự hoặc sử dụng cụm mật khẩu gồm 5–6 từ ngẫu nhiên.

Bước 3: Đảm bảo Master Password:

- Không trùng với mật khẩu tài khoản khác.
- Không sử dụng tên hoặc ngày sinh dễ đoán.
- Không chia sẻ cho người khác.

Bước 4: Cài đặt tiện ích Bitwarden trên trình duyệt Chrome.

Bước 5: Sử dụng Password Generator để tạo mật khẩu riêng cho GitHub, Email và các tài khoản khác.

**Lưu ý:** Master Password là chìa khóa chính để mở kho mật khẩu nên cần được bảo vệ cẩn thận.

## 2. KÍCH HOẠT 2FA BẰNG TOTP

### 2.1. Mục đích

2FA (Two-Factor Authentication) là phương thức xác thực hai yếu tố, yêu cầu người dùng xác minh thêm ngoài mật khẩu.

Nên sử dụng Google Authenticator hoặc Microsoft Authenticator để tạo mã TOTP.

### 2.2. Các bước thực hiện

Bước 1: Đăng nhập GitHub.

Bước 2: Chọn ảnh đại diện → Settings.

Bước 3: Chọn Password and authentication.

Bước 4: Tìm Two-factor authentication → Enable two-factor authentication.

Bước 5: Chọn Authenticator app.

Bước 6: Mở Google Authenticator trên điện thoại → nhấn dấu cộng (+) → quét mã QR trên GitHub.

Bước 7: Nhập mã OTP gồm 6 chữ số hiển thị trên ứng dụng để xác nhận.

Bước 8: Lưu bộ Backup Codes mà GitHub cung cấp.

### 3.3. Quản lý Backup Codes

Backup Codes là các mã dự phòng sử dụng khi mất điện thoại hoặc không thể truy cập ứng dụng Authenticator.

**Nơi nên lưu:**

- Bitwarden trong kho dữ liệu được mã hóa.
- Bản giấy cất trong két sắt hoặc tủ khóa an toàn.

**Nơi không nên lưu:**

- Ảnh chụp màn hình trong điện thoại.
- Tin nhắn Zalo, Messenger.
- Email hoặc file công khai trên GitHub.

Không chia sẻ Backup Codes cho người khác. Mỗi mã chỉ sử dụng được một lần.

## 3. QUY TRÌNH ĐẨY CODE LÊN GIT SẠCH SECRETS

### 3.1. Mục đích

Secrets là các thông tin bí mật như API Key, mật khẩu cơ sở dữ liệu và Access Token. Nếu đưa lên GitHub công khai, người khác có thể sử dụng chúng để truy cập trái phép.

### 3.2. Tạo file .env

File `.env` dùng để lưu các giá trị bí mật trên máy tính cá nhân.

Ví dụ:

```
API_KEY=your_real_api_key
DB_PASSWORD=your_real_database_password
```

Các giá trị trên chỉ là ví dụ minh họa. Khi thực hiện cần thay bằng thông tin bí mật thực tế.

### 3.3. Sửa code sử dụng process.env

File `config.js`:

```
const API_KEY = process.env.API_KEY;
const DB_PASSWORD = process.env.DB_PASSWORD;

function connectDatabase() {
  return connect(DB_PASSWORD);
}
```

Code không chứa trực tiếp API Key hoặc mật khẩu.

### 3.4. Tạo file .gitignore

File `.gitignore` giúp Git bỏ qua các file không được phép đưa lên GitHub.

```
# Environment variables
.env
.env.*
!.env.example

# Dependencies
node_modules/

# Log files
*.log
```

### 3.5. Tạo file .env.example

File này giúp các thành viên biết dự án cần những biến môi trường nào.

```
API_KEY=
DB_PASSWORD=
```

Không điền giá trị Secret thật vào file này.

### 3.6. Quy trình kiểm tra và đẩy code lên GitHub

Bước 1: Kiểm tra code, đảm bảo không chứa mật khẩu hoặc API Key thật.

Bước 2: Kiểm tra `.gitignore` đã có dòng `.env`.

Bước 3: Kiểm tra các file chuẩn bị đưa lên GitHub:

```
git status
```

Bước 4: Commit và Push:

```
git add .
git commit -m "Protect project secrets"
git push
```

**Lưu ý:** Không push file `.env` lên GitHub. Chỉ push `.env.example` để chia sẻ tên các biến môi trường.

Nếu Secrets đã từng được push lên GitHub, cần thay mới hoặc thu hồi các thông tin đó, không chỉ xóa chúng khỏi code.

## 4. CÁC BIỆN PHÁP BẢO MẬT KHI LÀM VIỆC TẠI QUÁN CÀ PHÊ

1. Ưu tiên sử dụng mạng cá nhân hoặc điểm phát Wi-Fi từ điện thoại.
2. Nếu sử dụng Wi-Fi công cộng, chỉ truy cập website HTTPS và cân nhắc sử dụng VPN đáng tin cậy.
3. Không đăng nhập tài khoản quan trọng trên máy tính công cộng.
4. Khóa màn hình bằng tổ hợp phím Windows + L khi rời khỏi máy.
5. Không cắm USB không rõ nguồn gốc.
6. Cập nhật Windows, trình duyệt và phần mềm diệt virus thường xuyên.
7. Sử dụng mã PIN hoặc sinh trắc học để bảo vệ máy tính.

## 5. KẾT LUẬN

Để bảo vệ máy tính và dữ liệu dự án, Freelancer cần thực hiện ba giải pháp chính:

- **Bitwarden:** Quản lý mật khẩu riêng biệt và an toàn.
- **2FA/TOTP:** Tăng cường bảo mật tài khoản, hạn chế nguy cơ bị chiếm đoạt.
- **`.env` và `.gitignore`:** Bảo vệ API Key, mật khẩu và các thông tin bí mật trước khi đưa code lên GitHub.

Bên cạnh đó, cần chú ý bảo mật thiết bị và mạng Internet khi làm việc tại quán cà phê.

**Kết quả mong muốn:** Máy tính được bảo vệ tốt hơn, tài khoản được quản lý an toàn và mã nguồn có thể đưa lên GitHub mà không làm lộ thông tin bí mật.

Thêm mục kiểm tra Secret trước khi pushSửa lệnh Git cho lần push đầu tiên
