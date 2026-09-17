# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602300
- Ngày / CVAT local: 17/09/2026 / http://localhost:8080 (CVAT v2.74.1)
- Công cụ đã dùng: CVAT 2.74.1 (Polygon, Brush, CVAT Headless REST API, YOLOv8x-seg, SegFormer)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh 000000181542.jpg, xe ô tô con (car) màu trắng đỗ ở làn đường phía trước góc dưới bên trái.
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Tôi dùng công cụ Polygon vẽ sát mép cản trước, viền lốp xe chạm mặt đường và mép gương chiếu hậu. Ranh giới dừng chính xác tại mép thân xe nhìn thấy được, không vẽ tràn sang bóng đổ trên mặt đường; phần lốp sau bị xe máy phía trước che khuất một góc nhỏ được dừng tại mép chắn bùn của xe máy theo đúng quy tắc Visibility Boundary.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Mô hình gợi ý tự động sau đó có xu hướng nhận diện cả phần bóng đổ dưới gầm xe thành thân xe và bị lẹm viền vào nan hoa xe máy. Tôi đã chủ động chỉnh sửa lại các điểm nút (vertices) của Polygon, co ngắn biên để loại bỏ vùng bóng đen mặt đường, giữ mask bám khít hình học vật lý của xe.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình: Đã đối chiếu và giữ mask tự vẽ tay bằng Polygon chuẩn xác từng điểm nút.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task cp1_holes, ảnh 000000144300.jpg, vùng kính chắn gió và cửa sổ của xe bus/xe tải lớn.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: biên / khoét lỗ tùy tiện (Holes / Pruning error)
- Bằng chứng tôi nhìn thấy: Khi vẽ ban đầu, do nhìn thấy ánh sáng xuyên qua kính cabin lộ ghế lái, tôi đã khoét rỗng mảng kính khiến thân xe bus bị thủng một lỗ lớn ở giữa đầu xe.
- Quy tắc và hành động sửa: Đối chiếu quy tắc task trong manifest.json: "Holes: windows/gaps stay inside the mask — do NOT cut them out". Kính xe là bộ phận kết cấu thân vỏ của phương tiện. Tôi đã tô phủ kín lại toàn bộ diện tích mặt kính chắn gió thành một khối mask bus liền mạch.
- Sau sửa đã Save và export lại chưa? Đã bấm Save trên CVAT, kiểm tra lại danh sách Objects và cập nhật file cp1_holes.zip trong submissions/.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Đã chạy inspect_submissions.py xác nhận [OK] 9 annotations hợp lệ; chưa có điểm ground truth chính thức. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. cp4_curb / 7d83710e-4697c3b2.jpg (Đoạn mép đường bên phải) | Cách 1: Xem toàn bộ mặt phẳng màu xám đen là road vì cùng chất liệu nhựa đường.<br>Cách 2: Tách dải cao hơn có rãnh thoát nước thành sidewalk. | Dựa trên cao độ gờ bó vỉa và vạch kẻ phân làn xe chạy. Sidewalk là phần hạ tầng dành cho người đi bộ dù có cùng vật liệu trải thảm. | Quyết định chọn Cách 2: Dùng Polygon vẽ theo đường chỉ mép bó vỉa, gán phần trong là road và dải sát tường nhà là sidewalk. |
| 2. cp5_occlusion / 000000336232.jpg (Chiếc xe hơi bị cột biển báo che ngang) | Cách 1: Tách thành 2 object car độc lập vì 2 mảng pixel không dính nhau.<br>Cách 2: Giữ là 1 object car duy nhất gồm 2 mảng nhìn thấy. | Quy tắc bài lab: Một vật thể vật lý bị che cắt đoạn vẫn có cùng định danh instance. | Quyết định chọn Cách 2: Gán cả 2 đa giác đầu xe và đuôi xe vào cùng 1 instance car, không vẽ xuyên qua cột biển báo. |
| 3. cp3_thin / 839f7736-abe28069.jpg (Đỉnh chóp đèn tín hiệu và cột mảnh) | Cách 1: Bỏ qua chóp đèn và cột siêu mảnh (< 2px) coi như nhiễu nền sky.<br>Cách 2: Phóng to vẽ ôm khít để giữ class pole / traffic sign. | Cột và biển báo mảnh nếu bỏ sót sẽ làm tụt recall/mIoU của class hiếm. | Quyết định chọn Cách 2: Phóng to 600%, vẽ đường thẳng ôm trọn thân cột và biển báo, không để nét cọ phình to lan sang bầu trời. |
