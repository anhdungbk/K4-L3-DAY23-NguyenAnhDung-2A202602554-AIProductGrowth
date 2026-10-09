# K4-L3-DAY23-NguyenAnhDung-2A202602554-AIProductGrowth

**Họ tên:** Nguyen Anh Dung  
**MSSV:** 2A202602554  
**Sản phẩm:** AI Ghi chép Hội thoại Khám bệnh

AI ghi nhận hội thoại giữa bác sĩ và bệnh nhân, tạo bản nháp hồ sơ khám bệnh có cấu trúc, để bác sĩ chỉnh sửa và duyệt trước khi import vào EHR. Bản chưa được bác sĩ duyệt không được ghi vào hồ sơ chính thức.

**Câu chốt loại mô hình:** Chúng tôi là **B2B** vì bệnh viện/phòng khám là bên trả tiền, còn bác sĩ và nhân viên y tế của chính tổ chức đó là người sử dụng trực tiếp; sản phẩm được triển khai vào quy trình khám và hệ thống EHR của tổ chức.

## Phạm vi và quy ước số liệu

- Dashboard và worksheet được chốt theo trạng thái sản phẩm trước pilot ngày 09/10/2026.
- Các số có nhãn `[MH]` là giả định minh họa suy từ mô hình; `[TB]` là baseline cần đo trong pilot; `[BM]` là benchmark có nguồn và ngày kiểm tra.
- Chưa có khách hàng, doanh thu hoặc số liệu người dùng thật; không đưa dữ liệu mật, API key hay thông tin bệnh nhân vào repo.

## Cấu trúc bài nộp

- `worksheet.md`: Trạm 1–4, thẻ đèn, ngưỡng, phép tính và luật quyết định.
- `dashboard.md`: Operating Dashboard Trạm 5.
- `dashboard.pdf`: bản PDF để chấm trước, gồm dashboard và phụ lục `[MH]`.
