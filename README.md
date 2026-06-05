# Claw-a-thon 2026 — Sample Agents

Bộ tài liệu mẫu chính thức dành cho các đội tham gia **Claw-a-thon 2026**, cuộc thi hackathon AI nội bộ của VNG Group do **GreenNode** phối hợp tổ chức.

---

## Claw-a-thon 2026 là gì?

Claw-a-thon là cuộc thi hackathon nội bộ **dành cho toàn thể nhân viên VNG Group**, với mục tiêu khuyến khích mọi người khám phá, xây dựng và chia sẻ các **AI agent thực sự hữu ích cho công việc hàng ngày** — trên nền tảng **AgentBase của GreenNode**.

Bạn không cần là kỹ sư AI. Bạn chỉ cần có một vấn đề thực tế cần giải quyết.

---

## Tài nguyên mỗi đội được cấp

Mỗi đội đăng ký sẽ nhận đầy đủ để bắt đầu ngay:

| Tài nguyên                      | Chi tiết                                                    |
| --------------------------------- | ------------------------------------------------------------ |
| **Tài khoản tính toán** | Tự động cấp phát bởi Ban Tổ Chức                     |
| **MaaS API Token**          | Truy cập các model Gemma, Qwen qua GreenNode MaaS          |
| **Ví POC**                 | 10.000.000 VND / đội để thử nghiệm dịch vụ GreenNode |
| **Tài liệu hướng dẫn** | Bộ tài liệu kỹ thuật và bộ mẫu (repo này)           |

---

## Lịch trình chính thức

| Mốc | Ngày | Mô tả |
|-----|------|-------|
| Đăng ký | Now → **10/06** | Đăng ký trên Zoho Form; hạn đăng ký trùng ngày Tập Huấn |
| Email chào mừng | Sau khi đăng ký | Email xác nhận + timeline tổng quan (không cần kích hoạt tài khoản) |
| Rulebook teaser | **22/05** | Thể lệ sơ bộ gửi kèm reminder |
| Rulebook chính thức | **27/05** | Bản rulebook v1.2 gửi thí sinh |
| Hội thảo khai mạc | **29/05** | Hội thảo trực tuyến giới thiệu cuộc thi & AgentBase |
| Ngày Tập Huấn + cấp tài nguyên | **10/06** · 08:30–11:30 | Sharing + Demo AgentBase; BTC tự động cấp tài khoản OpenClaw + 10M VND / đội |
| **Giai đoạn phát triển** | **10/06 → 17/06** | 7 ngày phát triển agent trên AgentBase |
| **Hạn nộp bài** | **17/06 · 12:00** | Đóng form nộp bài đúng 12:00 |
| Thẩm định nội bộ (vòng 1) | **17/06** · 13:00–17:00 | BTC thẩm định đạt/không đạt, gửi email kết quả ngay trong ngày |
| Khiếu nại / Bổ sung | **18/06 → 19/06** | Đội KHÔNG ĐẠT được khiếu nại hoặc bổ sung **một (01) lần duy nhất** |
| Công bố kết quả cuối | **22/06** | Danh sách dự án hợp lệ lên trang bình chọn |
| **Khoảng bình chọn** | **22/06 → 03/07** | Toàn VNG Group bình chọn |
| **Lễ Trao Giải** | **03/07** | Lễ trao giải + workshop + networking (offline, TP.HCM) |
| Tổng kết sau sự kiện | từ **04/07** | Báo cáo + case study |

---

## Cơ chế giải thưởng

**Top 5** được xác định dựa trên: bình chọn cộng đồng (toàn VNG Group) + thẩm định đạt của Ban Tổ Chức.

Hình thức trao thưởng gồm 3 dạng kết hợp:

- **Hiện kim** — tiền mặt trao trực tiếp tại lễ (phong bì cho đại diện đội)
- **Hiện vật** — voucher/credit POC dịch vụ AI của GreenNode (cấp vào tài khoản đội thắng cung cấp trong vòng 14 ngày sau sự kiện)
- Trường hợp đại diện Top 5 không thể tham dự: liên hệ Ban Tổ Chức để nhận giải sau

---

## Repo này dành để làm gì?

Đây là **bộ agent mẫu** — không phải đề bài, không phải template bắt buộc. Mục đích:

1. **Minh họa cách xây dựng một agent hoàn chỉnh** — từ kiến trúc, prompt, backend đến UI
2. **Cho thấy mức độ chi tiết kỳ vọng** — cả về code lẫn tài liệu README
3. **Làm điểm khởi đầu** — clone, đọc hiểu, rồi xây agent của riêng bạn theo hướng bạn muốn

Bạn **không cần** làm giống các mẫu này. Đây chỉ là ví dụ tham khảo.

---

## Agents mẫu

### [GreenNode PM Assistant](GreenNode-OpenClaw%20PM%20Assistant/)

> Trợ lý AI chuyên biệt cho Product Manager, chạy 24/7 trên AgentBase — triển khai một click, không cần code.

**Vấn đề giải quyết:** PM phải xử lý khối lượng công việc rộng và phân mảnh — nghiên cứu thị trường, lập roadmap, phỏng vấn người dùng, viết PRD. Nghiên cứu thường xuyên bị trì hoãn vì tốn thời gian, và context bị mất giữa các phiên làm việc.

**Giải pháp:**

- **Nghiên cứu thị trường & đối thủ** — tìm kiếm web, tổng hợp thành báo cáo thay vì trả về danh sách link
- **Lập roadmap** — draft, ưu tiên hóa và cấu trúc roadmap từ mục tiêu hoặc backlog ý tưởng
- **Hỗ trợ phỏng vấn người dùng** — chuẩn bị bộ câu hỏi, tổng hợp insights
- **Viết PRD** — draft và refine PRD từ brief
- **Định cỡ thị trường** — ước tính TAM/SAM/SOM kèm giả định và phương pháp
- **Tự động theo dõi AI & model mới** — định kỳ quét tin tức AI, lọc thông tin liên quan và push báo cáo phân tích + khuyến nghị (Adopt/Adapt/Alert) lên Telegram
- **Bộ nhớ lâu dài** — ghi nhớ context qua các phiên (`SOUL.md`, `USER.md`, `MEMORY.md`)

**Điểm đặc biệt:** Được tạo **một click trên GreenNode AgentBase** (OpenClaw runtime) — không cần server, Dockerfile hay deploy pipeline. Hành vi agent được định nghĩa bằng file Markdown, không phải source code, nên PM không cần kỹ năng kỹ thuật để đọc và điều chỉnh.

**Công nghệ:** GreenNode AgentBase (OpenClaw 1-Click) · GreenNode MaaS · Telegram / Zalo

---

### [Interview Assistant](interview-assistant/)

> Hỗ trợ quy trình phỏng vấn tuyển dụng kỹ thuật tại GreenNode.

**Vấn đề giải quyết:** Người phỏng vấn phải làm nhiều việc cùng lúc — lắng nghe, hỏi follow-up, ghi chú và đánh giá — dẫn đến bỏ sót thông tin và kết quả đánh giá thiếu nhất quán.

**Giải pháp:**

- Upload CV + JD → **gemma-4-31b-it** sinh bộ câu hỏi phỏng vấn phù hợp với ứng viên
- Phiên phỏng vấn được transcribe real-time qua GreenNode MaaS Whisper
- Sau phỏng vấn: **gemma-4-31b-it** chấm điểm theo rubric 9 tiêu chí và xuất báo cáo Excel

**Công nghệ:** FastAPI · WebSocket · GreenNode MaaS (Whisper + gemma-4-31b-it) · openpyxl

---

## Gợi ý hướng xây dựng agent của bạn

Một agent tốt trong cuộc thi thường có:

- **Vấn đề thực tế** — giải quyết điều gì đó bạn hoặc đồng nghiệp gặp phải thực sự
- **Input rõ ràng** — người dùng cung cấp gì? (file, text, audio, form...)
- **Output có giá trị** — agent trả về gì? (báo cáo, code, quyết định, hành động...)
- **AI làm phần khó** — logic phức tạp, phân tích, tổng hợp — không phải chỉ format text

Một số hướng chưa có mẫu để bạn tham khảo:

| Hướng                  | Ví dụ                                                        |
| ------------------------ | -------------------------------------------------------------- |
| Tự động hóa nội bộ | Agent tóm tắt meeting, tạo task từ biên bản, draft email |
| Hỗ trợ kỹ thuật      | Agent review PR, phân tích log, debug alert                  |
| Khách hàng             | Agent triage support ticket, trả lời FAQ, onboarding guide   |
| Dữ liệu                | Agent phân tích báo cáo, sinh insight từ spreadsheet      |
| Vận hành               | Agent theo dõi KPI, báo cáo tự động, escalation          |

---

## Liên hệ Ban Tổ Chức

Nếu có câu hỏi về tài nguyên, tiêu chí hợp lệ (§7), hoặc quy trình nộp bài — liên hệ đội GreenNode Product Marketing.
