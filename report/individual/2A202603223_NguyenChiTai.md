# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Nguyễn Chí Tài
- MSSV: 2A202603223
- Email: 26ai.tainc@vinuni.edu.vn
- Lớp: Lv2
- Repo bài làm: https://github.com/nguyen-chi-tai-2005/K5-L2-DAY02-NguyenChiTai-2A202603223-ObjectDetection
- Colab đã chạy: https://colab.research.google.com/drive/1-C8ZVjWDiyvrd7c64MjiWT9wHhT84ZBh?usp=sharing

## 2. Schema và mini guideline

Áp dụng `car=0`, `truck=1`, `bus=2`, `van=3`; xe bán tải thuộc car. Quy tắc box và các ca mơ hồ nằm trong [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

Trong quá trình annotation, học viên ghi nhận ba thay đổi: `drive_014.jpg` / ID411 car → van; `drive_017.jpg` / ID467 van → car; `drive_039.jpg` / ID936 truck → bus. Học viên xác nhận chúng đã nằm trong bản xuất đầu tiên. Do YOLO không lưu ID CVAT, không suy vị trí xe từ số ID. Mini guideline bổ sung ba ca nhìn thấy rõ ở drive_008, drive_030, drive_034; phần đối chiếu này do Codex hỗ trợ và được ghi nguồn.

## 3. Annotation và self-QC

- Công cụ: CVAT, xuất Ultralytics YOLO Detection 1.0.
- Sản phẩm cuối: 50 file nhãn, 681 box (car 560, truck 46, bus 21, van 54).
- `python3 tools/check_yolo_labels.py`: đạt 50/50; basename khớp ảnh, 5 cột, ID lớp và hình học hợp lệ.
- Học viên xác nhận đã rà bằng mắt 10 ảnh: `drive_001.jpg`, `drive_003.jpg`, `drive_004.jpg`, `drive_005.jpg`, `drive_006.jpg`, `drive_008.jpg`, `drive_029.jpg`, `drive_030.jpg`, `drive_034.jpg`, `drive_048.jpg`.
- Codex đối chiếu bổ sung ảnh với overlay nhãn ban đầu/cuối: ảnh 001 có dãy xe đỗ bên trái và xe chạy bên phải; ảnh 003/005/006 là giao lộ nhiều xe gần/xa; ảnh 004 là giao lộ đêm; ảnh 008 có xe tải và hai bus chồng hình; ảnh 029/048 nhiều bus và xe nhỏ xen kẽ; ảnh 030 có đoàn xe nối đuôi; ảnh 034 có xe chuyên dụng màu trắng và xe con. Các vùng này được dùng kiểm tra phạm vi, lớp và tách box từng xe; checker không tự xác minh nội dung.
- Bản đầu được giữ ở commit `b7aad2f`, trước lần chạy gold QC; bản đạt ở `7c67ca1`. Lịch sử Git không thay thế xác nhận mốc khóa nhãn chính thức của lớp.

## 4. Gold QC và rework

Kết quả chạy script chính thức ở IoU matching 0.50:

| Chỉ số | Trước sửa | Rework 1 | Cuối |
|---|---:|---:|---:|
| TP | 488 | 449 | 638 |
| FP | 682 | 302 | 43 |
| FN | 165 | 204 | 15 |
| Precision | 0.4171 | 0.5979 | 0.9369 |
| Recall | 0.7473 | 0.6876 | 0.9770 |
| F1 | 0.5354 | 0.6396 | 0.9565 |
| mean_iou_of_matches | 0.8377 | 0.8404 | 0.9993 |
| passed | false | false | true |

JSON: [trước sửa](../../outputs/annotation_evaluation_before_rework.json), [rework 1](../../outputs/annotation_evaluation_rework1.json), [cuối](../../outputs/annotation_evaluation.json). Bản cuối đạt F1 >= 0.80 và mean IoU >= 0.70 trước train.

Sau kết quả chưa đạt, học viên sửa và xuất lại từ CVAT. Một lần xuất vẫn giống bản cũ từng byte; lần kế tiếp có 751 box nhưng F1 còn thấp. Bản cuối có 681 box và đạt gate. Không sửa gold, không dùng gold thay nguyên bộ nhãn học viên và không viết lại lịch sử Git.

Thay đổi số box ở 10 ảnh đã rà (đếm trực tiếp từ hai bản nhãn, không suy toàn bộ nguyên nhân chỉ từ FP/FN):

- `drive_001.jpg`: 16 → 5 box.
- `drive_003.jpg`: 45 → 26 box.
- `drive_004.jpg`: 13 → 5 box.
- `drive_005.jpg`: 40 → 24 box.
- `drive_006.jpg`: 44 → 21 box.
- `drive_008.jpg`: 25 → 19 box.
- `drive_029.jpg`: 27 → 18 box.
- `drive_030.jpg`: 53 → 20 box.
- `drive_034.jpg`: 17 → 10 box.
- `drive_048.jpg`: 17 → 16 box.

Đối chiếu overlay trước/sau cho thấy nhiều box nhỏ/xa hoặc ở vùng xe đỗ đã bị loại trong các ảnh 001, 003, 005, 006, 030; ở ảnh 008 có điều chỉnh box quanh các bus chồng hình. Đây là mô tả thay đổi quan sát được, không chứng minh mọi lần xóa đều đúng. Guideline yêu cầu gán xe đỗ và xe bị che nếu nhận diện được lớp; những khác biệt về phạm vi với gold cần được giảng viên làm rõ, không tự đặt thêm quy tắc “bỏ xe đỗ/xe xa”. Gold chỉ sử dụng cục bộ, không đưa lên repo/Colab.

## 5. Kết quả YOLO

- Runtime: Google Colab, Tesla T4; Ultralytics 8.4.208; Torch 2.14.1+cu130 (log phiên chạy).
- YOLO26n pretrained, epochs=50, imgsz=640, batch=-1, seed=20260909; giữ tham số train của notebook gốc.
- Train: 50 ảnh train; validation/evaluation: 10 ảnh eval. Không trộn eval vào train.
- Best checkpoint trong runtime: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`; không commit `.pt`.

| Chỉ số model trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.6779 |
| Recall | 0.5266 |
| mAP50 | 0.5960 |
| mAP50–95 | 0.4055 |

[Số liệu gốc](assets/model_eval_metrics.json), [log 50 epoch](assets/yolo26_runs/vehicle_annotation/results.csv), [10 ảnh prediction](assets/eval_predictions/visuals/), [confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix.png), [PR curve](assets/yolo26_runs/vehicle_eval/BoxPR_curve.png).

### 1. Lớp nào recall thấp nhất? Ảnh nào minh họa?

Theo log `best_model.val()`, recall của car=0.840, truck=0.349, bus=0.500, van=0.417. Truck thấp nhất. Ở [drive_052.jpg](assets/eval_predictions/visuals/drive_052.jpg), nhãn eval có một truck rất nhỏ gần phía trên ảnh (tâm chuẩn hóa khoảng x=0.732, y=0.037); ảnh prediction conf=0.25 không có box mang lớp truck. Đây là ví dụ cần rà nhận diện xe nhỏ/xa. Ngược lại, [drive_059.jpg](assets/eval_predictions/visuals/drive_059.jpg) có truck nhỏ bên kia dải phân cách được dự đoán truck 0.48. Không suy rằng mọi xe tải đều bị bỏ sót. Ảnh 055 không có gold truck nên không dùng làm ví dụ bỏ sót truck. Ảnh prediction ở ngưỡng cố định không tái tạo nguyên phép tính recall tổng hợp của validation.

### 2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm gì?

Đây là tín hiệu cần kiểm độ chính xác định vị box: tâm, chiều rộng/cao, cạnh dư nền và cách bao phần xe bị che/cắt khung. mAP50–95 trung bình trên các ngưỡng IoU 0.50–0.95 nên khắt khe hơn mAP50. Trong lần chạy này, 0.5960 so với 0.4055 cho thấy hiệu quả giảm ở yêu cầu khớp chặt hơn; riêng chênh lệch này không đủ kết luận nhãn train sai hoặc xác định một cạnh box cụ thể.

### 3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ?

Ba chỉnh sửa class học viên cung cấp ở mục 2 cho thấy car/van và truck/bus là các cặp cần định nghĩa nhất quán. Trong confusion matrix model, trục ngang là lớp thật, trục dọc là lớp dự đoán: có 4 truck khớp vị trí nhưng dự đoán car, 1 van dự đoán bus và 1 bus dự đoán van. Các ô này hỗ trợ ưu tiên rà cặp lớp, chưa chứng minh guideline là nguyên nhân. Gold eval chỉ có 5 truck, 2 bus, 12 van so với 94 car; số mẫu ít khiến chỉ số từng lớp nhạy với từng lỗi. Cần phân biệt bán tải thuộc car theo đề, không phân lớp chỉ dựa kích thước hoặc tự đoán mục đích sử dụng.

### 4. Vì sao không dùng eval để train? Vai trò validation là gì?

Eval được giữ ngoài tối ưu trọng số để tránh rò rỉ giữa dữ liệu train và dữ liệu đánh giá theo split của bài. Trong notebook, chính 10 ảnh này còn được dùng validation qua các epoch và chọn best.pt, nên chúng không phải test độc lập và kết quả có thể lạc quan so với dữ liệu hoàn toàn mới. Không đổi split để tăng metric; nếu cần kết luận tổng quát hóa ngoài bài lab, phải có test độc lập. Đưa eval vào train làm mất ý nghĩa split, nhưng không bảo đảm mọi metric sẽ cao.

Bản notebook có outputs phiên chạy và phần phân tích đã hiệu đính: [day2_executed_NguyenChiTai.ipynb](../../notebooks/day2_executed_NguyenChiTai.ipynb). Bản Drive giữ câu trả lời ban đầu; báo cáo và notebook trong repo ghi phần đối chiếu bổ sung.

## 6. Khó khăn và cách xử lý

- Đánh giá lần đầu F1=0.5354 chưa đạt: giữ JSON và commit đầu, sửa nhãn học viên trên CVAT rồi chạy lại; kết quả cuối đạt gate.
- Một bản xuất label_job2 giống 50/50 file cũ: so sánh byte phát hiện chưa có cập nhật; học viên xuất lại bản mới.
- Nhập ZIP gold vào task tham chiếu báo thiếu YAML: chuẩn bị ZIP cấu trúc Ultralytics với data.yaml/train.txt và 50 nhãn nguyên trạng; file gold/ZIP không commit.
- Thu thập kết quả Colab: giải nén hai thư mục assets, giữ results.csv và ảnh; loại checkpoint và ZIP lồng nhau khỏi bài nộp.

## 7. Tự đánh giá theo rubric

- Đã có 50 nhãn đúng cấu trúc, schema cố định, mini guideline, ba quyết định class và các ca đối chiếu trực quan.
- Gold QC trước/sau được lưu; bản cuối vượt hai ngưỡng. F1/IoU phản ánh mức khớp gold, không tự chứng minh mọi quyết định nội dung đều đúng.
- Pipeline Colab đã chạy, đủ metrics/ảnh/log và giữ split. Model còn hạn chế ở truck và các lớp ít mẫu; không tự quy mọi lỗi cho annotation.
- Các điểm cần giảng viên xem xét: phạm vi gold khác một số xe nhìn thấy ở bản đầu; thời điểm khóa nhãn chính thức theo lớp không thể xác minh chỉ từ Git. Không tự nhận điểm số hoặc xác nhận thay giảng viên.

## 8. Công cụ AI và tài liệu tham khảo

- CVAT: học viên tự gán nhãn và sửa nhãn; Colab/Ultralytics: train và đánh giá model.
- Codex: chuẩn bị cấu trúc báo cáo/guideline, kiểm định dạng, chạy script đánh giá chính thức, đối chiếu ảnh và nhãn trước/sau, kiểm tra ảnh prediction/confusion matrix, hiệu đính câu trả lời theo bằng chứng, quản lý file/commit. Các quan sát bổ sung do Codex hỗ trợ, không trình bày là ghi chép quan sát ban đầu của học viên.
- Học viên xác nhận đã rà 10 ảnh ở mục 3 và cung cấp ba thay đổi class. Không tạo nhãn hoặc số liệu giả; kết quả đều lấy từ script/log.
- Nguồn: [GUIDE.md](../../GUIDE.md), [RUBRIC.md](../../RUBRIC.md), [SUBMISSION.md](../../SUBMISSION.md), https://docs.cvat.ai/docs/dataset_management/formats/format-yolo-ultralytics/.
