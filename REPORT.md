# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Đỗ Nguyễn Việt Linh<br>
**MSSV:** 2A202602277<br>
**Hình thức:** cá nhân — cá nhân hoặc theo cặp<br>
**Mã cặp:** SOLO — ghi `SOLO` nếu làm cá nhân

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33
- Bốn mã ảnh: drive_008.jpg, drive_022.jpg, drive_033.jpg, drive_038.jpg
- Số vật thể thực tế: 125
- Mã SHA-256 của gói YOLO của bạn: e331b218a679314ec19f0bba082b2b416e35cff2358ee63480e3b3f7a2e1cf39
- Mã SHA-256 của gói CVAT gốc của bạn: 280c09572c3cc03cd1937a1890602be69acb1641d55c81e21b7b47985201a1f2
- Nguồn đối chiếu: bạn cùng cặp hoặc bộ tham chiếu do người hướng dẫn thực hành cấp: bộ tham chiếu do người hướng dẫn thực hành cấp
- Mã SHA-256 của gói đối chiếu: c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: 15:53

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu: Bài vẫn độc lập vì bản thân tự quan sát hình ảnh, xác định dấu hiệu của xe và áp dụng quy tắc gán nhãn phù hợp. Quyết định dựa trên bằng chứng nhìn thấy trong ảnh, không dựa vào đáp án có sẵn.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| xe bus màu vàng xanh bên phải | bus | Thân dài, cao, nhiều cửa sổ hành khách | Xe chở nhiều hành khách, thân xe dài → gán bus |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

Ví dụ với vật thể 104:
- Lớp: bus — cho biết đó là loại phương tiện nào.
- Thuộc tính: visibility: clear — cho biết mức độ nhìn thấy của xe.
"bus" không phải thuộc tính, đó là nhãn phân loại.

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| Xe buýt vàng–xanh có hộp chưa ôm sát toàn bộ thân xe| phạm vi/hình học | So sánh hộp với mép thân xe trong ảnh gốc | Chỉnh hộp bao toàn bộ phần xe nhìn thấy, không lấy phần đường hoặc xe bên cạnh |

- Số hộp `needs_review` trước và sau khi kiểm: trước là 14 và sau là 0
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: ở ảnh drive_038.jpg mã CAR 102 vật thể khá nhỏ và chưa thể xác định, sau khi nhờ hỗ trợ từ lab coach, đi đến quyết định chung là chọn label "car" dựa trên hình dáng của vật thể

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: row: [0, 0.520289, 0.526766, 0.121359, 0.072813]
- Tên lớp và tọa độ điểm ảnh `xyxy`: lớp=0 (car) | tâm=(0.5203, 0.5268) | kích thước=(0.1214, 0.0728)
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?

 Đúng định dạng chỉ chứng minh cú pháp hợp lệ, nhãn vẫn có thể sai nếu nhầm lớp, hộp lệch/không bao đủ xe, hoặc dùng sai tọa độ.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: drive_022, drive_033, drive_038
- Mã ảnh thẩm định: drive_008
- Mô tả một dự đoán trong `detect_result.jpg`: Xe buýt vàng xanh lớn bên phải được khoanh hộp màu cam và gán lớp bus, hộp bao phủ gần như toàn bộ xe nên đây là dự đoán hợp lý
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào? quy tắc vẽ hộp có cần ôm sát toàn bộ thân xe hay không, đặc biệt với xe bus bị che một phần.
- Minh chứng nào có thể bác bỏ nhận định của bạn? nếu nhãn gốc cho xe này là lớp khác, hoặc hộp đúng theo quy ước phải chỉ bao phần nhìn thấy nhưng hộp hiện tại bao cả vùng không thuộc xe.
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế? chỉ có 4 ảnh, trong đó 3 ảnh huấn luyện và 1 ảnh thẩm định, dữ liệu quá nhỏ, không đại diện, và mục đích được ghi rõ là tìm lỗi dữ liệu

## 6. Đối chiếu nhãn

- Số hộp ghép được: 48
- IoU trung bình và trung vị: 0.855955 và 0.882635
- Mức đồng thuận lớp: 0.729167
- Số hộp phía bạn không ghép được: 77
- Số hộp phía đối chiếu không ghép được: 2
- Một điểm khác biệt cụ thể: Có 13/48 cặp hộp đã ghép hình học nhưng khác lớp
- Quy tắc hoặc hành động sửa phát sinh: Rà lại từng cặp khác lớp và 77 hộp không ghép, xác định là nhầm lớp, vẽ trùng, sai phạm vi hay đối tượng bị bỏ sót trước khi sửa.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng? Mức đồng thuận cao chỉ cho thấy các hộp đã ghép có vị trí gần nhau, vẫn còn khác lớp, 77 hộp không ghép và có thể cùng một lỗi quy tắc trong cả hai bộ nhãn

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach: Không có

