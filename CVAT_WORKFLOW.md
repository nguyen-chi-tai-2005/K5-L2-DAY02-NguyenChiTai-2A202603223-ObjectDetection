# Tiếp nhận nhãn CVAT và hoàn thành Lab 2

Trạng thái chuẩn bị: chưa có nhãn train, chưa chạy gold QC hoặc model. Tài liệu này là checklist thao tác, không phải bằng chứng đã hoàn thành.

## 1. Trong khi gán nhãn

- Chỉ đưa 50 ảnh `dataset/images/train/` vào task CVAT, giữ nguyên tên ảnh.
- Bốn lớp: `0 car`, `1 truck`, `2 bus`, `3 van`.
- Dùng Rectangle, mỗi xe một box sát phần nhìn thấy; không đoán phần khuất hoặc ngoài khung.
- Học viên tự điền `GUIDELINE_MINI.md`, gồm ít nhất 3 ca mơ hồ có tên ảnh, quyết định và lý do. Hỏi giảng viên với ca chưa rõ.

## 2. Xuất và self-QC

- Lưu annotation trong CVAT, xuất `Ultralytics YOLO Detection 1.0`.
- Giải nén ra thư mục tạm; kiểm tra mapping lớp trong YAML của bản xuất.
- Chỉ chép 50 file nhãn tương ứng vào `dataset/labels/train/`; không ghi đè `data.yaml` gốc.
- Nếu thiếu file ở ảnh không có xe, mở ảnh xác nhận trước khi tạo file `.txt` rỗng.
- Chạy từ thư mục gốc repo:

```bash
python3 tools/check_yolo_labels.py
```

- Rà bằng mắt ít nhất 10 ảnh và ghi tên ảnh/lỗi đã sửa vào báo cáo. Checker không kiểm được đúng lớp, bỏ sót hoặc độ sát box.

## 3. Khóa nhãn trước khi nhận gold

```bash
git add dataset/labels/train GUIDELINE_MINI.md
git commit -m "Lock initial annotations before gold evaluation"
git push
git rev-parse HEAD
```

Ghi mã commit vào báo cáo; khóa/nộp theo mốc của giảng viên. Không viết lại lịch sử bản đã khóa.

## 4. Gold QC, sau khi được giảng viên phát

Đặt gold ở `gold_train_labels/` trên máy, không commit hoặc đưa lên Colab.

```bash
python3 tools/evaluate_annotations.py \
  --student-labels dataset/labels/train \
  --gold-labels gold_train_labels \
  --output outputs/annotation_evaluation.json
```

Ghi số liệu lần đầu vào báo cáo trước khi chạy lại. Chỉ đi tiếp khi F1 >= 0.80 và mean_iou_of_matches >= 0.70. Nếu cần sửa: sửa nhãn của mình trên CVAT, xuất lại, chạy checker và evaluator; giữ bản khóa ban đầu. Nếu vẫn chưa đạt, báo giảng viên.

## 5. Colab và báo cáo

- Commit/push nhãn đã QC trước khi mở notebook; URL repo đã điền sẵn.
- Chạy notebook từ trên xuống dưới, ưu tiên GPU; không đưa eval vào train.
- Tải `day2_colab_outputs.zip`, giải nén cả `eval_predictions/` và `yolo26_runs/` vào `report/individual/assets/`.
- Điền 4 chỉ số từ `model_eval_metrics.json`, link Colab đã chạy, ảnh minh họa và 4 câu hỏi phân tích vào `report/individual/2A202603223_NguyenChiTai.md`.
- Không nộp `.pt`; `.gitignore` ngăn thêm file mới nhưng không xóa file trên ổ đĩa.

## 6. Chốt bài

```bash
git add dataset/labels/train GUIDELINE_MINI.md report/individual
git add -f outputs/annotation_evaluation.json
git diff --cached --name-only
git status
```

Kiểm tra không có gold, token, mật khẩu hoặc checkpoint trước khi commit/push. Mở GitHub xác nhận đủ nhãn, JSON, báo cáo và assets; nộp link repo trên LMS, bảo đảm người chấm xem được repo và Colab. Deadline theo đúng ca học và thông báo LMS.

Nguồn: RULES.md, CHECKPOINTS.md, GUIDE.md, SUBMISSION.md và https://docs.cvat.ai/docs/dataset_management/formats/format-yolo-ultralytics/.
