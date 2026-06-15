# Research Group Management System - UEH

Research Group Management System là nền tảng quản lý hồ sơ đăng ký, thẩm định và phê duyệt các nhóm nghiên cứu tại UEH. Hệ thống chạy trên Laravel 12, phục vụ ba nhóm vai trò chính: giảng viên/người khai hồ sơ, RDGE và Ban Giám đốc.

## Mục tiêu hệ thống

- Quản lý hồ sơ đăng ký nhóm nghiên cứu theo từng loại nhóm: nhóm quốc tế, nhóm công nghệ/đổi mới sáng tạo và nhóm chính sách.
- Cho phép giảng viên hoặc người khai hộ nhập hồ sơ, bổ sung thuyết minh, danh sách thành viên, sản phẩm khoa học, kế hoạch hoạt động và kinh phí đề xuất.
- Hỗ trợ RDGE xét duyệt sơ bộ, thẩm định thuyết minh, đánh giá trưởng nhóm/đồng trưởng nhóm và trình hồ sơ sang BGĐ.
- Hỗ trợ xuất hồ sơ ra Word/PDF theo template và gửi PDF sang hệ thống ký số UEH.
- Theo dõi lịch sử gửi ký số, người ký, trạng thái ký và link mở file trên Sign UEH.

## Vai trò người dùng

### Lecturer

Lecturer là giảng viên hoặc người khai hồ sơ. Một user lecturer có thể:

- Tạo hồ sơ đăng ký nhóm nghiên cứu.
- Khai thông tin trưởng nhóm, đồng trưởng nhóm, thành viên và CV.
- Hoàn thiện thuyết minh sau khi RDGE duyệt sơ bộ.
- Xem trạng thái xử lý hồ sơ.
- Xuất Word/PDF hồ sơ theo mẫu.
- Gửi hồ sơ đi ký số khi hồ sơ đã qua bước RDGE thẩm định đạt.

Hệ thống cho phép người A khai hộ cho người B. Nếu email trưởng nhóm trong hồ sơ trùng với email đăng nhập của user B, user B vẫn nhìn thấy hồ sơ của mình trên dashboard lecturer.

### RDGE

RDGE là vai trò quản lý, xét duyệt và thẩm định hồ sơ. User RDGE có thể:

- Xem dashboard tổng hợp hồ sơ.
- Tìm kiếm hồ sơ theo mã, tên nhóm, tên tiếng Anh hoặc trưởng nhóm.
- Xem chi tiết hồ sơ.
- Xét duyệt sơ bộ hồ sơ đăng ký ban đầu.
- Trả hồ sơ về để chỉnh sửa nếu chưa đạt.
- Thẩm định thuyết minh hoàn chỉnh.
- Đánh giá trưởng nhóm và đồng trưởng nhóm.
- Chọn người ký bước 2 và bước 3.
- Gửi hồ sơ PDF sang hệ thống ký số UEH.
- Xem báo cáo tóm tắt các hồ sơ đã qua bước RDGE trình BGĐ.
- Kích hoạt nhóm sau khi BGĐ phê duyệt.

### BGĐ

BGĐ là vai trò phê duyệt cấp cuối. User BGĐ có thể:

- Xem danh sách hồ sơ RDGE đã trình.
- Xem chi tiết hồ sơ.
- Phê duyệt hồ sơ.
- Từ chối hồ sơ.
- Yêu cầu chỉnh sửa/bổ sung hồ sơ.

## Luồng xử lý hồ sơ

### Bước 1: Lecturer nộp hồ sơ đăng ký ban đầu

Lecturer khai thông tin nhóm nghiên cứu, trưởng nhóm, đồng trưởng nhóm/thành viên quốc tế nếu có, thành viên và CV. Khi nộp hồ sơ, trạng thái chuyển sang:

```text
submitted
```

Ý nghĩa: hồ sơ đang chờ RDGE xét duyệt sơ bộ.

### Bước 2: RDGE xét duyệt sơ bộ

RDGE xem thông tin đăng ký ban đầu và quyết định:

- Duyệt sơ bộ: trạng thái chuyển sang `rdge_approved`.
- Trả về/từ chối: trạng thái chuyển sang `rdge_rejected`.

Khi hồ sơ ở trạng thái `rdge_approved`, lecturer có thể vào phần hoàn thiện thuyết minh.

### Bước 3: Lecturer hoàn thiện thuyết minh

Lecturer bổ sung nội dung thuyết minh chi tiết: định hướng nghiên cứu, mô tả nghiên cứu, danh sách thành viên chi tiết, kế hoạch hoạt động, sản phẩm, kinh phí và các thông tin theo loại nhóm.

Khi nộp thuyết minh, trạng thái chuyển sang:

```text
proposal_submitted
```

Ý nghĩa: thuyết minh đang chờ RDGE thẩm định.

### Bước 4: RDGE thẩm định thuyết minh

RDGE kiểm tra hồ sơ hoàn chỉnh và có thể:

- Thẩm định đạt và trình BGĐ: trạng thái chuyển sang `pending_bgd`.
- Yêu cầu chỉnh sửa: trạng thái chuyển sang `proposal_revision`.

Nếu `proposal_revision`, lecturer chỉnh sửa và nộp lại thuyết minh.

### Bước 5: Trình ký số

Hồ sơ chỉ được gửi ký số khi trạng thái là:

```text
pending_bgd
```

Điều kiện này có nghĩa là hồ sơ đã được RDGE thẩm định đạt và đã đến bước trình BGĐ.

Luồng ký số hiện tại gồm:

1. Trưởng nhóm.
2. Ban Giám hiệu trường thành viên hoặc Ban Giám đốc phân hiệu.
3. Lãnh đạo Ban RDGE.

Người gửi ký số chọn người ký bước 2 và bước 3 từ danh sách người ký đã cấu hình trong RDGE. Người phụ trách ký số được lấy từ biến môi trường `UEH_SIGN_RESPONSIBLE_PEOPLE`; nếu biến này không được cấu hình, hệ thống dùng user đang đăng nhập làm người phụ trách.

File PDF gửi sang Sign UEH được tạo từ template Word và đặt tên theo cấu trúc:

```text
ma-ho-so-yyyymmdd-hhmmss.pdf
```

Ví dụ:

```text
irl-059-20260615-181410.pdf
```

### Bước 6: BGĐ phê duyệt

BGĐ xử lý hồ sơ ở trạng thái `pending_bgd` và có thể:

- Phê duyệt: trạng thái chuyển sang `bgd_approved`.
- Từ chối: trạng thái chuyển sang `bgd_rejected`.
- Yêu cầu bổ sung: trạng thái chuyển sang `bgd_revision`.

Nếu `bgd_revision`, lecturer chỉnh sửa thuyết minh và nộp lại.

### Bước 7: RDGE kích hoạt nhóm

Sau khi BGĐ phê duyệt, RDGE có thể kích hoạt nhóm. Trạng thái cuối là:

```text
active
```

Ý nghĩa: nhóm nghiên cứu đã được công nhận/kích hoạt trong hệ thống.

## Các trạng thái chính

| Trạng thái | Ý nghĩa |
| --- | --- |
| `draft` | Hồ sơ nháp |
| `submitted` | Chờ RDGE xét duyệt sơ bộ |
| `rdge_approved` | RDGE đã duyệt sơ bộ |
| `rdge_rejected` | RDGE trả về/từ chối hồ sơ ban đầu |
| `proposal_submitted` | Thuyết minh chờ RDGE thẩm định |
| `proposal_revision` | RDGE yêu cầu bổ sung thuyết minh |
| `pending_bgd` | RDGE thẩm định đạt và trình BGĐ |
| `bgd_revision` | BGĐ yêu cầu bổ sung |
| `bgd_approved` | BGĐ đã phê duyệt |
| `bgd_rejected` | BGĐ không phê duyệt |
| `active` | Nhóm đã được kích hoạt |

## Xuất Word/PDF

Hệ thống xuất hồ sơ theo template Word tương ứng với từng loại nhóm:

- `international_group_template.docx`
- `technology_group_template.docx`
- `policy_group_template.docx`

File Word/PDF xuất ra dùng tên:

```text
ma-ho-so-yyyymmdd-hhmmss.docx
ma-ho-so-yyyymmdd-hhmmss.pdf
```

Một số placeholder ngày tháng trong template:

```text
{{ngay_xuat}}
{{ngay_xuat_ngay}}
{{ngay_xuat_thang}}
{{ngay_xuat_nam}}
```

Ví dụ trong template:

```text
ngày {{ngay_xuat_ngay}} tháng {{ngay_xuat_thang}} năm {{ngay_xuat_nam}}
```

## Tích hợp ký số UEH

API ký số được gọi qua service:

```text
app/Services/UehSignService.php
```

Controller gửi ký số:

```text
app/Http/Controllers/Rdge/GroupSigningController.php
```

Callback ký số:

```text
app/Http/Controllers/UehSignCallbackController.php
```

Route callback:

```text
/signing/ueh/callback
```

Các biến môi trường liên quan:

```env
UEH_SIGN_ENDPOINT=https://sign.ueh.edu.vn/api/TrinhKy
UEH_SIGN_CLIENT_ID=
UEH_SIGN_CLIENT_KEY=
UEH_SIGN_CALLBACK_URL=
UEH_SIGN_RESPONSIBLE_PEOPLE="email1@ueh.edu.vn|Họ Tên 1,email2@ueh.edu.vn|Họ Tên 2"
LIBREOFFICE_BIN=
```

Nếu Sign UEH trả file PDF thật về callback, hệ thống sẽ lưu file đã ký vào server. Nếu Sign UEH chỉ trả trang xem file, hệ thống hiển thị link mở file trên Sign UEH.

## Gợi ý nhân sự UEH

Hệ thống có chức năng gợi ý nhân sự từ bảng `dsnhansu` khi nhập tên trưởng nhóm, đồng trưởng nhóm, thành viên hoặc người ký. Khi chọn đúng nhân sự, hệ thống tự điền các thông tin như email, số điện thoại, học hàm/học vị, đơn vị và mã quản lý nếu dữ liệu có sẵn.

## Ghi chú triển khai

Sau khi cập nhật code trên server, thường cần chạy:

```bash
php artisan migrate
php artisan config:clear
php artisan view:clear
```

Nếu dùng chức năng xuất PDF, server cần cấu hình LibreOffice qua biến:

```env
LIBREOFFICE_BIN=
```
