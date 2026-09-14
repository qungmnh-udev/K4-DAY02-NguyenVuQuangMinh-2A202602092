# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** `Nguyen-Vu-Quang-Minh`<br>
**MSSV:** `2A202602092`<br>
**Hình thức:** `cá nhân`<br>
**Mã cặp:** `SOLO`

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: `f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33`
- Bốn mã ảnh: `drive_008, drive_022, drive_033, drive_038`
- Số vật thể thực tế: `82`
- Mã SHA-256 của gói YOLO của bạn: `ee3d98fba5009218a765c20f54afa2aca561a7d2c26104747dff938b739a706c`
- Mã SHA-256 của gói CVAT gốc của bạn: `3aaa35ffc347c2c63a15aeeede59ebf5644aa70d40726829ba1f2c0b342b0029`
- Nguồn đối chiếu: bạn cùng cặp hoặc bộ tham chiếu do người hướng dẫn thực hành cấp: `Bộ tham chiếu người hướng dẫn cấp`
- Mã SHA-256 của gói đối chiếu: `c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b`
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: `01 — 11:14`

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu: `Vì không tìm được cặp nên quyết định làm bài độc lập`

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| `CAR 78` | `car` | `Có cửa trước và cửa sau` | `So sánh đặc điểm nhận dạng xe con, xe tải, xe buýt và xe chở hàng` |
| `VAN 64` | `van` | `Có cửa trước, không có cửa sau` | `So sánh đặc điểm nhận dạng xe con, xe tải, xe buýt và xe chở hàng` |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:
`Có thể thấy như ví dụ trên, 2 vật thể có kích cỡ ngang nhau, cả 2 xe đều có thể có cùng màu, cùng số lượng bánh xe, cửa sổ, đó là các thuộc tính (attribute), nhưng 2 xe đó vẫn có thể thuộc 2 lớp (class) khác nhau.`

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |

- Số hộp `needs_review` trước và sau khi kiểm: `2 - 2`
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: `BUS 61 thiếu dữ kiện do không rõ về mặt hình ảnh, chỉ có thể đánh giá dựa trên kích cỡ của vật thể so với phương tiện cùng làn, không có dữ kiện rõ ràng`

## 4. Một dòng nhãn YOLO
- Dòng `lớp=0 (car) | tâm=(0.2650, 0.5058) | kích thước=(0.1014, 0.0715)`:
- Tên lớp và tọa độ điểm ảnh `xyxy`: `car xyxy: [137.1, 300.8, 202.0, 346.6]`
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?
`Đúng định dạng chưa giải quyết được vấn đề có gán đúng lớp hay phạm vi, hình học vì đúng định dạng chỉ phản ánh dữ liệu trả về là đúng với định dạng mà thuật toán yêu cầu, nhưng hoàn toàn có thể sai về mặt đánh giá theo class.`

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: `drive_022, drive_033, drive_038`
- Mã ảnh thẩm định: `drive_008`
- Mô tả một dự đoán trong `detect_result.jpg`:
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
- Minh chứng nào có thể bác bỏ nhận định của bạn?
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?

## 6. Đối chiếu nhãn

- Số hộp ghép được: `48`
- IoU trung bình và trung vị: `mean: 0.87159`, `median: 0.896724`
- Mức đồng thuận lớp: `0.729167`
- Số hộp phía bạn không ghép được: `2`
- Số hộp phía đối chiếu không ghép được: `34`
- Một điểm khác biệt cụ thể: `Tại ảnh drive_033, có 2 nhãn mô hình gán sai, gán trực tiếp 2 mã vào BUS 1 mặc dù ở đó không có gì để đánh giá.`
- Quy tắc hoặc hành động sửa phát sinh: `Loại bỏ 2 nhãn khác biệt khỏi BUS 1`
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng? `Mức đồng thuận cao chỉ nói lên là mô hình đang làm theo đúng như những gì được chỉ bảo, giả dụ nếu như input không được sạch, lỗi vặt nhiều hoặc không rõ ràng, thì mô hình cũng sẽ học theo và sẽ bám sát nhưng chỉ tới như input.`

## 7. Kiểm tra kho GitHub cá nhân

- [ ] Có phiếu quy tắc với ba tình huống mơ hồ.
- [ ] Có kết quả kiểm hai gói xuất.
- [ ] Có thông tin lần huấn luyện và ảnh dự đoán.
- [ ] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [ ] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [ ] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:
