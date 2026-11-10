# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe từ 4-9 chỗ, cỡ nhỏ/trung, đi lại cá nhân|Sedan, SUV, Hatchback. Khác van: không có thùng hàng cao vuông phía sau. |
| `truck` |Xe chở hàng cỡ lớn, cabin và thùng hàng biệt lập | Xe tải thùng, xe ben, đầu kéo. Khác van: lớn hơn, thường > 6 bánh.|
| `bus` |Xe chở khách số lượng lớn (> 15 người), thân dài |Xe buýt, xe khách đường dài. Khác van: chiều dài thường > 6m. |
| `van` |Xe thương mại cỡ vừa, trần cao, đuôi vuông phẳng|Ford Transit, xe tải van đô thị. Khác car: đuôi dốc đứng, cửa sau dạng lùa/mở đôi. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Ôm sát rìa ngoài cùng(gương, lốp, cản xe). 
- Vật thể bị che khuất: Vẽ ước lượng toàn bộ xe dựa trên phần nhìn thấy. Bỏ qua nếu bị che khuất > 80% không thể nhận diện.
- Vật thể bị cắt bởi rìa ảnh: Vẽ ôm sát phần xuất hiện trong ảnh. Biên của box dừng ngay tại rìa ảnh.
- Vật thể rất nhỏ/mơ hồ: Kích thước quá nhỏ (< 15px) hoặc quá mờ ở xa thì bỏ qua

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| [drive_021.jpg](/dataset/images/train/drive_021.jpg) |Một chiếc car nằm sát mép ảnh, bị cắt một phần |Gán nhãn car, box giới hạn trong phần nhìn thấy |Vẫn nhận diện được class; không mở rộng box ra ngoài khung hình |
| [drive_015.jpg](/dataset/images/train/drive_015.jpg) |Một chiếc car xuất hiện mở mờ giống với xe van|Gán nhãn car vì có thể xác định được |Phân lớp theo đặc điểm của phương tiện, không suy đoán khi ảnh không đủ thông tin |
| [drive_039.jpg](/dataset/images/train/drive_039.jpg) |Chiếc xe bus chỉ hiển thị tầm 30% trong khung ảnh|Quyết định gán class là bus |Xác định được đó là xe bus vì có chiều dài khá dài và đang dừng đón khách |
