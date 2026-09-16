# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Vũ Tuấn Minh   Nhóm:    Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 314 / 142 / 37 |
| Thời gian trung bình mỗi ảnh | 4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear — 66% (19/29)
2. right_ear — 52% (15/29)
3. left_hip / right_hip — 38% (11/29)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Không hoàn toàn. Khớp tai có `%v=1` cao chủ yếu do người quay nghiêng hoặc bị tóc che lấp một bên, nhưng vị trí giải phẫu của tai tương đối dễ ngoại suy từ mắt và sống mũi. Khớp thực sự khó gán nhất là khớp hông (`left_hip`, `right_hip`) và cổ tay (`wrists`) khi bị áo khoác dài, quần rộng hoặc người khác đứng che khuất, đòi hỏi phải ước lượng khung xương chậu và trục cơ thể.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.925 | 0.942 |
| OKS@0.50 | 1.000 | 1.000 |
| OKS@0.75 | 0.966 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 3 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_03.jpg`, người #1: Khôi phục 3 khớp bị xóa (đổi từ `v=0` sang `v=1` và ước lượng vị trí): `left_shoulder` (x≈0.707, y≈0.247), `right_wrist` (x≈0.691, y≈0.338), `left_hip` (x≈0.557, y≈0.484).
- `train_13.jpg`, người #2: Căn chỉnh lại tọa độ các khớp chân (đầu gối, mắt cá chân) bị lệch vị trí giải phẫu để nâng OKS từ 0.732 lên > 0.85.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh (0 lỗi). Tôi đã tuân thủ quy tắc quy ước hướng cơ thể của người trong ảnh (bên trái người đó là left, bên phải người đó là right) thay vì nhìn theo góc nhìn của người gán.

## 3. Kiểm chéo

Bạn cùng nhóm: Thành viên nhóm đối chiếu

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_hip / right_hip | 38% | 17% | 21% | Guideline chưa rõ: Bên bạn kia đánh v=0 (outside) khi người mặc áo dài che hông, trong khi guideline yêu cầu người còn trong khung phải đánh v=1 và ước lượng vị trí. |
| left_ear / right_ear | 59% | 41% | 18% | Guideline chưa thống nhất: Tai bị tóc che một phần bên bạn kia để v=2 (nhìn thấy), bên tôi quy về v=1 (bị che). |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Khi thân người nằm hoàn toàn trong khung hình nhưng bị áo khoác dài hoặc người khác che khuất vùng hông/xương chậu: BẮT BUỘC dùng `v=1` và ước lượng vị trí hông dựa trên trục cột sống và khớp đùi/gối, TUYỆT ĐỐI KHÔNG dùng `v=0`.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   - Trả lời: `pose_mAP50-95` tăng từ 0.6853 lên 0.6908 (chênh +0.0055, tương đương +0.55%). Độ chính xác `pose_precision` cũng tăng từ 0.9734 lên 0.9792 (+0.0058). Điều này cho thấy nhãn chất lượng cao sau rework (OKS đạt 0.942) đã giúp mô hình thích nghi tốt hơn với phân phối dữ liệu cụ thể của bài toán, học cách định vị các khớp occluded tốt hơn mà không làm giảm recall (vẫn giữ nguyên 0.8462).

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
   - Trả lời: `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) là 0.1133 (11.33%). Model tìm *người* dễ hơn tìm *khớp* rất nhiều. Lý do là bounding box bao quát toàn bộ diện tích cơ thể người (đặc trưng biên dạng, quần áo lớn và rõ ràng), trong khi pose yêu cầu xác định chính xác 17 điểm keypoints cục bộ vốn dễ bị ảnh hưởng bởi che khuất (occlusion), góc quay và biến dạng giải phẫu phức tạp.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   - Trả lời: Trong tập test, model xuất hiện lỗi **lệch nhẹ** ở khớp cổ tay và mắt cá chân (đặc biệt khi người quay nghiêng hoặc khớp nằm gần mép nền có độ tương phản thấp). Ngoài ra ở người bị che khuất một phần, model có xu hướng dự đoán khớp cổ chân trượt nhẹ theo phương thẳng đứng.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   - Trả lời: Ảnh `train_13.jpg` (người #2) có OKS chênh lệch nhiều nhất giữa nhãn và model. Nhãn của tôi đúng vì dựa vào cấu trúc giải phẫu liên tục của cơ thể người khi bị che khuất và đã được đối chiếu đạt OKS > 0.85 với gold. Model bị lệch do người này đứng chen chúc sau người khác, khiến các keypoint thân dưới của model bị hút dính sang biên dạng của người đứng phía trước (lỗi trượt/nhầm người).

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   - Trả lời: Có. `train_03.jpg` và `train_13.jpg` là hai ảnh trước rework có OKS thấp nhất (0.732 - 0.752) và model cũng gặp khó khăn nhất trên các ảnh này. Điều này phản ánh bức ảnh có **độ phức tạp thị giác cao**: mật độ người dày đặc, nhiều khớp occluded (v=1) đan xen nhau, ánh sáng đổ bóng phức tạp. Những ca khó đối với con người khi gán nhãn cũng chính là những ca khó nhất đối với mô hình thị giác máy tính.

## 5. Một rule evidence bạn đã dùng

Ảnh **train_03.jpg**, người thứ 1, khớp **left_shoulder** (kp5). Trong ảnh, vai trái của người này bị tay và cánh tay người khác đứng phía trước che một phần, nhưng toàn bộ phần trên cơ thể vẫn nằm gọn trong khung — không có chi nào vượt ra ngoài mép ảnh. Căn cứ vào vị trí của right_shoulder (kp6, nhìn thấy rõ ở x≈0.568) và tỉ lệ đối xứng cơ thể, left_shoulder ước lượng nằm ở x≈0.707 về phía đối diện. Vì khớp còn trong khung và chỉ bị người khác che (không ra ngoài ảnh), luật bắt buộc yêu cầu dùng `v=1` và vẫn đặt chấm ở vị trí ước lượng — không được dùng `v=0`.
