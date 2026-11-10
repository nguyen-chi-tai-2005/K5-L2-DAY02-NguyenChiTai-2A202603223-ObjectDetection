# Mini guideline — Nguyễn Chí Tài — 2A202603223

> Bản nháp hỗ trợ bởi Codex, tổng hợp từ schema và GUIDE.md của bài lab. Học viên cần đọc, tự kiểm tra khi gán nhãn và hoàn thiện ít nhất ba ca mơ hồ thực tế trước khi nộp. Ba ca dưới đây do học viên cung cấp và xác nhận đã được sửa trong bản xuất CVAT đang dùng. Lý do dưới đây giải thích theo schema; học viên cần đối chiếu đặc điểm xe thực tế trước khi nộp.

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
| CASE-01 / `drive_014.jpg` | ID411 | Đã điều chỉnh phân lớp trong quá trình annotation. | Đổi `car` → `van`; học viên xác nhận bản xuất hiện tại đã chứa thay đổi. | Theo schema, xe van/minivan chở hàng hoặc chở người thuộc `van`, phân biệt với xe con thông thường thuộc `car`. Học viên xác định lại loại xe là van; không xác minh vị trí ID từ YOLO; xem các ca bổ sung có dấu hiệu trực quan bên dưới. |
| CASE-02 / `drive_017.jpg` | ID467 | Đã điều chỉnh phân lớp trong quá trình annotation. | Đổi `van` → `car`; học viên xác nhận bản xuất hiện tại đã chứa thay đổi. | Theo schema, xe con, sedan, SUV, taxi và bán tải thuộc `car`; không xếp vào `van` chỉ vì thân xe lớn. Học viên xác định lại loại xe là car; không xác minh vị trí ID từ YOLO; xem các ca bổ sung có dấu hiệu trực quan bên dưới. |
| CASE-03 / `drive_039.jpg` | ID936 | Đã điều chỉnh phân lớp trong quá trình annotation. | Đổi `truck` → `bus`; học viên xác nhận bản xuất hiện tại đã chứa thay đổi. | Theo schema, xe buýt/minibus chở khách thuộc `bus`, còn xe tải hàng/đầu kéo thuộc `truck`. Học viên xác định lại loại xe là bus; không xác minh vị trí ID từ YOLO; xem các ca bổ sung có dấu hiệu trực quan bên dưới. |

Lưu ý đồng bộ: học viên xác nhận ba thay đổi đã có trong bản xuất CVAT được nhập vào repo tại commit `b7aad2f`; không sửa nhãn lần nữa từ ba ID này. Đây là xác nhận của học viên, không phải kết quả đối chiếu ID tự động: file YOLO 5 cột không lưu ID đối tượng CVAT.

## Ba ca mơ hồ bổ sung đã đối chiếu trực quan

Các quan sát dưới đây được Codex đối chiếu từ ảnh và overlay nhãn theo yêu cầu học viên; không gán lại chúng cho ID CVAT cũ và không coi là ghi chép trước khi nhận gold.

- `drive_008.jpg`: hai bus lớn bên phải chồng hình, xe phía sau bị che một phần. Dấu hiệu còn thấy gồm thân xe dài, dãy cửa sổ và phần đầu/đuôi riêng của hai xe. Quyết định: phân biệt hai vật thể, lớp bus, không gộp thành một box; box theo phần nhìn thấy của từng xe. Cần kiểm lại các cạnh khi một xe che xe còn lại.
- `drive_030.jpg`: xe màu sáng bên phải và xe tối màu gần góc phải có thân cao, dài, nóc liên tục và không thấy thùng hàng hở; có thể gây phân vân car/van khi nhìn từ góc cao. Quyết định trong nhãn cuối là van; cơ sở là thân xe dạng van/minivan theo schema, không phải chỉ vì kích thước lớn. Xe xa không đủ chi tiết không được tự đoán lớp.
- `drive_034.jpg`: xe trắng giữa phần dưới ảnh có cabin riêng và thiết bị nâng/cẩu trên khung phía sau, khác thân liền của van và xe bán tải thông thường. Quyết định nhãn cuối là truck; cơ sở là cấu trúc xe tải chuyên dụng/cabin và khung sau. Đây là điểm schema mô tả chưa chi tiết bằng xe tải hàng, nên ghi rõ trường hợp chuyên dụng để giảng viên đối chiếu; không suy thành bus chỉ vì xe lớn.

## Cách ghi nhận trong lúc làm CVAT

1. Giữ nguyên tên ảnh, ghi chính xác tên ảnh đang mở (ví dụ dạng `drive_XXX.jpg`, không chỉ số thứ tự frame).
2. Đặt mã CASE-01, CASE-02... cho ghi chép. Mã ca chỉ dùng trong tài liệu, không tạo thêm lớp annotation.
3. Ghi vị trí dễ tìm: trái/phải, gần/xa, làn nào, cạnh vật nào. Nếu đã có box, ghi ID box đang hiển thị để hỗ trợ tìm lại; tên ảnh và vị trí vẫn là thông tin chính khi ID thay đổi sau xuất/nhập.
4. Chụp ảnh màn hình toàn cảnh và, nếu cần, vùng phóng to; có thể lưu vào `report/individual/assets/ambiguous_cases/` với tên `CASE-01_drive_XXX.png`. Đây là minh chứng tùy chọn, không thay ảnh gốc.
5. Ghi hai khả năng đang phân vân, dấu hiệu nhìn thấy và điều chưa rõ. Hỏi giảng viên bằng mô tả đó, không tạo nhãn `unknown`, `ambiguous` hoặc ID lớp thứ năm.
6. Khi được làm rõ, cập nhật lớp/box trên CVAT và quyết định cuối trong bảng. Áp dụng nhất quán cho các ca tương tự rồi xuất lại nhãn và chạy checker.

Ví dụ cách diễn đạt (chỉ minh họa, không tính là ca thực tế): “CASE-xx — drive_XXX.jpg — xe trắng bên phải, bị xe phía trước che phần đầu; đang phân vân van/bus do chỉ thấy thân xe. Chờ giảng viên xác nhận lớp và phần nhìn thấy cần bao box.”

Nguồn: README.md (schema), GUIDE.md (quy tắc box), RULES.md (ca mơ hồ và sử dụng AI). Không dùng gold train để viết quyết định trước mốc phát.
