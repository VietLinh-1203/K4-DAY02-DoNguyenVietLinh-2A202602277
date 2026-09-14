# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Đỗ Nguyễn Việt Linh<br>
**MSSV:** 2A202602277<br>
**Hình thức:** cá nhân — cá nhân hoặc theo cặp<br>
**Mã cặp:** SOLO — ghi `SOLO` nếu làm cá nhân

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

- Ảnh và mã vật thể: ảnh drive_038.jpg và mã BUS 104
- Dấu hiệu nhìn thấy: thân xe khách dài, nhiều cửa sổ hoặc hàng ghế
- Quy tắc áp dụng: gán label "bus" cho xe dạng này, không gán label "van"
- Quyết định: Giữ label "bus" cho vật thể 104
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Phóng to vùng xe để kiểm tra số lượng cửa sổ, chiều dài thân xe và đặc điểm khoang hành khách, so sánh với các thuộc tính của xe van để chốt

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: ảnh drive_038.jpg và mã TRUCK 72
- Dấu hiệu nhìn thấy: xe có thiết bị công vụ rõ ràng, không kín một khối như xe van/ô tô con
- Quy tắc áp dụng: gán label "truck" cho xe dạng này, không gán label "van"/"car"
- Quyết định: Giữ label "truck" cho vật thể 72
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Phóng to và kiểm tra kĩ hình dạng hay cấu trúc xe, nếu có những đặc điểm mô tả phù hợp thì quyết định chọn loại xe

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: ảnh drive_008.jpg và mã VAN 17
- Dấu hiệu nhìn thấy khi phóng 100%: Chỉ nhìn thấy 1 góc đuôi xe, nếu nhìn theo góc nhìn trong ảnh thì là hình hộp, bị mép ảnh cắt
- Giá trị `visibility`: occluded
- Giá trị `boundary`: truncated
- Trạng thái `review_state`: need_review
- Lý do: trong ảnh vật thể chỉ hiện 1 góc đuôi xe, có dấu hiệu đặc điểm giống với miêu tả của xe van nhưng chưa thể xác thực rõ ràng, cần phải review lại

## 6. Xác nhận tự kiểm tra

- [ ] Đã rà đủ bốn ảnh.
- [ ] Đã kiểm vật thể thiếu và trùng.
- [ ] Đã kiểm lớp và hình học từng hộp.
- [ ] Mỗi hộp có đủ ba thuộc tính.
- [ ] Đã xử lý mọi hộp `needs_review`.
- [ ] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [ ] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [ ] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
- [ ] Số vật thể thực tế: CHƯA ĐIỀN — 40–60 là mục tiêu khối lượng, không phải điểm cắt.
