# Moni Budget Copilot — Demo Scenarios & Run Guide

## Giới thiệu
Đây là tài liệu phần 4 của nhóm `Day06-E403-NhomB3`, tập trung vào:
- demo scenarios ngân sách và purchase advisor
- hướng dẫn chạy prototype
- kiểm tra flow AI thật

## Công cụ & API đã dùng
- Frontend: `HTML`, `CSS`, `JavaScript` (vanilla)
- Backend: `Node.js`, `Express`
- AI model: `OpenAI GPT-4.1-mini`
- OpenAI client và tool-calling flow được triển khai trong `server/openaiClient.js`, `server/prompts.js`, `server/toolSchemas.js`
- Mock routing fallback nếu không có `OPENAI_API_KEY`

## Phân công nhóm
- Tran Quang Huy — 2A202601010
  - Backend chính, tool-calling flow, OpenAI client, prompt engineering, schema tool.
- Truong Hai Quan — 2A202600898
  - Frontend app logic, chat actions, sync state, render cards và trace/log.
- Bui Minh Hieu — 2A202600876
  - Giao diện, layout responsive, style dashboard, assistant Markdown polish.
- Nguyen Si Viet — 2A202600658
  - Demo scenarios, hướng dẫn chạy, test manual các flow ngân sách và purchase advisor.

## Mục tiêu phần 4
- Ghi rõ kịch bản demo và các flow ngân sách / tư vấn mua sắm.
- Viết hướng dẫn chạy prototype để giảng viên và nhóm khác có thể test nhanh.
- Thực hiện test thủ công ít nhất 1 flow AI thật và note lại kết quả.

## Hướng dẫn chạy
1. Mở terminal trong thư mục `codebase`.
2. Cài dependencies:
   ```powershell
   npm install
   ```
3. Tạo file `.env` ở thư mục gốc `codebase`:
   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   OPENAI_MODEL=gpt-4.1-mini
   ```
4. Chạy server:
   ```powershell
   npm start
   ```
5. Mở trình duyệt và truy cập:
   ```text
   http://localhost:5173
   ```

> Nếu không có `OPENAI_API_KEY`, prototype vẫn chạy với mock routing nhưng cần dùng API key để kiểm tra các flow AI thật.

## Cách test manual API
- Gửi request thử vào endpoint chat:
  ```powershell
  Invoke-RestMethod -Method Post -Uri http://localhost:5173/api/moni/chat -ContentType "application/json" -Body '{"message":"Tôi có nên mua tai nghe 1.5 triệu không?","userId":"demo_user","month":"2026-06"}'
  ```
- Quan sát `llmTrace` trong response để xác nhận tool gọi, arguments, tool result và final assistant response.

## Demo scenarios chính
1. **Kiểm tra ngân sách tháng**
   - Input: “Cho tôi biết tháng 6 tôi còn bao nhiêu tiền sau khi trừ chi tiêu đã nhập?”
   - Expected: AI phân tích ngân sách, so sánh thu nhập vs chi tiêu, gợi ý hạn mức chi tiêu còn lại và cảnh báo khi gần vượt.

2. **Tư vấn mua sắm**
   - Input: “Tôi định mua tai nghe 1.5 triệu, ngân sách còn đủ không và nếu mua thì tôi có nên chọn trả góp hay trả ngay?”
   - Expected: AI đưa ra đánh giá dựa trên ngân sách, ưu nhược điểm trả góp/trả ngay và đề xuất phương án phù hợp.

3. **Purchase advisor — so sánh hai phương án**
   - Input: “So sánh mua ngay với trả góp 3 tháng cho khoản chi 2 triệu, dựa vào ngân sách tháng này của tôi.”
   - Expected: AI nêu rõ tổng chi phí, ảnh hưởng đến ngân sách hàng tháng, đề xuất phương án nhẹ nhàng nhất.

## Ghi chú test
- Nếu AI không dùng tool hoặc trả về câu trả lời chung chung, nghĩa là flow chưa gọi đúng tool.
- Nếu response thiếu giá trị cụ thể, cần chỉnh lại prompt/tool schema để assistant đưa ra câu trả lời actionable.
- Ghi lại ít nhất một trường hợp test đã chạy thành công trong `codebase/README-demo.md` hoặc `README.md` nhóm.
