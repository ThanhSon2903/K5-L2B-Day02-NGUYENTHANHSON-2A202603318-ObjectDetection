# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Nguyễn Thanh Sơn
- MSSV: 2A202603318
- Email: thanhnguyensonjqk@gmail.com
- Lớp: 2B
- Link repo bài làm: https://github.com/ThanhSon2903/K5-L2B-Day02-NGUYENTHANHSON-2A202603318-ObjectDetection/tree/main
- Link Colab đã chạy: https://colab.research.google.com/drive/1WmmwjX_keUk0lKgUhhz1DMwV7V0A6aIW?hl=vi#scrollTo=tzq7xlHYVLKZ

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC 

- Số file nhãn train: 50
- Kết quả checker và lỗi đã xử lý: Đạt cấu trúc, 50/50 file nhãn hợp lệ
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001.jpg, drive_002.jpg,drive_003.jpg,drive_004.jpg,drive_005.jpg,drive_006.jpg,drive_007.jpg,drive_008.jpg,drive_009.jpg,drive_010.jpg,drive_011.jpg,drive_012.jpg,
- Lỗi bỏ sót: Một số phương tiện bị che khuất hoặc nằm sát mép ảnh chưa được gán nhãn. Đã kiểm tra lại ảnh gốc và bổ sung bounding box cho các đối tượng nhận diện được.
- Lỗi box: Một số bounding box chưa bao sát phần vật thể nhìn thấy hoặc còn chứa nhiều nền. Đã điều chỉnh tọa độ box.
- Lỗi class: Một số phương tiện có đặc điểm gần giống nhau giữa car, truck và van. Đã đối chiếu guideline và điều chỉnh class khi đủ căn cứ.
- Commit khóa nhãn ban đầu: `0e217fb` (feat: complete object detection dataset)

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |406 | |
| FP |329| |
| FN | 247| |
| Precision | 0.5524| |
| Recall | 0.6217| |
| F1 | 0.585| |
| mean_iou_of_matches | 0.8521| |
| passed |false| |


## 5. Kết quả YOLO

- Runtime: Google Colab
- GPU/CPU: NVIDIA Tesla T4
- Phiên bản Ultralytics và Torch: 3.13.16 và 8.4.208
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy: outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.410 |
| Recall |0.234 |
| mAP50 |0.270|
| mAP50–95 |0.199|

### Kết quả YOLO

- Link thư mục ảnh dự đoán: [eval_predictions](assets/eval_predictions/)
- Link kết quả train/evaluate: [yolo26_runs](assets/yolo26_runs/)

### Phân tích ảnh dự đoán
- Cụ thể trong bức ảnh [drive_056.jpg](assets/eval_predictions/visuals/drive_056.jpg) thì bằng bắt thường có thể quan sát được là chiếc car nhưng mô hình lại dự đoán là class van
- Nhận xét: Mô hình chưa phân biệt chính xác giữa hai class car và van. Điều này có thể xuất phát từ sự tương đồng về hình dáng bên ngoài của phương tiện hoặc đặc trưng hình ảnh chưa đủ rõ để mô hình nhận diện chính xác. Đây là một trường hợp phân lớp nhầm (False Positive đối với class van và có thể là False Negative đối với class car). Cần kiểm tra lại bounding box, nhãn Ground Truth và đối chiếu với quy tắc phân lớp trong guideline để xác định nguyên nhân cụ thể.

Trả lời bốn câu hỏi cuối notebook:
1. Lớp có recall thấp nhất là bus (Recall = 0). Cụ thể là bức ảnh `drive_058.jpg`
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm tra độ chính xác vị trí và kích thước bounding box. Box có thể phát hiện đúng đối tượng nhưng chưa khớp sát với Ground Truth ở các ngưỡng IoU cao.
3. Các lỗi liên quan đến class truck, bus và van có thể liên quan đến quy tắc phân biệt class trong guideline. Tuy nhiên, cần xem ảnh dự đoán và Ground Truth cụ thể để xác nhận, không thể kết luận chỉ từ metrics tổng hợp.
4. Không dùng 10 ảnh eval để train vì sẽ gây rò rỉ dữ liệu, làm mất tính khách quan của đánh giá. Trong notebook, dữ liệu eval được sử dụng để validation, theo dõi hiệu suất và lựa chọn checkpoint tốt nhất (best.pt), sau đó đánh giá kết quả dự đoán.

{
  ``"precision": 0.4090642760207564``,
  ``"recall": 0.23393606659813174``,
  ``"map50": 0.2706670034721526``,
  ``"map50_95": 0.1995504032766255``,
  ``"eval_images": 10``,
  ``"checkpoint": "/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt"``
}


## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Gold QC |Đường dẫn thư mục Ground Truth bị lồng hai cấp |Kiểm tra lại cấu trúc thư mục và chạy lại lệnh evaluate |Đã chạy đánh giá thành công |
|Annotation checker |Kiểm tra cấu trúc nhãn YOLO |Chạy tools/check_yolo_labels.py |50/50 file hợp lệ |
|YOLO evaluation |Hiệu suất một số class còn thấp |Kiểm tra Precision, Recall và mAP theo class |Cần tiếp tục phân tích ảnh dự đoán |

## 7. Tự đánh giá theo rubric

| Tiêu chí | Kết quả thực tế | Tự đánh giá |
|---|---|---
|Hoàn thành và định dạng YOLO (10 điểm) |Có 50 file nhãn train, checker xác nhận 50/50 file hợp lệ về cấu trúc |Đã kiểm tra cấu trúc, chưa xác nhận toàn bộ chất lượng nội dung |
|Độ bám sát và hình học box (20 điểm) |Đã chạy Gold QC, mean IoU = 0.8521 |Đạt ngưỡng mean IoU; vẫn cần kiểm tra các box lỗi |
|Đầy đủ vật thể (20 điểm) |Đã thực hiện annotation cho 50 ảnh train |Cần đối chiếu thêm với ảnh gốc để xác nhận không bỏ sót vật thể |
|Phân lớp nhất quán (15 điểm)|Đã sử dụng bốn class car, truck, bus, van |Cần rà soát thêm các trường hợp phân lớp nhầm |
|Mini guideline và quyết định mơ hồ (10 điểm) |Đã có GUIDELINE_MINI.md |Cần đảm bảo có định nghĩa lớp và ít nhất ba trường hợp mơ hồ kèm lý do|
|Gold-set annotation QC (15 điểm) |F1 = 0.585, mean IoU = 0.8521|Chưa đạt ngưỡng F1 >= 0.80; cần thực hiện rework và đánh giá lại|
|Pipeline YOLO và phân tích (10 điểm) |Đã chạy YOLO26n trên Google Colab, evaluate trên 10 ảnh eval và thu được metrics |Đã chạy pipeline, cần hoàn thiện ảnh dự đoán và phân tích lỗi cụ thể trong báo cáo|

## Tổng kết
- Đã hoàn thành annotation 50 ảnh train, kiểm tra cấu trúc nhãn, thực hiện Gold-set evaluation và chạy pipeline YOLO26n trên Google Colab.

- Kết quả Gold QC cho thấy F1 = 0.585, chưa đạt ngưỡng yêu cầu 0.80, trong khi mean IoU = 0.8521 đạt ngưỡng 0.70. Vì vậy, chất lượng annotation vẫn cần được cải thiện thông qua việc kiểm tra FN, FP và các trường hợp box/class chưa chính xác.

- Pipeline YOLO đã chạy thành công trên tập eval gồm 10 ảnh. Kết quả mAP50 = 0.270 và mAP50-95 = 0.199. Mô hình nhận diện class car tốt hơn các class còn lại. Cần tiếp tục phân tích ảnh dự đoán để xác định các lỗi cụ thể.

- Các nội dung chưa kiểm chứng hoặc chưa thực hiện được ghi nhận rõ ràng, không tự nhận đã hoàn thành.

## 8. Công cụ AI và tài liệu tham khảo

- Google Colab: Chạy notebook và huấn luyện mô hình YOLO.

- Ultralytics YOLO26n: Huấn luyện và đánh giá object detection.

- Python: Kiểm tra nhãn và xử lý dữ liệu.

- Git/GitHub: Quản lý mã nguồn và nộp bài.

- ChatGPT: Hỗ trợ giải thích lỗi, hướng dẫn Git, phân tích metrics và hoàn thiện báo cáo. Các kết quả được đối chiếu với log thực tế.

- Tài liệu tham khảo: GUIDE.md, SUBMISSION.md, RUBRIC.md và GUIDELINE_MINI.md trong repository của bài lab.
