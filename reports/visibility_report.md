# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.72 khớp có v > 0 mỗi người
- Tổng: v=2 314 | v=1 142 | v=0 37

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 8 | 0 | 28% |
| 1 | left_eye | 19 | 10 | 0 | 34% |
| 2 | right_eye | 20 | 9 | 0 | 31% |
| 3 | left_ear | 10 | 19 | 0 | 66% |
| 4 | right_ear | 13 | 15 | 1 | 52% |
| 5 | left_shoulder | 26 | 2 | 1 | 7% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 23 | 5 | 1 | 17% |
| 8 | right_elbow | 26 | 3 | 0 | 10% |
| 9 | left_wrist | 19 | 9 | 1 | 31% |
| 10 | right_wrist | 18 | 9 | 2 | 31% |
| 11 | left_hip | 16 | 11 | 2 | 38% |
| 12 | right_hip | 17 | 11 | 1 | 38% |
| 13 | left_knee | 16 | 8 | 5 | 28% |
| 14 | right_knee | 15 | 9 | 5 | 31% |
| 15 | left_ankle | 15 | 5 | 9 | 17% |
| 16 | right_ankle | 13 | 7 | 9 | 24% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
