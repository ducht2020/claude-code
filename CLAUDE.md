# CLAUDE.md

Tài liệu này tóm tắt các trang đã xây dựng (`login.html`, `register.html`) và quy định bộ quy tắc thiết kế chung cho mọi trang cơ bản trong tương lai của dự án.

---

## 1. Tổng quan dự án

| Mục | Nội dung |
|---|---|
| Loại dự án | Các trang HTML tĩnh, một file cho mỗi trang |
| Công nghệ | HTML5, Tailwind CSS (CDN), Lucide Icons (CDN), JavaScript thuần |
| Ngôn ngữ giao diện | Tiếng Anh 100% |
| Theme | Dark toàn bộ |
| Trạng thái backend | Chưa kết nối; form chỉ ghi log ra console (có dòng `TODO` để gắn API) |

---

## 2. Tóm tắt các trang

### 2.1. `login.html`: Trang đăng nhập

**Mục đích:** Cho phép người dùng đã có tài khoản đăng nhập vào hệ thống.

**Yêu cầu:**
- Trường nhập Email và Password
- Nút Sign in
- Checkbox "Remember me"
- Liên kết "Forgot password?"
- Đăng nhập nhanh bằng Google / GitHub / Facebook
- Liên kết sang trang đăng ký: "Don't have an account yet? Please register"

**Hành động:**
- Hiện/ẩn mật khẩu bằng nút biểu tượng con mắt
- Khi submit: kiểm tra Email hợp lệ, Password tối thiểu 6 ký tự; hiển thị lỗi đỏ dưới từng trường nếu sai
- Nếu hợp lệ: ghi log `{ email, remember }` ra console (chờ gắn API)

**Kết quả:** Trang đăng nhập dark theme, responsive; ba nút social xếp ngang trên tablet/PC và xếp dọc trên điện thoại. Lucide không có logo Google nên nút Google dùng SVG riêng.

### 2.2. `register.html`: Trang đăng ký

**Mục đích:** Cho phép người dùng mới tạo tài khoản.

**Các trường (theo thứ tự hiển thị):**

| # | Trường | Bắt buộc | Ràng buộc |
|---|---|---|---|
| 1 | Full name | Có | Không để trống |
| 2 | Email | Có | Đúng định dạng email |
| 3 | Password | Có | Tối thiểu 6 ký tự |
| 4 | Confirm password | Có | Phải trùng Password |
| 5 | Gender | Không | Chọn Male / Female / Other |
| 6 | Phone number | Không | Nếu nhập: đúng 10 chữ số, chỉ nhận số |
| 7 | Address | Không | Tự do |
| 8 | Terms checkbox | Có | Phải được tick |

**Hành động:**
- Hiện/ẩn mật khẩu cho cả hai trường mật khẩu
- Ô số điện thoại tự loại bỏ ký tự không phải số và giới hạn 10 ký tự
- Khi submit: kiểm tra toàn bộ các ràng buộc ở trên, hiển thị lỗi đỏ dưới từng trường sai
- Nếu hợp lệ: ghi log dữ liệu ra console (chờ gắn API)

**Kết quả:** Trang đăng ký dark theme; form hai cột trên tablet/PC, một cột trên điện thoại. Có liên kết "Back to login" quay về `login.html`.

### 2.3. Liên kết giữa hai trang

| Từ | Đến | Văn bản liên kết |
|---|---|---|
| `login.html` | `register.html` | "Don't have an account yet? Please register" |
| `register.html` | `login.html` | "Back to login" |

Các file phải nằm cùng một thư mục để liên kết tương đối hoạt động.

---

## 3. Quy tắc thiết kế chung cho các trang trong tương lai

Mọi trang cơ bản mới (ví dụ: quên mật khẩu, đặt lại mật khẩu, hồ sơ cá nhân...) **phải** tuân theo các quy tắc dưới đây để đảm bảo tính nhất quán.

### 3.1. Theme màu sắc và font chữ

**Màu sắc** (khai báo trong `tailwind.config`):

| Tên | Mã màu | Dùng cho |
|---|---|---|
| `bg` | `#090A14` | Nền trang |
| `primary` | `#DF6B33` | Màu chính, nút CTA, liên kết, focus |
| `surface` | `#12131F` | Thẻ (card) |
| `surface2` | `#1A1C2B` | Ô nhập liệu, nút phụ |
| `line` | `#262839` | Viền, đường phân cách |

- Văn bản: tiêu đề `text-white`, nội dung `text-gray-300`, phụ trợ `text-gray-400/500`
- Lỗi: `text-red-400` cho thông báo, `border-red-500` cho ô nhập sai
- Toàn bộ trang là dark theme: thẻ `<html>` có `class="dark"`, `color-scheme: dark`, không dùng nền sáng ở bất kỳ đâu (kể cả autofill và `<option>`)

**Font chữ:**
- Tiêu đề và nút CTA: **Poppins** bold/extrabold (700–800), class `font-heading`
- Nội dung: **Inter** regular (400)
- Tải qua Google Fonts: `Inter:wght@400` và `Poppins:wght@700;800`

### 3.2. Bố cục và responsive

- Cách tiếp cận mobile-first; thêm `<meta name="viewport" content="width=device-width, initial-scale=1.0">`
- Nội dung đặt trong thẻ giữa trang: `max-w-md` cho form ngắn, `md:max-w-2xl` cho form nhiều trường
- Lưới trường: `grid-cols-1` trên điện thoại, `md:grid-cols-2` trên tablet/PC khi cần
- Khoảng cách ngang `px-4 sm:px-6`; cỡ chữ `text-sm sm:text-base`
- Nút và ô nhập đủ lớn để chạm trên điện thoại (`py-3`)
- Cần kiểm tra bố cục ở ba mức: điện thoại (~375px), tablet (~768px), PC (≥1024px)

### 3.3. Lucide Icons

- Dùng Lucide qua CDN đã ghim phiên bản `lucide@0.468.0` (phiên bản này còn có `github` và `facebook`)
- Cú pháp: `<i data-lucide="tên-icon" class="h-5 w-5"></i>`, gọi `lucide.createIcons()` khi tải trang
- Khi đổi icon bằng JavaScript (ví dụ eye / eye-off), phải gọi lại `lucide.createIcons()`
- Mỗi ô nhập có icon dẫn đầu bên trái (mail, lock, user, phone, map-pin...); nút hành động có icon (arrow-right, arrow-left...)
- Nút chỉ có icon phải có `aria-label`
- Logo thương hiệu mà Lucide không có (như Google) dùng SVG nội tuyến

### 3.4. Ràng buộc nhập liệu

- Trường bắt buộc: đánh dấu `*` màu `primary` cạnh nhãn; trường tùy chọn ghi chú `(optional)`
- Dùng đúng `type` và `autocomplete` cho từng trường (`email`, `password`, `tel`, `new-password`, `current-password`...)
- Quy tắc tối thiểu:
  - Email: đúng định dạng
  - Mật khẩu: tối thiểu 6 ký tự; trang đăng ký có thêm ô xác nhận phải trùng khớp
  - Số điện thoại: đúng 10 chữ số, chỉ nhận số (`inputmode="numeric"`, `maxlength="10"`)
  - Checkbox điều khoản: bắt buộc tick
- Kiểm tra khi submit bằng JavaScript (`novalidate` trên `<form>`, `preventDefault`), hiển thị lỗi ngay dưới trường sai bằng chữ đỏ; không dùng `alert()`
- Thông báo lỗi bằng tiếng Anh, ngắn gọn, nói rõ cách sửa
- Trường mật khẩu luôn có nút hiện/ẩn

### 3.5. Liên kết chuyển đổi giữa các trang liên quan

- Mỗi trang phải có đường đi rõ ràng sang trang liên quan, không để người dùng ở ngõ cụt
- Trang đăng nhập → trang đăng ký; trang đăng ký → "Back to login"; các trang phụ (quên mật khẩu...) → "Back to login"
- Dùng đường dẫn tương đối (ví dụ `href="register.html"`); chỉ dùng `href="#"` cho trang chưa tồn tại
- Liên kết dùng màu `primary`, có hiệu ứng `hover:underline`

### 3.6. Quy ước mã nguồn

- Mỗi trang là một file HTML độc lập, tên chữ thường (`login.html`, `register.html`)
- Dùng chung cấu hình Tailwind, `<style>` nền dark và khối script như hai trang hiện có; sao chép phần `<head>` từ `login.html` làm mẫu
- Mọi chuỗi giao diện bằng tiếng Anh; nhận xét trong mã cũng nên bằng tiếng Anh
- Nút CTA chính: `bg-primary`, chữ trắng, `font-heading font-bold`, có hiệu ứng hover/active và `focus-visible` ring
- Điểm kết nối backend ghi chú bằng `// TODO: connect to your ... API here`

---

## 4. Checklist cho trang mới

- [ ] Có cấu hình màu `bg / primary / surface / surface2 / line` và hai font Poppins + Inter
- [ ] Dark theme toàn bộ, không có thành phần nền sáng
- [ ] Hiển thị tốt trên điện thoại, tablet, PC
- [ ] Mọi ô nhập có icon Lucide, nhãn và `autocomplete` phù hợp
- [ ] Trường bắt buộc có `*`; có kiểm tra và thông báo lỗi tiếng Anh
- [ ] Có liên kết chuyển đến các trang liên quan
- [ ] Toàn bộ nội dung bằng tiếng Anh
