# HƯỚNG DẪN KẾT NỐI GOOGLE SHEET — Đăng ký / Đăng nhập

Làm 1 lần, mất khoảng **5 phút**. Sau này dữ liệu thành viên sẽ tự lưu vào file
Google Sheet (Excel online) của bạn.

## Bước 1 — Tạo Google Sheet mới
1. Vào **sheets.google.com** → nhấp **Trang tính trống** (Blank).
2. Đặt tên file, ví dụ: **ThanhVien Website**.

## Bước 2 — Mở phần viết mã Apps Script
1. Trên trang Google Sheet, vào menu **Tiện ích mở rộng (Extensions)**
   → **Apps Script**.
2. Xóa hết nội dung trong file `Code.gs` đang mở.

## Bước 3 — Dán mã
1. Mở file **GOOGLE_APPS_SCRIPT.gs** trong thư mục website (mở bằng Notepad).
2. Copy TOÀN BỘ nội dung, dán vào `Code.gs` trên mạng.
3. Nhấn biểu tượng **lưu** (hình đĩa mềm).

## Bước 4 — Triển khai (Deploy)
1. Nhấp nút **Triển khai (Deploy)** → **Triển khai mới (New deployment)**.
2. Nhấp biểu tượng **bánh răng** → chọn **Ứng dụng web (Web app)**.
3. Cài đặt như sau:
   - **Mô tả**: điền gì cũng được (vd: `web thanh vien`)
   - **Thực thi với tư cách (Execute as)**: `Tôi (email của bạn)`
   - **Ai có quyền truy cập (Who has access)**: `Bất kỳ ai (Anyone)` ← QUAN TRỌNG
4. Nhấp **Triển khai (Deploy)**.
5. Nếu hiện cửa sổ xin quyền → nhấp **Uỷ quyền truy cập** → chọn tài khoản
   → **Nâng cao** → **Cho phép truy cập**. Script cần quyền ghi Sheet và gửi
  email qua Gmail. Nếu được hỏi lại quyền sau khi thêm alias, hãy chấp thuận.

> Để dùng các Gmail gửi dự phòng: trong Gmail đang chạy Apps Script, vào
> **Cài đặt → Tài khoản và nhập → Gửi thư bằng địa chỉ này → Thêm địa chỉ email khác**.
> Thêm từng địa chỉ Gmail và xác minh. Nếu Gmail yêu cầu cấu hình SMTP, nhập máy chủ,
> tên đăng nhập và key ứng dụng của chính Gmail đó ngay trong màn hình cài đặt Gmail.
> Sau khi địa chỉ xuất hiện trong danh sách **Gửi thư bằng địa chỉ này**, script chỉ
> thử các địa chỉ phụ đã xác minh, không dùng Gmail chính. Không nhập key vào `.gs` hay website.

## Bước 5 — Cấu hình URL server-side
1. Sau khi triển khai xong sẽ hiện **URL ứng dụng web** dạng:
   `https://script.google.com/macros/s/AKfycb.../exec`
2. Nhấn nút **Sao chép (Copy)**.
3. Trên Vercel, mở project → **Settings → Environment Variables**.
4. Tạo biến `APPS_SCRIPT_URL`, dán URL vừa sao chép và chọn môi trường
  **Production** (và **Preview** nếu dùng preview deployments). Giữ URL ở server;
  không chép vào `auth.js`, HTML hay repository.
5. Tạo deployment mới để biến có hiệu lực. Khi chạy local bằng `vercel dev`, đăng nhập
  CLI rồi chạy `npx.cmd vercel env pull .env.local`. File `.env.local` đã được Git ignore.

## Bước 6 — Kiểm tra
0. **Kiểm tra nhanh kết nối**: mở thẳng URL Web App bằng trình duyệt
   (dán URL vào thanh địa chỉ) — nếu thấy dòng `{"ok":true,...}` là triển khai
   đã thành công. Nếu thấy báo lỗi "Script function not found" thì mã chưa
   được lưu hoặc chưa Deploy.
1. Mở website → trang **Đăng nhập** → tab **Đăng ký**.
2. Điền thông tin thử → **Tạo tài khoản**.
3. Mở Google Sheet lại: sẽ thấy sheet **ThanhVien** xuất hiện với
   thành viên vừa đăng ký (thời gian, họ tên, email, mật khẩu đã băm,
   số điện thoại).

## Nhập thành viên hàng loạt từ file Excel
Có sẵn file **`NHAP-THANH-VIEN-EXCEL.xlsx`** trong thư mục website để điền
danh sách thành viên rồi đưa lên Google Sheet (chi tiết cũng ghi trong
sheet "Hướng dẫn" của chính file đó):
1. Mở file Excel, điền vào sheet **ThanhVien**: Họ và tên, Email,
   Mật khẩu (gõ mật khẩu THÔ, chưa mã hóa), Số điện thoại. Xóa hàng ví dụ
   màu xám trước khi nhập. Hai cột "Thời gian" và "Email cảm ơn" để trống.
2. Trên Google Sheet: **Tệp → Nhập → Tải lên** → chọn file Excel →
   vị trí nhập chọn **Chèn hàng mới** (Insert new rows) → Nhập dữ liệu.
   Nếu hàng tiêu đề bị lặp dưới dữ liệu cũ thì xóa hàng lặp đi.
3. **Bắt buộc**: trên Google Sheet bấm menu **Công cụ → Mã hóa mật khẩu thô**.
   Cột mật khẩu sẽ biến thành chuỗi 64 ký tự — khi đó thành viên mới đăng
   nhập web được. (Chưa thấy menu "Công cụ"? Dán mã `GOOGLE_APPS_SCRIPT.gs`
   mới nhất vào Apps Script, lưu rồi làm mới trang Sheet.)
4. Thử đăng nhập web bằng một tài khoản vừa nhập để xác nhận.
- ⚠️ File Excel khi còn mật khẩu thô là file nhạy cảm: không gửi cho ai,
  không đăng lên mạng; chạy mã hóa xong có thể xóa file đi.

## Lưu ý
- **Mật khẩu không lưu thô**: website mã hóa (SHA-256) trước khi gửi,
  trong Sheet chỉ thấy chuỗi mã hóa — an toàn hơn cho thành viên.
- Khi đăng ký mới, website gửi email cảm ơn đến địa chỉ email đã đăng ký.
- Email chỉ gửi từ các alias đã xác minh, không gửi từ Gmail chính. Nếu một alias
  lỗi, script thử alias tiếp theo. Cột **"Email cảm ơn"** ghi địa chỉ gửi thành công.
- GmailApp cần được cấp quyền khi Deploy. Không lưu mật khẩu ứng dụng trong `.gs`
  hoặc website. Nếu Gmail yêu cầu SMTP khi thêm địa chỉ gửi, chỉ nhập key của tài khoản
  đó tại trang cài đặt Gmail chính thức; **không dán key vào file web hoặc gửi qua chat**.
  Nếu đã chia sẻ key này ở đâu đó, hãy vào `myaccount.google.com` → **Bảo mật** →
  **Mật khẩu ứng dụng** để xóa và tạo key mới.
- Nếu mọi địa chỉ gửi đều lỗi, tài khoản vẫn được tạo nhưng cột **"Email cảm ơn"**
  sẽ ghi "Lỗi"; kiểm tra alias đã xác minh, quyền Gmail và hạn mức gửi.
- **Thư cảm ơn dạng HTML** (giao diện tối màu giống website): muốn đổi nội dung,
  sửa hàm `buildThankYouHtml` trong `GOOGLE_APPS_SCRIPT.gs`. Mở file
  `xem-truoc-email/index.html` bằng trình duyệt để xem thử giao diện mà không cần gửi.
- **Nút "Khám phá website"** trong thư sẽ tự ẩn cho tới khi bạn điền link website
  thật vào biến `SITE_URL` ở đầu file `GOOGLE_APPS_SCRIPT.gs` sau khi
  đưa web lên Netlify/GitHub.
- Sai mật khẩu / trùng email sẽ được báo lỗi ngay trên trang web.
- **Chống dò mật khẩu**: mỗi email chỉ được thử sai tối đa 5 lần trong 15 phút
  (hết thời gian tự mở khóa). Quên mật khẩu? Chủ website xóa dòng trong Sheet
  rồi thành viên đăng ký lại là được.
- Muốn xem/sửa/xóa thành viên: mở trực tiếp file Google Sheet như Excel bình thường.
- Nếu sau này sửa mã trong `Code.gs`, nhớ **Deploy → Quản lý triển khai →
  chỉnh sửa → phiên bản mới** rồi Deploy lại, không thì web vẫn dùng bản cũ.

## Quyền riêng tư
File Sheet chỉ bạn (chủ tài khoản Google) nhìn thấy; thành viên khác không
xem được danh sách. Website gửi: họ tên, email, số điện thoại và mật khẩu đã băm.
