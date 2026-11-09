# Báo cáo cá nhân — Ngày 2 Object Detection

Trạng thái: đã xuất nhãn CVAT và kiểm tra cấu trúc đạt 50/50; chưa xác nhận rà bằng mắt 10 ảnh, chưa chạy gold QC hoặc huấn luyện. Các mục chưa có kết quả phải được bổ sung trước khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

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
- Số file nhãn train: 50; tổng 1.170 box (car: 990, truck: 21, bus: 107, van: 52). Đây là thống kê nhãn xuất, chưa xác nhận độ đúng.
- Kết quả checker và lỗi đã xử lý: `python3 tools/check_yolo_labels.py` đạt 50/50; không phát hiện lỗi cấu trúc. Mapping bản xuất đúng car=0, truck=1, bus=2, van=3.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: Chờ học viên xác nhận và ghi tên ảnh.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Học viên cung cấp 3 thay đổi lớp nêu ở mục 2. Học viên xác nhận bản xuất CVAT đã chứa cả ba thay đổi trước khi nhập vào repo; không sửa nhãn thêm từ ID đối tượng.
- Commit khóa nhãn ban đầu: Chờ hoàn tất self-QC và khóa theo mốc lớp; commit nhập nhãn CVAT chưa được coi là xác nhận hoàn thành bước này.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | | |
| FP | | |
| FN | | |
| Precision | | |
| Recall | | |
| F1 | | |
| mean_iou_of_matches | | |
| passed | | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

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
