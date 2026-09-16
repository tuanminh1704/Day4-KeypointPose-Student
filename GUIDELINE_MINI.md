# Mini guideline - nhóm:  |  người gán: Vũ Tuấn Minh  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Dùng `v=1`, căn vị trí theo trục đối xứng xương chậu và khớp gối | Khớp hông còn trong khung hình, chỉ bị vải áo/quần che khuất |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Dùng `v=1`, ngoại suy từ đuôi mắt và góc xương hàm | Giúp model học được vị trí tương quan của tai ngay cả khi bị che |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Chân dưới mép ảnh dùng `v=0` (tọa độ `0.0 0.0`) | Khớp thật sự nằm ngoài khung hình (Out of frame) |
| Cổ tay nằm sau tay lái / sau thân mình | Dùng `v=1`, đặt chấm tại vị trí khớp cổ tay tiếp giáp bàn tay/tay lái | Khớp nằm trong khung nhưng bị vật thể chắn |
| Hai người chồng lên nhau | Gán đủ 17 khớp cho từng người; khớp người sau bị người trước che dùng `v=1` | Tránh bỏ sót người và duy trì tính toàn vẹn của skeleton |
| Người nhỏ đến mức nào thì không gán nữa | Bounding box có chiều cao hoặc chiều rộng < 20px thì bỏ qua | Quá mờ để xác định được cấu trúc giải phẫu 17 điểm |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_03.jpg`, người thứ `1`, khớp `left_shoulder`

- Mơ hồ ở chỗ nào: Vai trái bị cánh tay người đứng trước che khuất một phần lớn.
- Bạn quyết thế nào: Dùng `v=1`, đặt chấm tại vị trí đối xứng giải phẫu với vai phải (`right_shoulder`).
- Vì sao: Thân trên người này vẫn nằm trọn trong khung hình, không bị cắt mép.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu dùng `v=0`, model sẽ học thói quen coi khớp bị che là ngoài khung hình và mất khả năng dự đoán pose khi có che khuất.

### Ca 2 - ảnh `train_13.jpg`, người thứ `2`, khớp `left_ankle` / `right_ankle`

- Mơ hồ ở chỗ nào: Bàn chân và mắt cá chân bị người đứng phía trước che gần hết, chỉ thấy mờ phần ống chân.
- Bạn quyết thế nào: Đặt cờ `v=1`, ước lượng vị trí mắt cá chân theo phương thẳng đứng từ đầu gối xuống sàn.
- Vì sao: Người đang ở tư thế đứng thẳng, tiếp đất trong khung ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ bị cụt chân hoặc hút nhầm keypoint sang chân của người đứng trước.

### Ca 3 - ảnh `train_01.jpg`, người thứ `2`, khớp `left_ear`

- Mơ hồ ở chỗ nào: Đầu quay nghiêng góc 3/4, tai trái hoàn toàn bị tóc và góc mặt che khuất.
- Bạn quyết thế nào: Dùng `v=1`, ước lượng vị trí tai dựa vào khoảng cách từ mũi và mắt trái.
- Vì sao: Đầu vẫn trong khung ảnh, không bị cắt bởi mép ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ nhầm lẫn giữa việc không nhìn thấy do góc nghiêng với việc cơ thể bị crop ngoài ảnh.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_hip / right_hip` (bạn `38%` / họ `17%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline chưa rõ về việc xử lý người mặc áo dài trùm hông dẫn đến bên bạn kia gán `v=0` (nhầm sang outside).
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Người mặc áo dài hoặc váy che hông nhưng còn trong ảnh thì bắt buộc dùng `v=1`, không được dùng `v=0`.
