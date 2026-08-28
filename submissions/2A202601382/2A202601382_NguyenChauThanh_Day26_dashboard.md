<!-- TRANG 1 -->

# Operating Dashboard — TalentScreenAI

**Model:** B2B · **Cập nhật:** 2026-08-28 · **Owner phiên họp:** Product Operations

**Chẩn đoán:** Doanh nghiệp SME tuyển dụng in-house trả tiền theo CV sàng lọc;
recruiter của doanh nghiệp vận hành; TalentScreenAI không có quan hệ sản phẩm
độc lập với ứng viên.

**North Star:** Median time-to-first-value · hiện tại 14 ngày · mục tiêu ≤ 10 ngày · 🟡

## Cây đèn 3 tầng

| Tầng · ID | Metric và định nghĩa ngắn | Hiện tại · 🟢 / 🟡 / 🔴 · Nguồn | Nhịp · Owner | Báo trước cho · Luật |
|---|---|---|---|---|
| L · L-01 | Ngày connect ATS → recruiter nhận shortlist đầu từ ≥ 50 CV | 14d · ≤10 / 11–18 / >18 · `[TB]` | Tuần · Product Ops | Activation + retention · R-01 |
| L · L-02 | CV recruiter giữ nguyên kết luận AI ÷ CV trong mẫu có nhãn | 78% · ≥85 / 78–84 / <78 · `[TB]` | Tuần · Product Ops | TTFV + churn · R-02 |
| O · O-01 | CV AI xong không cần làm lại ÷ CV xử lý trong tuần | 82% · ≥80 / 65–79 / <65 · `[MH]` | Tuần · Product Ops | GM + retention · R-03 |
| O · O-02 | Token + inference + infra + retry ÷ CV đạt QA | 1.990đ · ≤2,4k / 2,4–4k / >4k · `[MH]` | Tuần · FinOps | GM · R-04 |
| O · O-03 | Team connect ATS + sàng lọc ≥ 50 CV/14d ÷ team đăng ký | 45% · ≥60 / 40–59 / <40 · `[TB]` | Tuần · Growth | POC→paid · R-03 |
| G · G-01 | (Doanh thu − chi phí biến đổi gồm AI, infra, retry, QA) ÷ doanh thu | 58% · ≥60 / 53–59 / <53 · `[MH]` | Tháng · Finance | Runway + payback · R-04 |
| G · G-02 | MRR cuối kỳ cohort ÷ MRR đầu kỳ sau expansion và churn | 2 cohort quý xong 2027-03-31 · ≥110 / 95–109 / ≤94 · `[TB]` | Quý · Finance | LTV · R-05 |

## Luật quyết định

| ID | NẾU · TRONG · VÀ | THÌ | KHÔNG THÌ | Dừng? |
|---|---|---|---|---|
| R-01 | TTFV >18d · 2 cohort · ≥8 team/cohort | Dừng đăng ký mới 14d; cắt onboarding còn 1 luồng connect-ATS→50 CV | Không giảm giá / tặng credit để bù chờ | CÓ |
| R-02 | Agreement <78% · 3 tuần · ≥500 CV nhãn/tuần | Đóng băng mở ngành mới; retrain rubric trên 2.000 CV bị lật | Không nới định nghĩa "đúng" | KHÔNG |
| R-03 | Activation <40% hoặc containment <65% · 2 cohort/2 tuần · ≥10 team go-live | Đóng băng outbound; sửa checklist activation 1 sprint | Không tăng đăng ký để kéo trung bình | CÓ |
| R-04 | AI cost >4.000đ hoặc GM <53% · 2 tuần/1 tháng · ≥5.000 CV QA | Giới hạn context, hạ model tier, bật Batch API, đàm phán quota | Không bỏ Human QA để làm đẹp cost | KHÔNG |
| R-05 | NRR <95% · 2 quý · ≥12 team trả tiền | Dừng nhận agency mới 1 quý; roadmap sang 3 churn cause có evidence | Không tính pipeline / user free vào NRR | CÓ |

## Cổng 90 ngày

| Ngày | Một metric · ngưỡng | Evidence | Đạt / Trượt |
|---:|---|---|---|
| 30 | Recruiter agreement · ≥ 80% trên ≥ 2.000 CV nhãn | Eval report Day 21–22 mở rộng + confusion matrix | GO / FIX |
| 60 | PLG activation · ≥ 45% trên ≥ 10 team go-live | Cohort report từ event log | GO / PIVOT |
| 90 | GM sau AI cost · ≥ 55% trên ≥ 20.000 CV | Billing export + QA report | GO / KILL |

**Kill criteria:** KILL ngày 90 nếu GM sau AI cost <55% sau hai vòng tối ưu model
và không team nào chấp nhận giá sàn 0,23 USD/CV, hoặc agreement không vượt 80%
trên 20.000 CV nhãn.

**Chưa đo được:** Giờ recruiter tiết kiệm mỗi tuần · cần instrument time-to-shortlist
trước/sau trong pilot 3 design partner · owner Founder · có số ngày 2026-10-15.

<div style="page-break-after: always;"></div>

<!-- TRANG 2 — PHỤ LỤC -->

# Phụ lục — TalentScreenAI Day 26

## Hai ngưỡng suy từ mô hình `[MH]`

| ID | Metric | Input Day 24–25 | Phép tính | Kết quả và ngưỡng áp dụng |
|---|---|---|---|---|
| MH-01 | Trần chi phí AI mỗi CV | Giá bán 0,30 USD/CV (Day 25 tab `2_Pricing` B19); gross margin mục tiêu 60% (Day 25 B32); chi phí biến đổi khác 0,033 USD/CV (QA nội bộ + infra phân bổ, Day 25 tab `1_Cost_Job`); tỷ giá 26.000 đ/USD (Day 25 B68) | 0,30 × (1 − 0,60) − 0,033 = 0,087 USD ≈ 2.260 đ | Xanh khi AI cost ≤ 2.400 đ/CV; đỏ trên 4.000 đ sau buffer — áp cho O-02 và G-01 |
| MH-02 | Containment tối thiểu để hòa vốn | v = 0,041 USD/CV thử (API sau Batch + infra + retry, Day 25 tab `1_Cost_Job`); q = 0,021 USD/CV thử (QA nội bộ 8% × 2 phút × 8 USD/giờ); e = 0 USD (biến thể A, khách tự chịu escalate); giá 0,30 USD; GM mục tiêu 60% | R ≥ (0,041 + 0,021 + 0) ÷ (0,30 × (1 − 0,60) + 0) = 0,062 ÷ 0,12 = 0,52 | Breakeven containment 52%; O-01 xanh khi ≥ 80%, đỏ dưới 65% |

## Chi tiết nguồn từng đèn

| ID | Loại nguồn | Chi tiết và lý do dùng |
|---|---|---|
| L-01 | `[TB]` | Chưa có baseline TTFV. Dùng hai cohort đăng ký đầu làm chuẩn tạm, đo bằng cùng cặp event (connect ATS → recruiter accept shortlist ≥ 50 CV), chốt baseline sau bốn cohort — dự kiến 2026-10-31. |
| L-02 | `[TB]` | Prototype eval Day 21–22 trên 800 CV có nhãn cho agreement 78%, false-reject < 3%. Mở rộng lên 8.000 CV có nhãn (pilot 3 design partner) rồi chốt baseline — dự kiến 2026-09-30. |
| O-01 | `[MH]` MH-02 | Ngưỡng xanh 80% đặt đệm ≥ 28 điểm phần trăm trên breakeven containment 52% để chịu được dao động chất lượng CV đầu vào. |
| O-02 | `[MH]` MH-01 | Trần 2.400 đ/CV suy trực tiếp từ giá bán 0,30 USD và gross margin mục tiêu 60%; đỏ 4.000 đ là mức mà GM rơi xuống dưới benchmark AI-native ≈ 53% (ICONIQ 2026). |
| O-03 | `[TB]` | Chưa đủ cohort PLG. Đo bốn cohort đăng ký bằng cùng định nghĩa activation rồi chốt baseline — dự kiến 2026-10-31. |
| G-01 | `[MH]` MH-01 | Ngưỡng đỏ 53% khớp gross margin AI-native trung vị 2026; xanh 60% là mục tiêu nội bộ để CAC payback PLG < 6 tháng. |
| G-02 | `[TB]` | Chưa đủ hai cohort quý. Đo cùng cohort team trả tiền trong hai quý rồi chốt baseline — dự kiến 2027-03-31. |

## Ghi nhận AI critique

| Phản biện | Chấp nhận / Bác bỏ | Thay đổi | Lý do |
|---|---|---|---|
| TTFV thiếu định nghĩa "giá trị đầu tiên" đo được | Chấp nhận | Chốt mốc recruiter chấp nhận shortlist đầu từ ≥ 50 CV | Hai người đo cùng một cách |
| Lấy benchmark NRR 110% làm ngưỡng đỏ ngay | Bác bỏ | Giữ baseline theo cohort của chính sản phẩm | Chưa đủ dữ liệu cùng phân khúc |
| ARPU Day 24 và Day 25 chưa khớp đơn vị đóng gói | Chấp nhận | Đưa vào mục "Chưa đo được", chờ hóa đơn design partner | Cần số thật mới chốt |
