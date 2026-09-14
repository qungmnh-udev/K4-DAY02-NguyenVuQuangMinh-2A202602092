# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Nguyen-Vu-Quang-Minh<br>
**MSSV:** 2A202602092<br>
**Hình thức:** Cá nhân<br>
**Mã cặp:** `SOLO`

## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| ---: | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

Hoàn thành trước khi xem bài của người khác hoặc bộ nhãn tham chiếu.

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: `drive_038`,`BUS 61`
- Dấu hiệu nhìn thấy: `Độ cao của xe khác so với các xe cùng làn`
- Quy tắc áp dụng: `Đánh giá dựa trên việc đối chiếu với các xe cùng làn`
- Quyết định: `Đánh giá là xe buýt dựa trên độ cao và chiều dài của xe`
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? `Giữ nguyên class, chuyển review_state thành need_review`

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: `drive_038`,`VAN 64`
- Dấu hiệu nhìn thấy: `Đuôi xe dài`
- Quy tắc áp dụng: `So sánh trực tiếp với CAR 78`
- Quyết định: `Cần xác định thêm`
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? `Giữ nguyên class, chuyển review_state thành need_review`

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: `drive_038`,`CAR 75`
- Dấu hiệu nhìn thấy khi phóng 100%: `2 ghế sau, bánh nhỏ, có cửa sau, không có cốp to, thùng hàng`
- Giá trị `visibility`: `Clear`
- Giá trị `boundary`: `Truncated`
- Trạng thái `review_state`: `Confident`
- Lý do: `Có thể xác định được không phải là truck do không có bồn, không phải van do không có thùng hàng kín, không phải bus do kích thước chênh lệch`

## 6. Xác nhận tự kiểm tra

- [ ] Đã rà đủ bốn ảnh.
- [ ] Đã kiểm vật thể thiếu và trùng.
- [ ] Đã kiểm lớp và hình học từng hộp.
- [ ] Mỗi hộp có đủ ba thuộc tính.
- [ ] Đã xử lý mọi hộp `needs_review`.
- [ ] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [ ] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [ ] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
