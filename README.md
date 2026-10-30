# Học viện An Tâm Số – Đào tạo CBNV MISA

Website đào tạo Bảo hiểm An Tâm Số (BH PVI × MISA): 11 bài học có công cụ tương tác, bài thi 30 câu rút ngẫu nhiên từ ngân hàng 67 câu, giấy xác nhận hoàn thành.

**Bài thi được chấm trên Google Apps Script**: website chỉ chứa câu hỏi, không chứa đáp án. Đáp án, lời giải, việc chấm điểm và ghi kết quả nằm trong file `Code.gs` – file này **không đưa lên repo** (repo public), chỉ lưu nội bộ tại BH PVI.

Toàn bộ website nằm trong **một file `index.html`** (không cần build, không cần server).

## 1. Đưa lên GitHub Pages

1. Tải `index.html` và `README.md` lên nhánh `main` của repo này (**không** tải `Code.gs`).
2. Vào **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main` / `/ (root)` → **Save**.
3. Sau 1–2 phút, website chạy tại:
   `https://pvi-aviationdivision.github.io/Daotao_Misa_AnTamSo/`

> Repo phải ở chế độ **Public** để dùng GitHub Pages miễn phí (gói Free). Nội dung website là tài liệu đào tạo nội bộ, không chứa số liệu thương mại nhạy cảm (phí dịch vụ, hoa hồng, tỷ lệ tái).

## 2. Kết nối Google Sheet (hệ thống chấm bài)

1. Tạo một Google Sheet mới, ví dụ *"Kết quả đào tạo An Tâm Số – MISA"*.
2. Trong Sheet: **Tiện ích mở rộng → Apps Script**. Xóa code mẫu, dán toàn bộ nội dung file `Code.gs` (bản lưu nội bộ), bấm **Lưu**. Chọn hàm `setup` → **Chạy** một lần để cấp quyền.
3. Bấm **Triển khai → Tùy chọn triển khai mới** → biểu tượng bánh răng → chọn **Ứng dụng web**:
   - Thực thi dưới dạng: **Tôi**
   - Người có quyền truy cập: **Bất kỳ ai**
   - Bấm **Triển khai**, cấp quyền khi Google hỏi.
4. Sao chép **URL ứng dụng web** (dạng `https://script.google.com/macros/s/.../exec`).
5. Mở `index.html`, tìm dòng:
   ```js
   const SHEET_ENDPOINT = "";
   ```
   dán URL vào giữa hai dấu ngoặc kép, lưu và tải lại lên GitHub.
6. Làm thử một bài thi → Sheet ghi một dòng vào tab **KetQuaThi**.

Khi chưa kết nối, các bài học vẫn dùng bình thường nhưng nút làm bài thi bị khóa.

Mỗi lần nộp bài, Apps Script chấm điểm và ghi: thời gian, họ tên, email, phòng ban, khu vực, điểm, % đúng, Đạt/Chưa đạt, thời gian làm bài, lần thi thứ mấy, mã xác nhận, điểm theo 5 nhóm kiến thức, ID các câu sai (để biết câu nào nhiều người sai), số bài đã học.

> Nếu sửa code Apps Script sau này: **Triển khai → Quản lý triển khai → Chỉnh sửa → Phiên bản: Mới** để URL giữ nguyên.

## 3. Cấu hình bài thi

Trong `index.html`, tìm `const EXAM =`:

| Tham số | Hiện tại | Ý nghĩa |
|---|---|---|
| `total` | 30 | Số câu mỗi đề |
| `pass` | 0.8 | Tỷ lệ đạt (80% = 24/30 câu) |
| `minutes` | 30 | Thời gian làm bài |
| `quota` | P 8 · X 6 · E 4 · C 6 · S 6 | Số câu rút từ mỗi nhóm (tổng phải bằng `total`) |

Nhóm câu hỏi: **P** Sản phẩm & quyền lợi (18 câu) · **X** Phạm vi & loại trừ (10) · **E** Điều kiện tham gia (8) · **C** Quy trình & bồi thường (14) · **S** Tư vấn & tuân thủ (17). Sửa ngân hàng đề: liên hệ Phòng TMĐT – BH PVI (câu hỏi trong `index.html` và đáp án trong `Code.gs` phải cập nhật cùng nhau theo ID câu).

## 4. Nguồn nội dung

- Quy tắc bảo hiểm An ninh mạng SME (PVI CYBERSHIELD SME) – QĐ 888/QĐ-PVIBH ngày 20/08/2026
- Sổ tay đào tạo MISA An Tâm Số v1.1 (10/2026), Bảng báo phí Cyber Shield SME BH PVI × MISA

Khi có khác biệt, Quy tắc bảo hiểm và Giấy chứng nhận bảo hiểm được ưu tiên áp dụng.

## Lưu ý vận hành

- Tiến độ học lưu trên trình duyệt của từng học viên (localStorage); đổi máy/trình duyệt sẽ học lại từ đầu, nhưng kết quả thi đã gửi về Sheet không mất.
- Mã xác nhận trên giấy xác nhận do Apps Script ký bằng khóa bí mật; đối chiếu với cột *Mã xác nhận* trong Sheet để kiểm tra.
