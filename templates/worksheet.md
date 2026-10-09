# Worksheet — AI Ghi chép Hội thoại Khám bệnh

Họ tên: Nguyen Anh Dung · MSSV: 2A202602554 · Ngày làm: 09/10/2026

> **Phạm vi sản phẩm:** AI ghi nhận hội thoại khám bệnh giữa bác sĩ và bệnh nhân, tạo bản nháp hồ sơ khám bệnh có cấu trúc, cho phép bác sĩ chỉnh sửa và duyệt, sau đó import bản đã duyệt vào EHR. AI không tự động ghi dữ liệu chưa được bác sĩ duyệt vào hồ sơ chính thức.

## Trạm 1 — Loại mô hình

**Câu chốt loại:** Chúng tôi là **B2B** vì bệnh viện/phòng khám là bên trả tiền, còn bác sĩ và nhân viên y tế của chính tổ chức đó là người sử dụng trực tiếp; sản phẩm được triển khai vào quy trình khám và hệ thống EHR của tổ chức.

**Bảng đèn B2B §3**

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| Time-to-first-value (TTFV) | 🔧 | Log từ ngày bắt đầu pilot đến khi có bản note đầu tiên được bác sĩ duyệt và import vào EHR; cần event tracking. |
| Pipeline coverage | 🔧 | CRM và giá trị cơ hội đủ điều kiện; hiện chưa có pipeline chuẩn hóa. |
| % deal chết ở security/procurement | 🔧 | CRM với lý do mất deal; cần thống nhất taxonomy lý do. |
| POC → paid | 🔧 | Hợp đồng pilot và hợp đồng trả tiền; cần mã hóa trạng thái khách hàng. |
| Sales cycle | 🔧 | CRM: từ cơ hội đủ điều kiện đến ngày ký hợp đồng. |
| Usage depth trong tài khoản | 🔧 | Log số bác sĩ/workflow có hoạt động hằng tuần chia cho số đã mua; cần định nghĩa active rõ ràng. |
| Chi phí triển khai ÷ ACV | 🔧 | Timesheet triển khai + hóa đơn hạ tầng/tích hợp chia cho ACV; cần mã dự án theo từng khách hàng. |
| Tập trung doanh thu | 🔧 | Hệ thống kế toán/CRM; đo doanh thu theo từng khách hàng mỗi tháng. |
| NRR | ❌ | Chưa có khách hàng trả phí đủ lâu để đo mở rộng, thu hẹp và gia hạn. |
| Gross Margin | 🔧 | Doanh thu trừ chi phí speech-to-text, LLM, lưu trữ, hạ tầng và hỗ trợ; cần phân bổ chi phí theo khách hàng. |
| CAC payback | 🔧 | Chi phí sales/marketing chia cho lãi gộp tăng thêm; cần số ACV, GM và chi phí bán hàng thực tế. |

## Trạm 2 — Thẻ đèn

**North Star:** **Time-to-first-value (TTFV)** — hiện tại: chưa có dữ liệu — mục tiêu: dưới 30 ngày cho mỗi khách hàng pilot.

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | L | **TTFV** | Số ngày từ ngày bắt đầu pilot đến khi có note khám đầu tiên được bác sĩ duyệt và import thành công vào EHR. Không tính cài đặt xong nhưng chưa có note được dùng thật. | Ngày import note đầu tiên − ngày bắt đầu pilot | Mỗi khách · Product/Ops | POC → paid và usage depth |
| 2 | L | **Tỷ lệ note được bác sĩ duyệt để import** | Tỷ lệ note AI tạo ra được bác sĩ duyệt để import vào EHR. Đếm một bản cuối/encounter có bác sĩ xem trong tuần; loại test, bản tạo lại và bản chưa xem; note bị từ chối vẫn nằm trong mẫu số. | Số note được duyệt và import ÷ số encounter có note đã được bác sĩ xem | Hằng tuần · Product/Ops | Usage depth và POC → paid |
| 3 | O | **POC → paid** | Tỷ lệ pilot có ít nhất một workflow được nghiệm thu chuyển thành hợp đồng trả tiền. Không tính gia hạn miễn phí hoặc lời hứa mua hàng chưa ký. | Số POC chuyển thành hợp đồng trả tiền ÷ số POC kết thúc | Hằng quý · Sales/Finance | Doanh thu và NRR |
| 4 | O | **Usage depth trong tài khoản** | Tỷ lệ bác sĩ đã được cấp quyền và có ít nhất một note được duyệt/import trong tuần. Không tính tài khoản admin hoặc lượt demo. | Số bác sĩ active hằng tuần ÷ số bác sĩ được cấp quyền | Hằng tuần · Customer Success | Gia hạn và mở rộng hợp đồng |
| 5 | O | **Chi phí triển khai ÷ ACV** | Tổng công sức tích hợp, cấu hình, đào tạo và chi phí kỹ thuật cần cho một khách trong 90 ngày đầu. Không tính chi phí R&D dùng chung cho mọi khách. | Chi phí triển khai khách hàng ÷ ACV | Mỗi khách · Finance/Ops | Gross Margin và khả năng scale |
| 6 | O | **AI cost per validated encounter** | Chi phí speech-to-text, LLM, lưu trữ và xử lý cho một lượt khám có note được bác sĩ duyệt. Không tính lượt test nội bộ hoặc lượt bị hủy trước xử lý. | Toàn bộ chi phí AI production trong kỳ, gồm note bị từ chối và lần tạo lại ÷ số encounter có note được duyệt | Hằng tuần · Engineering/Finance | Gross Margin |
| 7 | G | **Gross Margin** | Phần doanh thu còn lại sau toàn bộ chi phí biến đổi để cung cấp dịch vụ, gồm AI, hạ tầng, hỗ trợ trực tiếp và phí tích hợp theo lượt dùng. | (Doanh thu − COGS) ÷ doanh thu | Hằng tháng · Finance | Khả năng tái đầu tư và CAC payback |
| 8 | G | **CAC payback** | Số tháng cần để lãi gộp từ khách hàng bù chi phí bán hàng và triển khai để có khách đó. | CAC ÷ (ACV × Gross Margin ÷ 12) | Hằng quý · Finance/Sales | Runway và quyết định tăng trưởng |

**Đèn chi phí AI:** **AI cost per validated encounter** — theo dõi chi phí cho một lượt khám tạo ra note được bác sĩ duyệt, vì đây là đơn vị giá trị và đơn vị chi phí thực tế của sản phẩm.

## Trạm 3 — Ngưỡng

> Bản trước pilot: các ngưỡng [TB] là giả thuyết vận hành, chưa phải baseline đã đo. [MH] dùng mô hình minh họa chưa được chủ dự án xác nhận; không giả nhận là số Day 24–25 thực tế. Không dùng [TB] vì chưa xác minh nguồn gốc. Cần người học xác nhận đầu vào và viết lại quyết định bằng lời của mình trước nộp.

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn [TB]/[MH]/[TB] | Lý do · ngày kiểm tra nếu [TB] |
|---|---|---:|---:|---:|---|---|
| 1 | TTFV | <30 ngày | 30–60 ngày | >60 ngày | [TB] | Theo ngưỡng B2B trong Handbook; giá trị chỉ được tính khi bác sĩ duyệt và import note, không phải khi cài đặt xong. Baseline dự kiến: 23/10/2026. |
| 2 | Tỷ lệ note được duyệt để import | ≥85% | 70%–<85% | <70% | [TB] | Tỷ lệ thấp cho thấy bác sĩ chưa tin chất lượng hoặc workflow chưa phù hợp. Đo 2 tuần pilot đầu; baseline dự kiến: 23/10/2026. |
| 3 | POC → paid | ≥50% | 35%–<50% | <35% | [TB] | Giả thuyết: cần ít nhất 1/2 pilot mua; dưới 35% cần đổi cách chứng minh giá trị. Chưa có baseline; dự kiến ngày 90. |
| 4 | Usage depth trong tài khoản | ≥60% | 30%–<60% | <30% | [TB] | Bệnh viện chỉ nhận được giá trị khi nhiều bác sĩ dùng thật hằng tuần; baseline dự kiến: 23/10/2026. |
| 5 | Chi phí triển khai ÷ ACV | <15% | 15–30% | >30% | [MH] | Mức trên 30% cho thấy sản phẩm đang giống dịch vụ tùy biến và khó scale; cần tính lại theo ACV thực tế. |
| 6 | AI cost per validated encounter | ≤5.400đ | 5.401–9.000đ | >9.000đ | [MH] | Suy từ giả định giá bán, 55% gross margin và phân bổ tối đa 60% COGS cho AI; thay bằng Cost/Job thật sau khi chạy pilot. |
| 7 | Gross Margin | ≥55% | 45%–<55% | <45% | [TB] | Mục tiêu mô hình: GM 55%; 45% là mức cảnh báo nội bộ chưa kiểm chứng. Đối soát tháng đầu có doanh thu. |
| 8 | CAC payback | <12 tháng | 12–18 tháng | >18 tháng | [TB] | Chọn phân khúc phòng khám tư; mục tiêu thu hồi CAC trong 12 tháng, tối đa 18 tháng là giả thuyết cần xác nhận bằng runway. |

### Phụ lục [MH] — phép tính

**[MH] 1 — Chi phí triển khai ÷ ACV**

```text
Giả định ACV của một phòng khám/bệnh viện nhỏ = 240.000.000đ/năm
Mục tiêu chi phí triển khai tối đa = 15% ACV
Ngưỡng xanh = 240.000.000 × 15% = 36.000.000đ/khách
Ngưỡng đỏ theo luật B2B = 240.000.000 × 30% = 72.000.000đ/khách

Kết quả → 🟢 <36.000.000đ · 🟡 36.000.000–72.000.000đ · 🔴 >72.000.000đ
```

**[MH] 2 — AI cost per validated encounter**

```text
Giả định giá bán = 20.000.000đ/tháng
Giả định 1.000 lượt khám được validate/tháng
Doanh thu trên một lượt khám = 20.000.000 ÷ 1.000 = 20.000đ
Để dành tối thiểu 55% gross margin, tổng COGS tối đa = 45% × 20.000đ = 9.000đ/lượt

Phân bổ tối đa 60% COGS cho AI (speech-to-text + LLM + lưu trữ)
= 9.000 × 60% = 5.400đ/lượt.
Phần còn lại dành cho hạ tầng và hỗ trợ biến đổi:
🟢 ≤5.400đ · 🟡 5.401–9.000đ · 🔴 >9.000đ/lượt

Đây là ngưỡng mô hình tạm thời, phải thay bằng Cost/Job đo từ hóa đơn và log token thật.
```

## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng.

1. **NẾU** TTFV >60 ngày **TRÊN** 3 khách pilot gần nhất **VÀ** mỗi khách đã có ít nhất 10 ngày sử dụng thực tế **THÌ** cắt pilot xuống một chuyên khoa và một workflow import SOAP note, đồng thời khóa việc mở rộng phạm vi **KHÔNG THÌ** không tuyển thêm sales để ký thêm pilot rộng hơn. ⏹

2. **NẾU** tỷ lệ note được bác sĩ duyệt để import <70% **TRONG** 2 tuần liên tiếp **VÀ** có ít nhất 100 note đã được bác sĩ xem **THÌ** đóng băng việc thêm tính năng mới, phân tích 20 lỗi bị từ chối nhiều nhất và sửa nhóm lỗi đứng đầu trong 2 tuần **KHÔNG THÌ** không tuyên bố sản phẩm đã sẵn sàng triển khai rộng. ⏹

3. **NẾU** chi phí triển khai >30% ACV **TRÊN** 3 khách liên tiếp **THÌ** chuẩn hóa template cấu hình, connector EHR và tài liệu onboarding, hoặc tăng giá triển khai trước khi nhận khách mới **KHÔNG THÌ** không tiếp tục bán cùng mức giá với cùng phạm vi tùy biến.

4. **NẾU** usage depth <30% **SAU** 60 ngày kể từ go-live **VÀ** tài khoản có ít nhất 10 bác sĩ được cấp quyền **THÌ** cử Customer Success đào tạo lại theo một workflow cụ thể và đặt mục tiêu 60% trong 30 ngày **KHÔNG THÌ** không bán thêm module cho tài khoản chưa sử dụng sản phẩm cốt lõi.

5. **NẾU** AI cost per validated encounter >9.000đ **TRONG** 2 kỳ tính phí liên tiếp **VÀ** gross margin dự phóng <45% **THÌ** chuyển workflow nặng sang model/routing tiết kiệm hơn và giới hạn lượt dùng chưa tạo giá trị **KHÔNG THÌ** không tăng số lượt khám miễn phí để theo đuổi tăng trưởng danh nghĩa. ⏹

### Kế hoạch đo và liên kết luật

L1→luật 1; L2→luật 2; O4→luật 4; O5→luật 3; O6 và G7→luật 5. O3/G8 đỏ: dừng mở rộng bán hàng, phân tích pilot mất và cập nhật mô hình trong 2 tuần. TTFV chưa đạt phải báo số ngày đang chờ, không loại khách chưa có giá trị khỏi mẫu. Thời gian duyệt được log để kiểm tra lợi ích tiết kiệm nhập liệu; import thành công đơn lẻ chưa chứng minh ROI.

Chưa có lịch pilot xác nhận: 23/10/2026 chỉ là ngày dự kiến thiết lập tracking, baseline sau 2 tuần hoạt động thật. POC→paid sau khi có ≥3 pilot kết thúc; GM sau tháng có doanh thu đầu tiên; CAC payback sau khi đủ chi phí acquisition và doanh thu. NRR chưa có cohort gia hạn.
