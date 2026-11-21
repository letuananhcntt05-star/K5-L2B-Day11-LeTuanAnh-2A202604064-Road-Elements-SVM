# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): LeTranPhiHung        Người kiểm (học viên): Lê Tuấn Anh (letuananh)  
Ngày: 21/11/2026                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `LeTranPhiHung-front.zip`, `LeTranPhiHung-fisheye.zip` — không nhận được manifest, coi là revision 1 (CVAT export ngày 21/11/2026, 11:04–11:05)  
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 122  

Ảnh được kiểm: A01, A04, A08, A11 (front) · B02 (fisheye_front, đuôi FV) · B06 (fisheye_left, đuôi MVL).
Số nhãn theo ảnh: A01 = 11 + 4 tag, A04 = 13 + 4, A08 = 41 + 4, A11 = 11 + 4, B02 = 9 + 4, B06 = 13 + 4 → 98 shape + 24 tag = 122.
Cách kiểm: mở nhãn đè lên ảnh gốc, phóng to từng vạch, đèn và biển; chạy `road_check lint` trên cả 2 file làm gợi ý.

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 | 2 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 5 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 22 | 1 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 22 | 2 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 22 | 3 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 22 | 1 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 37 | 1 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 21 | 2 | 1 nặng + 1 chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 39 | 5 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 18 | 1 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 22 | 4 | chính |
| QC1-12 file | lỗi file | 2 | 0 | nhẹ / nặng |

Cách đếm "Số mục kiểm":
- QC1-01: 1 tag camera mỗi ảnh.
- QC1-02: 5 ảnh cần có L1/R1 (4 ảnh front + B02). B06 là camera hai bên nên không áp dụng.
- QC1-03 đến QC1-06: 22 polyline lane.
- QC1-07: 12 drivable + 8 crosswalk + 17 road_curb.
- QC1-08: 21 đèn.
- QC1-09: 21 đèn + 18 biển.
- QC1-10: 18 biển.
- QC1-11: 22 shape trên 2 ảnh fisheye.
- QC1-12: 2 file zip.

QC1-02 ghi 0 lỗi vì ở A01 lint báo thiếu R1, nhưng nguyên nhân là vạch R1 đã được vẽ và gán nhầm position L2. Lỗi này đã ghi ở QC1-03 (lỗi #3), không đếm hai lần.

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-01 | B02 | tag camera | nặng | Ghi `fisheye_rear`, nhưng ảnh đuôi `_FV` nên phải là `fisheye_front` |
| 2 | QC1-01 | B06 | tag camera | nặng | Ghi `fisheye_right`, nhưng ảnh đuôi `_MVL` nên phải là `fisheye_left` |
| 3 | QC1-03 | A01 | lane #9 | nặng | Vạch trắng đứt sát bên phải làn mình bị gán `position = L2`, phải là **R1**. Hệ quả: ảnh thiếu R1 và có 2 polyline cùng L2 |
| 4 | QC1-08 | A08 | traffic_light #16 | nặng | Đèn quay về xe đang sáng **xanh**, nhưng ghi `color = unknown` |
| 5 | QC1-04 | A08 | lane #20 (R1) | chính | Chỉ có 2 điểm, điểm đầu nằm giữa vạch ngựa vằn: polyline kéo xuyên qua crosswalk, không dừng ở mép vạch |
| 6 | QC1-04 | A11 | lane #8 (L1) | chính | Vẽ từ xa về gần (điểm đầu y = 429, điểm cuối y = 720), chỉ có 2 điểm |
| 7 | QC1-05 | A04 | lane #2, #10 (L1) | chính | Vàng đôi tách thành 2 polyline cùng L1; phải là 1 polyline giữa hai nét, `kind = double` |
| 8 | QC1-05 | A08 | lane #39, #40 (L2) | chính | Một vạch L2 bị tách thành 2 polyline nối đuôi nhau |
| 9 | QC1-05 | A11 | lane #8, #10 (L1) | chính | Vàng đôi tách thành 2 polyline cùng L1 |
| 10 | QC1-06 | A01 | lane #5 (L2) | chính | Vạch trắng liền mép trái là một nét, nhưng ghi `kind = double` |
| 11 | QC1-07 | A11 | drivable #3 | chính | Polygon `direct` thứ hai phủ dải xe đỗ sát lề phải. Dải đỗ không tính là `direct`, và cũng không phải làn mình đang đi |
| 12 | QC1-08 | A08 | traffic_light #7 | chính | Đèn nhìn nghiêng, không thấy bóng đang sáng, nhưng ghi `color = red` |
| 13 | QC1-09 | A08 | traffic_light #7 | chính | Đèn nhìn nghiêng, ghi `facing = front`, phải là `side` |
| 14 | QC1-09 | A08 | traffic_light #8 | chính | Đèn vỏ vàng nhìn nghiêng ở cột phải, ghi `facing = front`, phải là `side` |
| 15 | QC1-09 | A08 | traffic_light #28 | chính | Thấy rõ bóng xanh đang sáng (đèn quay về xe), nhưng ghi `facing = back` |
| 16 | QC1-09 | A08 | traffic_sign #30 | chính | Biển xanh chỉ hướng "Harlem River Dr / RFK Br" ghi `group = prohibitory`, phải là `information` |
| 17 | QC1-09 | A04 | traffic_sign #7 | chính | Biển tên đường (xanh, nhìn thẳng) ghi `group = other`, phải là `information` |
| 18 | QC1-10 | A08 | (thiếu) | chính | Bỏ sót tấm biển nhỏ "PASSENGER CARS ONLY" ngay trên biển #30 (cao khoảng 26 px, trên 8 px nên bắt buộc gán) |
| 19 | QC1-11 | B02 | traffic_light #6 | chính | Tâm hộp nằm ngoài 80 % bán kính vòng ảnh nhưng `edge_zone` tắt |
| 20 | QC1-11 | B02 | traffic_light #7 | chính | Như trên: `edge_zone` phải bật |
| 21 | QC1-11 | B02 | traffic_light #8 | chính | Như trên: `edge_zone` phải bật |
| 22 | QC1-11 | B06 | drivable #1 | chính | Polygon lấn xuống thân xe mình (phần cửa xe ở nửa dưới ảnh, tới y ≈ 945). Thân xe không được gán |

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A11 | `timeofday = night`, `weather = overcast`, trong khi trời còn xanh đậm (chạng vạng) và không thấy mây. Có nên là `dusk` / `clear`? Không tính lỗi vì guideline không định nghĩa ranh giới dusk/night | |
| A04, A08, B02 | `timeofday = dusk` cho ảnh trời âm u ban ngày. Âm u ban ngày nên là `day`? | |
| A04 | Lane #0 (R2): hai nét trắng song song (dải đệm làn xe đạp) nên `kind = single` hay `double`? | |
| B02 | Drivable #2 chỉ phủ mặt đường phía sau vạch ngựa vằn. Phần mặt đường dưới crosswalk có cần tính vào drivable không? | |
| A08 | Traffic_light #29 (hộp màu cam ở cột phải) là đèn đi bộ hay biển? | |
| Lô của tôi (letuananh): A05, A06, A07, A09 | Lint báo QC1-02 (thiếu L1/R1) ở các ảnh không thấy vạch: A05 không có vạch kẻ; A06 bên phải là hàng cột đường sắt trên cao; A07 mép trái là dải bê tông (đã gán `road_curb`); A09 mép phải là vỉa (đã gán `road_curb`). Đề nghị coach xác nhận miễn theo CHECKPOINTS | [chờ coach] |

## Quyết định
Lỗi nặng: 4   Lỗi chính: 18 / 122 nhãn = 14,8 %   Lỗi nhẹ: 0   Ca chờ lead: 5 (không ảnh hưởng kết luận)  
Quyết định (Release / Hold / Rework): **Rework**  
Lý do (theo luật của phiếu): Có 4 lỗi nặng (2 tag camera sai, 1 sai `position`, 1 sai màu đèn `facing = front`), mỗi lỗi đủ để Rework. Ngoài ra, tỉ lệ lỗi chính 14,8 % vượt ngưỡng 5 %. Các ca mơ hồ không làm thay đổi kết luận, nên không chọn Hold.

Ghi chú thêm ngoài mẫu (lint trên cả lô, chỉ là gợi ý, không tính vào tỉ lệ trên):
- Tag camera còn sai ở **B10** (`fisheye_right`, phải là `fisheye_left`) và **B11** (`fisheye_left`, phải là `fisheye_rear`).
- Lint báo vạch đứt có thể bị tách (QC1-05) ở A09, A14, A15.
- Lint báo thiếu L1/R1 (QC1-02) ở A03, A05, A06, A07, A09, A10, A13, A14. Cần tự kiểm xem có thật sự không thấy vạch không.
- Fisheye chỉ đặt 2–3 điểm (QC1-11) ở B05, B07, B09, B11, B12.
- Có điểm lặp liền nhau ở B01 road_curb #6 và B07 drivable #13.

Đề nghị sửa tất cả lỗi trên, export lại giữ nguyên tên zip, tạo manifest mới với `--revision 2`.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](../docs/qc_severity_road_v1.0.md).
