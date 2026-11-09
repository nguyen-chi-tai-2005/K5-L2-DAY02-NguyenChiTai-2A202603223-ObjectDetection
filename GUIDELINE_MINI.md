# Mini guideline — Nguyễn Chí Tài — 2A202603223

> Bản nháp hỗ trợ bởi Codex, tổng hợp từ schema và GUIDE.md của bài lab. Học viên cần đọc, tự kiểm tra khi gán nhãn và hoàn thiện ít nhất ba ca mơ hồ thực tế trước khi nộp. Ba ca dưới đây do học viên cung cấp; lý do quan sát và bản xuất nhãn sau chỉnh sửa còn chờ bổ sung.

## Định nghĩa lớp

| ID / lớp | Định nghĩa theo đề bài | Điểm phân biệt |
| --- | --- | --- |
| `0 car` | Xe con, sedan, SUV, taxi và xe bán tải. | Xe bán tải thuộc car theo schema của lab; không chuyển sang truck chỉ vì có thùng chở hàng. |
| `1 truck` | Xe tải chở hàng và xe đầu kéo. | Phân biệt với bán tải thuộc car; trường hợp hình dáng/cabin khó xác định cần ghi lại và hỏi giảng viên. |
| `2 bus` | Xe buýt và minibus chở khách. | Không suy ra bus chỉ vì xe lớn hoặc chở người; van/minivan thuộc van. Nếu chưa rõ minibus hay van, ghi ca mơ hồ. |
| `3 van` | Xe van, minivan chở hàng hoặc chở người. | Phân biệt với xe con thông thường và bus theo hình dáng quan sát được; không đoán loại xe khi bị che quá nhiều. |

## Quy tắc chung

- Mỗi xe hợp lệ có một bounding box riêng. Không gộp hai xe dù chúng chồng lên nhau.
- Box bám sát phần thân xe nhìn thấy: đặt các cạnh sát giới hạn phần nhìn thấy, tránh lấy thêm đường, trời hoặc xe bên cạnh. Không suy đoán kích thước đầy đủ của phần bị khuất.
- Vật thể bị che khuất: vẫn gán nếu phần nhìn thấy đủ để nhận diện lớp; box bao phần nhìn thấy của chính xe đó, không tính xe/vật cản phía trước là một phần xe.
- Vật thể bị cắt bởi rìa ảnh: box dừng đúng rìa ảnh; không kéo ra ngoài hoặc đoán phần ngoài khung.
- Vật thể rất nhỏ/mơ hồ: không tự đặt ngưỡng kích thước mới. Nếu không nhận diện chắc lớp, ghi ảnh, vị trí, dấu hiệu quan sát được và hỏi giảng viên; không tự đoán.
- Xe đỗ vẫn gán theo loại xe. Trạng thái đỗ/di chuyển không tạo lớp mới.
- Không gán người, xe máy, xe đạp, biển báo, đèn giao thông, ảnh xe trên quảng cáo hoặc phản chiếu không phải xe thật trong cảnh.
- Chỉ gán 50 ảnh train, giữ nguyên tên ảnh và ID lớp. Xuất YOLO detection với 5 cột, tọa độ chuẩn hóa. Ảnh đã kiểm tra không có xe hợp lệ vẫn có file nhãn rỗng.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

Điền bằng ảnh thực tế đang gán. Mỗi ca cần chỉ rõ xe nào; nếu chưa có kết luận, ghi “Chờ giảng viên xác nhận”, sau đó cập nhật quyết định cuối và lý do trước khi nộp.

| Mã ca / ảnh | Vị trí xe hoặc ID box trong CVAT | Tình huống và dấu hiệu nhìn thấy | Quyết định cuối | Lý do theo schema / xác nhận |
| --- | --- | --- | --- | --- |
| CASE-01 / `drive_014.jpg` | ID411 | Nhầm lớp `car` / `van`; học viên chưa mô tả dấu hiệu hình ảnh. | Đổi `car` → `van` theo thông tin học viên cung cấp. | Chờ học viên bổ sung dấu hiệu quan sát và lý do theo schema; chưa ghi nhận xác nhận của giảng viên. |
| CASE-02 / `drive_017.jpg` | ID467 | Nhầm lớp `van` / `car`; học viên chưa mô tả dấu hiệu hình ảnh. | Đổi `van` → `car` theo thông tin học viên cung cấp. | Chờ học viên bổ sung dấu hiệu quan sát và lý do theo schema; chưa ghi nhận xác nhận của giảng viên. |
| CASE-03 / `drive_039.jpg` | ID936 | Nhầm lớp `truck` / `bus`; học viên chưa mô tả dấu hiệu hình ảnh. | Đổi `truck` → `bus` theo thông tin học viên cung cấp. | Chờ học viên bổ sung dấu hiệu quan sát và lý do theo schema; chưa ghi nhận xác nhận của giảng viên. |

Lưu ý đồng bộ: quyết định đã được ghi nhận trong tài liệu; cần xuất lại nhãn CVAT sau chỉnh sửa để cập nhật các file YOLO. ID đối tượng CVAT không được lưu trong file YOLO 5 cột, nên không dùng ID làm số dòng để sửa nhãn.

## Cách ghi nhận trong lúc làm CVAT

1. Giữ nguyên tên ảnh, ghi chính xác tên ảnh đang mở (ví dụ dạng `drive_XXX.jpg`, không chỉ số thứ tự frame).
2. Đặt mã CASE-01, CASE-02... cho ghi chép. Mã ca chỉ dùng trong tài liệu, không tạo thêm lớp annotation.
3. Ghi vị trí dễ tìm: trái/phải, gần/xa, làn nào, cạnh vật nào. Nếu đã có box, ghi ID box đang hiển thị để hỗ trợ tìm lại; tên ảnh và vị trí vẫn là thông tin chính khi ID thay đổi sau xuất/nhập.
4. Chụp ảnh màn hình toàn cảnh và, nếu cần, vùng phóng to; có thể lưu vào `report/individual/assets/ambiguous_cases/` với tên `CASE-01_drive_XXX.png`. Đây là minh chứng tùy chọn, không thay ảnh gốc.
5. Ghi hai khả năng đang phân vân, dấu hiệu nhìn thấy và điều chưa rõ. Hỏi giảng viên bằng mô tả đó, không tạo nhãn `unknown`, `ambiguous` hoặc ID lớp thứ năm.
6. Khi được làm rõ, cập nhật lớp/box trên CVAT và quyết định cuối trong bảng. Áp dụng nhất quán cho các ca tương tự rồi xuất lại nhãn và chạy checker.

Ví dụ cách diễn đạt (chỉ minh họa, không tính là ca thực tế): “CASE-xx — drive_XXX.jpg — xe trắng bên phải, bị xe phía trước che phần đầu; đang phân vân van/bus do chỉ thấy thân xe. Chờ giảng viên xác nhận lớp và phần nhìn thấy cần bao box.”

Nguồn: README.md (schema), GUIDE.md (quy tắc box), RULES.md (ca mơ hồ và sử dụng AI). Không dùng gold train để viết quyết định trước mốc phát.
