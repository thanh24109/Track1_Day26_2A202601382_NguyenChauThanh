# Operating Dashboard — TalentScreenAI

> Đây là **worksheet nguồn** để validator và rubric truy vết evidence. Sau khi
> hoàn tất, rút gọn phần vận hành sang `one-page-dashboard.md`; không ép bảng
> này lên một trang.

- Học viên: Nguyễn Châu Thành
- Mã học viên: 2A202601382
- Mô hình: B2B
- Cập nhật: 2026-08-28
- North Star: Median time-to-first-value ≤ 10 ngày (từ connect ATS đến khi recruiter chấp nhận shortlist đầu tiên từ ≥ 50 CV đã sàng lọc)

## Chẩn đoán mô hình

TalentScreenAI là B2B: doanh nghiệp SME tuyển dụng in-house (ngành tech, 10–20 vị trí song song) trả tiền theo số CV được sàng lọc xong, recruiter của chính doanh nghiệp đó là người vận hành sản phẩm hằng ngày, và chúng tôi không cần một quan hệ sản phẩm độc lập với ứng viên để tạo ra giá trị cốt lõi. Vì thế đèn bật trước là time-to-first-value chứ không phải đường cong retention của người dùng cuối.

| Dữ liệu đầu vào | Trạng thái | Nằm ở đâu hoặc cần gì để đo | Ngày có số |
|---|---|---|---|
| Unit economics Day 24 (ARPU, gross margin, CAC, payback) | Đo được | File `2A202601382_NguyenChauThanh_Day24.xlsx`, tab `2. Unit Economics` cột Base | 2026-08-28 |
| Value Metric và Cost/Job Day 25 | Đo được | File `NguyenChauThanh_Day25_model.xlsx`, tab `1_Cost_Job` và `2_Pricing` | 2026-08-28 |

## Kiểm kê đèn ứng viên

Mở bảng B2B trong handbook, copy 11 đèn và đánh đúng một trạng thái ✅/🔧/❌.

| Đèn ứng viên từ handbook | Tầng | Trạng thái | Bằng chứng hiện có hoặc kế hoạch đo |
|---|---|---|---|
| Time-to-first-value (TTFV) | L | ✅ | Event connect-ATS và event "recruiter accept shortlist" đã có trong pilot log |
| Pipeline coverage | L | ❌ | Đi PLG self-serve trong 90 ngày đầu, chưa dựng pipeline sales để đo |
| % deal chết ở khâu security/procurement | L | 🔧 | Thêm trường bắt buộc "lý do dừng" vào form DPA; audit trước 2026-10-15 |
| POC → paid | O | 🔧 | Chuẩn hóa mốc "team trả tiền lần đầu" trong billing trước 2026-09-30 |
| Sales cycle (ngày) | O | ❌ | PLG self-serve, không có chu kỳ sales để đo trong giai đoạn này |
| Usage depth trong tài khoản | O | ✅ | Weekly active screening theo account trong event log |
| Chi phí triển khai ÷ ACV | O | 🔧 | Gắn giờ onboarding của founder với account ID trước 2026-10-15 |
| Tập trung doanh thu | O | 🔧 | Billing export theo account đã redacted; chốt công thức top-1/top-5 trước 2026-09-30 |
| NRR | G | 🔧 | Cần đủ hai cohort quý; có số vào 2027-03-31 |
| Gross Margin | G | ✅ | Billing export ghép token, infra và giờ QA nội bộ |
| CAC payback | G | 🔧 | Chuẩn hóa fully-loaded CAC cho kênh PLG trước 2026-10-31 |

## Đèn báo sớm

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| L-01 | Time-to-first-value | Số ngày từ khi team connect ATS đến khi recruiter chấp nhận shortlist đầu tiên dựng từ ≥ 50 CV đã sàng lọc; median theo cohort tuần | Tuần · Product Ops | 14 ngày | ≤ 10 ngày | 11–18 ngày | trên 18 ngày | [TB] Dùng hai cohort đầu làm chuẩn tạm; chốt baseline sau bốn cohort vào 2026-10-31 | 2026-08-28 | Activation và retention tháng | R-01 |
| L-02 | Recruiter agreement rate | Số CV mà recruiter giữ nguyên kết luận Pass/Reject/Review của AI chia tổng CV trong mẫu có nhãn | Tuần · Product Ops | 78% | ≥ 85% | 78–84% | dưới 78% | [TB] Prototype 800 CV agreement 78%; mở rộng lên 8.000 CV có nhãn rồi chốt baseline vào 2026-09-30 | 2026-08-28 | Time-to-first-value và churn | R-02 |

## Đèn vận hành

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| O-01 | Weekly containment rate | Số CV AI sàng lọc xong không cần recruiter làm lại chia số CV đưa vào xử lý trong tuần | Tuần · Product Ops | 82% | ≥ 80% | 65–79% | dưới 65% | [MH] MH-02 breakeven containment là 52%; xanh khi giữ đệm ≥ 28 điểm phần trăm trên mức đó | 2026-08-28 | Gross margin và retention | R-03 |
| O-02 | Chi phí AI trên mỗi CV sàng lọc xong | Tổng token, inference, infra và retry chia số CV đạt QA trong tuần | Tuần · FinOps | 1.990 đ | ≤ 2.400 đ | 2.401–4.000 đ | trên 4.000 đ | [MH] MH-01 suy trần AI cost từ giá bán và gross margin mục tiêu | 2026-08-28 | Gross margin | R-04 |
| O-03 | PLG activation rate | Số team connect ATS và sàng lọc ≥ 50 CV thật trong 14 ngày chia tổng team đăng ký | Tuần · Growth | 45% | ≥ 60% | 40–59% | dưới 40% | [TB] Chưa đủ cohort; đo bốn cohort đăng ký rồi chốt baseline vào 2026-10-31 | 2026-08-28 | POC-to-paid conversion | R-03 |

## Đèn kết quả

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| G-01 | Gross margin sau AI cost | Doanh thu trừ toàn bộ chi phí biến đổi (AI, infra, retry, QA nội bộ) chia doanh thu | Tháng · Finance | 58% | ≥ 60% | 53–59% | dưới 53% | [MH] MH-01 đặt trần AI cost để gross margin không xuống dưới mục tiêu | 2026-08-28 | Runway và CAC payback | R-04 |
| G-02 | Net revenue retention | MRR cuối kỳ của cohort team trả tiền chia MRR đầu kỳ sau expansion và churn | Quý · Finance | chưa đủ 2 cohort quý; có số 2027-03-31 | ≥ 110% | 95–109% | ≤ 94% | [TB] Chưa đủ lịch sử; đo cùng cohort trong hai quý rồi chốt baseline vào 2027-03-31 | 2026-08-28 | LTV và mở rộng ngách agency | R-05 |

## Luật quyết định

| ID | NẾU | TRONG | VÀ | THÌ | KHÔNG THÌ | Luật dừng? |
|---|---|---|---|---|---|---|
| R-01 | Median TTFV trên 18 ngày | 2 cohort liên tiếp | Mỗi cohort có ít nhất 8 team activated | Dừng nhận đăng ký mới trong 14 ngày và cắt onboarding còn đúng một luồng connect-ATS đến 50 CV | Không giảm giá hay tặng credit để bù thời gian chờ | CÓ |
| R-02 | Recruiter agreement dưới 78% | 3 tuần liên tiếp | Mẫu có ít nhất 500 CV có nhãn mỗi tuần | Đóng băng mở rộng ngành mới và tái huấn luyện rubric trên 2.000 CV bị recruiter lật gần nhất trong một sprint | Không nới định nghĩa "đúng" để chỉ số đẹp lên | KHÔNG |
| R-03 | PLG activation dưới 40% hoặc weekly containment dưới 65% | 2 cohort hoặc 2 tuần liên tiếp | Có ít nhất 10 team go-live | Đóng băng chi outbound và sửa checklist activation trong một sprint | Không tăng số đăng ký để kéo trung bình tỷ lệ lên | CÓ |
| R-04 | Chi phí AI mỗi CV trên 4.000 đ hoặc gross margin dưới 53% | 2 tuần hoặc 1 tháng liên tiếp | Có ít nhất 5.000 CV đạt QA | Giới hạn context còn 6.000 token cache và 2.000 token fresh, hạ model tier cho CV đơn giản, bật Batch API và đàm phán lại quota trước kỳ billing | Không bỏ bước Human QA để chi phí trông thấp hơn | KHÔNG |
| R-05 | NRR dưới 95% | 2 quý liên tiếp | Cohort có ít nhất 12 team trả tiền | Dừng nhận ngách agency mới trong một quý và chuyển toàn bộ roadmap sang ba nguyên nhân churn lớn nhất có evidence | Không tính pipeline mới hay user free vào NRR | CÓ |

## Cổng gác 90 ngày

| Ngày | Metric gác cổng | Ngưỡng | Bằng chứng vật lý | Nếu đạt | Nếu trượt |
|---:|---|---|---|---|---|
| 30 | Recruiter agreement rate trên tập CV có nhãn | ≥ 80% trên ít nhất 2.000 CV | Eval report Day 21–22 mở rộng kèm label sheet và confusion matrix | GO | FIX |
| 60 | PLG activation rate | ≥ 45% trên ít nhất 10 team go-live | Cohort report xuất từ event log | GO | PIVOT |
| 90 | Gross margin sau AI cost | ≥ 55% trên ít nhất 20.000 CV đã sàng lọc | Billing export ghép token và QA report | GO | KILL |

## Kill criteria

KILL hướng sản phẩm vào ngày 90 nếu gross margin sau AI cost vẫn dưới 55% sau hai vòng tối ưu model và không có team nào chấp nhận mức giá sàn 0,23 USD mỗi CV, hoặc recruiter agreement không vượt 80% trên 20.000 CV có nhãn.

## Chưa đo được

| Đèn hoặc giả định | Cần gì để đo | Ai chịu trách nhiệm | Ngày có số |
|---|---|---|---|
| Giờ recruiter tiết kiệm mỗi tuần nhờ AI | Instrument time-to-shortlist trước và sau trong pilot 6 tuần với 3 design partner | Founder | 2026-10-15 |
| ARPU thực tế mỗi team khi đóng gói theo vị trí thay vì theo CV | Đối chiếu hóa đơn của 5 design partner sau pilot | Founder và FinOps | 2026-10-31 |

## Phụ lục ngưỡng suy từ mô hình

| ID | Metric | Input Day 24–25 | Phép tính | Kết quả và ngưỡng áp dụng |
|---|---|---|---|---|
| MH-01 | Trần chi phí AI mỗi CV | Giá bán 0,30 USD/CV; gross margin mục tiêu 60%; chi phí biến đổi khác (QA nội bộ, infra phân bổ) 0,033 USD/CV; tỷ giá 26.000 đ/USD | 0,30 × (1 − 0,60) − 0,033 = 0,087 USD ≈ 2.260 đ | Xanh khi AI cost ≤ 2.400 đ/CV; đỏ trên 4.000 đ sau buffer — áp cho O-02 và G-01 |
| MH-02 | Containment tối thiểu để hòa vốn | v (chi phí biến đổi mỗi CV thử) 0,041 USD; q (QA nội bộ mỗi CV thử) 0,021 USD; e (chi phí một ca escalate, biến thể A) 0 USD; giá 0,30 USD; gross margin mục tiêu 60% | R ≥ (0,041 + 0,021 + 0) ÷ (0,30 × (1 − 0,60) + 0) = 0,062 ÷ 0,12 = 0,52 | Breakeven containment 52%; O-01 xanh khi ≥ 80%, đỏ dưới 65% |

## Ghi nhận AI critique

| Phản biện | Chấp nhận hay bác bỏ | Thay đổi đã thực hiện | Lý do |
|---|---|---|---|
| TTFV thiếu định nghĩa "giá trị đầu tiên" đo được | Chấp nhận | Chốt mốc recruiter chấp nhận shortlist đầu tiên từ ≥ 50 CV | Hai người đo cùng một cách, không tranh cãi |
| Nên lấy benchmark NRR 110% làm ngưỡng đỏ ngay | Bác bỏ | Giữ baseline theo cohort của chính sản phẩm | Chưa đủ dữ liệu cùng phân khúc để coi benchmark là ngưỡng hành động |
| ARPU Day 24 (159k đ) và Day 25 (0,30 USD/CV × ~2.000 CV) chưa khớp | Chấp nhận | Đưa việc đối chiếu ARPU vào mục "Chưa đo được" | Cần hóa đơn thật của design partner mới chốt được đơn vị đóng gói |
