# OPERATING DASHBOARD — AI Ghi chép Hội thoại Khám bệnh

**Loại mô hình:** B2B · **Cập nhật:** 09/10/2026 · **Nguyen Anh Dung – 2A202602554**

> **Sản phẩm:** AI ghi nhận hội thoại khám bệnh, tạo bản nháp hồ sơ có cấu trúc, để bác sĩ duyệt trước khi import vào EHR. Bản chưa được bác sĩ duyệt không được ghi vào hồ sơ chính thức.

## NORTH STAR

**Time-to-first-value (TTFV):** chưa có dữ liệu — **mục tiêu <30 ngày/khách pilot**

Số ngày từ lúc bắt đầu pilot đến khi có note khám đầu tiên được bác sĩ duyệt và import thành công vào EHR.

## Đèn báo sớm — Leading

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---:|---|---|---|
| TTFV | Chưa đo | 🟢 <30 ngày · 🟡 30–60 · 🔴 >60 | [TB] | POC → paid, usage depth |
| Tỷ lệ note được bác sĩ duyệt để import | Chưa đo | 🟢 ≥85% · 🟡 70%–<85% · 🔴 <70% | [TB] | Usage depth, POC → paid |

## Đèn vận hành — Operating

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---:|---|---|---|
| Usage depth trong tài khoản | Chưa đo | 🟢 ≥60% · 🟡 30%–<60% · 🔴 <30% | [TB] | Gia hạn, mở rộng hợp đồng |
| POC → paid | Chưa có POC | 🟢 ≥50% · 🟡 35%–<50% · 🔴 <35% | [TB] | Doanh thu, NRR |
| Chi phí triển khai ÷ ACV | Chưa đo | 🟢 <15% · 🟡 15–30% · 🔴 >30% | [MH] | Gross Margin, khả năng scale |
| AI cost per validated encounter | Chưa đo | 🟢 ≤5.400đ · 🟡 5.401–9.000đ · 🔴 >9.000đ | [MH] | Gross Margin |

## Đèn kết quả — Lagging

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---:|---|---|
| Gross Margin | Chưa có doanh thu | 🟢 ≥55% · 🟡 45%–<55% · 🔴 <45% | [TB] |
| CAC payback | Chưa đo | 🟢 <12 tháng · 🟡 12–18 · 🔴 >18 | [TB] |

## 5 luật quyết định (⏹ = luật dừng)

1. **NẾU** TTFV >60 ngày **TRÊN** 3 khách pilot gần nhất **VÀ** mỗi khách đã có ít nhất 10 ngày sử dụng thực tế **THÌ** cắt pilot xuống một chuyên khoa và một workflow import SOAP note **KHÔNG THÌ** không tuyển thêm sales để ký pilot rộng hơn. ⏹

2. **NẾU** tỷ lệ note được bác sĩ duyệt để import <70% **TRONG** 2 tuần liên tiếp **VÀ** có ít nhất 100 note đã được bác sĩ xem **THÌ** đóng băng tính năng mới, phân tích 20 note bị từ chối và sửa nhóm lỗi đứng đầu trong 2 tuần **KHÔNG THÌ** không triển khai rộng. ⏹

3. **NẾU** chi phí triển khai >30% ACV **TRÊN** 3 khách liên tiếp **THÌ** chuẩn hóa cấu hình, connector EHR và onboarding, hoặc tăng giá triển khai **KHÔNG THÌ** không nhận khách mới với cùng phạm vi tùy biến.

4. **NẾU** usage depth <30% **SAU** 60 ngày go-live **VÀ** tài khoản có ít nhất 10 bác sĩ được cấp quyền **THÌ** đào tạo lại theo một workflow và đặt mục tiêu 60% trong 30 ngày **KHÔNG THÌ** không bán thêm module.

5. **NẾU** AI cost per validated encounter >9.000đ **TRONG** 2 kỳ tính phí liên tiếp **VÀ** gross margin dự phóng <45% **THÌ** chuyển workflow nặng sang model/routing tiết kiệm hơn và giới hạn lượt dùng chưa tạo giá trị **KHÔNG THÌ** không tăng lượt khám miễn phí để theo đuổi tăng trưởng danh nghĩa. ⏹

## Cổng gác 90 ngày

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 (08/11/2026) | Số encounter được đánh giá lỗi | ≥100 encounter được bác sĩ gán loại lỗi và xác định 3 nhóm lỗi lớn nhất | eval.csv, báo cáo lỗi ẩn danh | GO nếu đạt; FIX 30 ngày bổ sung mẫu |
| 60 (08/12/2026) | Số khách đạt TTFV <30 ngày | ≥2 khách; khách chưa đạt vẫn nằm trong mẫu | pilot-start log, approval log, EHR receipt | GO nếu đạt; FIX onboarding một lần |
| 90 (07/01/2027) | POC → paid | ≥35%, mẫu ≥3 pilot kết thúc | danh sách pilot, hợp đồng trả phí | GO nếu đạt; PIVOT nếu giả định sai; KILL nếu đã FIX cùng vấn đề mà không cải thiện |

**KILL CRITERIA:** Ngày 07/01/2027, nếu có ≥3 pilot kết thúc và đã FIX chuyển đổi một lần nhưng vẫn 0 hợp đồng trả phí, dừng hướng triển khai hiện tại. Thiếu mẫu: PIVOT kênh/phân khúc, không tuyên bố đã qua cổng.

**CHƯA ĐO ĐƯỢC:** TTFV, tỷ lệ note được duyệt, usage depth, chi phí triển khai/ACV, AI cost/encounter, Gross Margin và CAC payback. Cần event tracking theo từng khách, log EHR import, audit log bác sĩ duyệt/từ chối, timesheet triển khai, hóa đơn speech-to-text/LLM/hạ tầng, CRM và dữ liệu kế toán. Dự kiến có baseline đầu tiên sau 2 tuần pilot: 23/10/2026.

**NGUỒN/LÝ DO:** [TB] là ngưỡng nội bộ dự kiến, chưa đo; [MH] là mô hình minh họa chưa xác nhận. Lý do từng đèn và phép tính ở worksheet. Chưa dùng benchmark đã xác minh. Baseline sau 2 tuần pilot thật; ngày 23/10 chỉ là mốc dự kiến thiết lập tracking.
