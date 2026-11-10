# Báo cáo cá nhân — Ngày 2 Object Detection

Trạng thái: đã xuất nhãn CVAT và kiểm tra cấu trúc đạt 50/50; chưa xác nhận rà bằng mắt 10 ảnh, đã chạy gold QC sau rework và đạt cả F1/mean IoU; chưa huấn luyện. Các mục chưa có kết quả phải được bổ sung trước khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Chí Tài (kiểm tra lại dấu theo hồ sơ)
- MSSV: 2A202603223
- Email:
- Lớp:
- Link repo bài làm: https://github.com/nguyen-chi-tai-2005/K5-L2-Day02-NguyenChiTai-2A202603223-ObjectDetection
- Link mở notebook Colab (chưa chạy): https://colab.research.google.com/github/nguyen-chi-tai-2005/K5-L2-Day02-NguyenChiTai-2A202603223-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb
- Link bản Colab cá nhân đã chạy: Chờ bổ sung

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Ba quyết định học viên cung cấp: `drive_014.jpg` / ID411: car → van; `drive_017.jpg` / ID467: van → car; `drive_039.jpg` / ID936: truck → bus. Học viên xác nhận các thay đổi đã nằm trong bản xuất CVAT được nhập tại commit `b7aad2f`. Mini guideline có giải thích theo schema; dấu hiệu hình ảnh cụ thể chưa được học viên mô tả.

## 3. Annotation và self-QC

- Công cụ gán nhãn: CVAT; xuất YOLO detection, kiểm tra mapping car=0, truck=1, bus=2, van=3.
- Số file nhãn train: 50; bản rework hiện tại có 681 box (car: 560, truck: 46, bus: 21, van: 54); bản đầu có 1.170 box. Đây là thống kê nhãn xuất, không tự chứng minh độ đúng.
- Kết quả checker và lỗi đã xử lý: `python3 tools/check_yolo_labels.py` đạt 50/50; không phát hiện lỗi cấu trúc. Mapping bản xuất đúng car=0, truck=1, bus=2, van=3.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: Chờ học viên xác nhận và ghi tên ảnh.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Học viên cung cấp 3 thay đổi lớp nêu ở mục 2. Học viên xác nhận bản xuất CVAT đã chứa cả ba thay đổi trước khi nhập vào repo; không sửa nhãn thêm từ ID đối tượng.
- Commit khóa nhãn ban đầu: Chờ hoàn tất self-QC và khóa theo mốc lớp; commit nhập nhãn CVAT chưa được coi là xác nhận hoàn thành bước này.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 488 | 638 |
| FP | 682 | 43 |
| FN | 165 | 15 |
| Precision | 0.4171 | 0.9369 |
| Recall | 0.7473 | 0.977 |
| F1 | 0.5354 | 0.9565 |
| mean_iou_of_matches | 0.8377 | 0.9993 |
| passed | false | true |

Kết quả lần đầu: [annotation_evaluation_before_rework.json](../../outputs/annotation_evaluation_before_rework.json). Kết quả mới nhất: [annotation_evaluation.json](../../outputs/annotation_evaluation.json). Đủ 50 file gold khớp tên ảnh train; checker nhãn học viên đạt 50/50. Chưa đạt điều kiện train: F1 = 0.5354 < 0.80, dù mean IoU = 0.8377 >= 0.70. Bộ nhãn học viên có 1.170 box, gold có 653 box; TP=488, FP=682, FN=165. Chênh lệch số lượng và FP/FN chỉ là tín hiệu cần rà lại ảnh, không chứng minh từng box thừa/thiếu hoặc nguyên nhân sai lớp. Giữ bản ban đầu tại commit `b7aad2f`; đã nhận bản rework cập nhật trong `label_job2`. Gold chỉ dùng cục bộ, không đưa lên repo hoặc Colab. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

Kiểm tra bản `label_job2`: checker đạt 50/50, mapping lớp đúng. So sánh từng byte cho thấy cả 50 file nhãn giống bản ban đầu trong repo, không có file thay đổi. Chạy evaluator lại cho cùng kết quả F1=0.5354, mean IoU=0.8377, passed=false; chưa thể ghi nhận bộ nhãn rework mới. Cần học viên kiểm tra lưu và xuất đúng task/job đã chỉnh sửa trên CVAT.

Lần xuất lại tiếp theo của `label_job2`: cả 50 file đã thay đổi so với bản đầu, checker đạt 50/50. Kết quả mới: Precision=0.5979, Recall=0.6876, F1=0.6396, mean IoU=0.8404; TP=449, FP=302, FN=204. F1 tăng nhưng chưa đạt 0.80, nên chưa train. Học viên chưa cung cấp danh sách lỗi và lý do sửa theo từng ảnh; cần bổ sung từ quá trình rà thực tế. Không kết luận nguyên nhân chỉ từ số liệu.

Kiểm tra bản xuất mới nhất: cả 50 file thay đổi so với lần rework trước, checker đạt 50/50. Precision=0.9369, Recall=0.9770, F1=0.9565, mean IoU=0.9993; TP=638, FP=43, FN=15; passed=true. Đã đạt cả hai ngưỡng annotation để chuyển sang train Colab. Kết quả vòng rework trước được giữ tại [annotation_evaluation_rework1.json](../../outputs/annotation_evaluation_rework1.json). Các đoạn mô tả chưa đạt phía trên là lịch sử các lần đánh giá, không phải trạng thái hiện tại. Học viên cần bổ sung mô tả việc sửa thực tế theo từng ảnh; chưa có kết quả model.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | |
| Recall | |
| mAP50 | |
| mAP50–95 | |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

- Codex: hỗ trợ đọc yêu cầu, lập thứ tự thực hiện, chuẩn bị khung báo cáo, URL repo trong notebook, checklist CVAT và bản nháp định nghĩa lớp/quy tắc mini guideline theo đề bài. Ba ca mơ hồ thực tế do học viên tự ghi và xác nhận. Không tạo nhãn hoặc kết quả đánh giá.
- Phần học viên tự kiểm tra: Chờ bổ sung các bước đã thực hiện thực tế.
- Tài liệu công cụ: https://docs.cvat.ai/docs/dataset_management/formats/format-yolo-ultralytics/
- Không ghi mật khẩu/token hoặc đính kèm gold train.
