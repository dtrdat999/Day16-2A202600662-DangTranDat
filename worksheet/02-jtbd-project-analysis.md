---
artifact: 02 — JTBD Project Analysis
bai-tap: Lab 2 — Dùng JTBD để phân tích dự án
---

# Lab 2 — JTBD Project Analysis

## Thông tin

- **Người làm:** Đặng Trần Đạt
- **Mã học viên:** 2A202600662
- **Hình thức:** Cá nhân
- **Tên dự án:** DevVerify
- **Ngày:** 18/06/2026

## 1. Project Slice

DevVerify là ý tưởng sản phẩm hỗ trợ developer kiểm chứng câu trả lời và đoạn
code do AI đề xuất trước khi áp dụng vào codebase thật.

Lát cắt được phân tích trong bài là:

> Developer đang debug một lỗi trong dự án và đã nhận được hướng sửa từ AI,
> nhưng cần xác định hướng sửa đó có đúng, tương thích và đủ an toàn để áp dụng
> hay không.

Lát cắt này chỉ tập trung vào bước kiểm chứng trước khi sửa code, không bao gồm
toàn bộ quy trình phát triển phần mềm hay một AI coding assistant hoàn chỉnh.

## 2. Project Snapshot và Market Context

### Project Snapshot

1. **Vấn đề cần giải quyết:** AI có thể đưa ra câu trả lời nhanh và thuyết phục
   nhưng vẫn có khả năng dùng API không tồn tại, bỏ sót context, tạo lỗ hổng
   bảo mật hoặc đề xuất code không tương thích với dự án.
2. **Người dùng chính:** Developer đang sử dụng ChatGPT, Claude, Gemini,
   GitHub Copilot hoặc Cursor để debug và viết code.
3. **Cách họ xử lý hiện nay:** Chạy thử code, đọc documentation, tìm trên
   Stack Overflow/GitHub Issues, viết test hoặc hỏi đồng nghiệp.

### Market Context

AI coding assistant đã trở thành một phần phổ biến trong workflow của
developer. Stack Overflow Developer Survey 2025 cho biết 84% người trả lời
đang dùng hoặc dự định dùng AI trong quy trình phát triển, nhưng nghiên cứu
trên 517 câu hỏi lập trình cũng phát hiện 52% câu trả lời ChatGPT được đánh giá
có thông tin sai.

Vì vậy, vấn đề mới không chỉ là tìm được câu trả lời nhanh mà còn là xác định
câu trả lời nào có thể tin và áp dụng trong context cụ thể. Developer hiện phải
tự ghép nhiều phương pháp kiểm chứng, khiến lợi ích tiết kiệm thời gian của AI
bị giảm và tạo rủi ro nếu câu trả lời được chấp nhận quá sớm.

## 3. Job Executor và Core JTBD

### Job Executor

**Job executor:** Developer trực tiếp sử dụng AI để debug hoặc sửa code trong
một dự án phần mềm.

Đây là người trực tiếp đọc đề xuất, đánh giá bằng chứng, chạy thử và chịu trách
nhiệm quyết định có đưa thay đổi vào codebase hay không. Engineering manager
hoặc doanh nghiệp có thể là buyer, nhưng không phải người trực tiếp thực hiện
job trong lát cắt này.

### Core JTBD

> Xác minh một hướng sửa lỗi có đúng, tương thích và đủ an toàn trong context
> của dự án trước khi áp dụng.

Core JTBD này không phụ thuộc vào DevVerify hay AI. Ngay cả khi công cụ hiện
tại biến mất, developer vẫn phải hoàn thành công việc kiểm chứng trước khi sửa
code.

## 4. Job Stories

| # | When — Tình huống | I want to — Động lực | So I can — Kết quả |
|---|---|---|---|
| JS1 | Khi AI đề xuất dùng một API hoặc thư viện mà tôi chưa quen | Tôi muốn kiểm tra API đó có thật và phù hợp với phiên bản dependency của dự án | Tôi có thể áp dụng hướng sửa mà không tạo thêm lỗi tương thích |
| JS2 | Khi tôi đang xử lý lỗi gấp và nhận được nhiều hướng giải quyết khác nhau | Tôi muốn biết hướng nào có bằng chứng đáng tin nhất và phù hợp nhất với codebase | Tôi có thể chọn phương án nhanh mà vẫn kiểm soát rủi ro |
| JS3 | Khi code do AI tạo đã compile hoặc chạy được ở ví dụ đơn giản | Tôi muốn kiểm tra edge case, bảo mật và ảnh hưởng tới hành vi hiện tại | Tôi có thể merge thay đổi mà không gây regression hoặc lỗ hổng mới |

Ba story cho thấy sản phẩm đáng xuất hiện sau khi AI đưa ra câu trả lời nhưng
trước khi developer áp dụng hoặc merge code.

## 5. Current Alternatives

| Giải pháp hiện tại | User thuê nó để làm gì? | Điểm mạnh | Điểm yếu | Switching cost |
|---|---|---|---|---|
| Documentation chính thức | Xác minh API, cú pháp, version và hành vi được hỗ trợ | Nguồn gốc, độ tin cậy cao | Phân tán, tốn thời gian đọc và khó nối với context dự án | Trung bình |
| Stack Overflow, GitHub Issues và web search | Tìm case tương tự và kinh nghiệm từ cộng đồng | Nhiều tình huống thực tế, có thảo luận và dấu hiệu kiểm chứng | Có thể cũ, khác version hoặc không khớp hoàn toàn với codebase | Thấp |
| Chạy test, sandbox và code review | Kiểm tra hướng sửa trong thực tế | Sát với codebase và phát hiện regression tốt | Tốn thời gian; coverage không đầy đủ; cần người có kinh nghiệm | Cao |

Nếu DevVerify biến mất, developer nhiều khả năng sẽ quay về kết hợp web search,
documentation và chạy test thủ công.

## 6. JTBD Lite Map

| Step | User đang cố làm gì? | Cách làm hiện tại | Friction / pain | Mức đau |
|---|---|---|---|---|
| Define | Xác định lỗi, phạm vi ảnh hưởng và tiêu chí để kết luận một hướng sửa là hợp lệ | Đọc log, stack trace và mô tả lại vấn đề | Triệu chứng có thể bị nhầm với nguyên nhân gốc | High |
| Locate | Tìm documentation, changelog, issue và case tương tự liên quan tới đề xuất | Search web, Stack Overflow, GitHub và tài liệu | Nguồn phân tán, khác version và tốn thời gian đối chiếu | High |
| Prepare | Chuẩn bị code context, dependency, branch và test case tái hiện lỗi | Thu thập thủ công và tạo sandbox hoặc test | Khó tái hiện; dễ thiếu context cần thiết | Medium |
| Confirm | Xác nhận đã có đủ môi trường, quyền truy cập, nguồn và tiêu chí để bắt đầu kiểm chứng | Kiểm tra checklist và chạy thử test tái hiện | Có thể bắt đầu kiểm chứng khi dữ liệu hoặc môi trường chưa sẵn sàng | Medium |
| Execute | Đối chiếu từng claim với codebase và tài liệu, sau đó chạy test hoặc static analysis | Đọc nguồn, sửa code và chạy nhiều công cụ thủ công | Dễ bỏ sót mâu thuẫn; mất thời gian; đề xuất AI thường nghe rất thuyết phục | High |
| Monitor | Quan sát kết quả test, log, cảnh báo bảo mật và thay đổi hành vi | Theo dõi output từ test suite, linter và ứng dụng | Tín hiệu nằm ở nhiều công cụ và coverage có thể không đầy đủ | Medium |
| Modify | Điều chỉnh đề xuất, test hoặc phạm vi kiểm chứng khi phát hiện vấn đề | Quay lại AI, sửa prompt, test hoặc code | Vòng lặp thử-sai dài và dễ mất context | Medium |
| Conclude | Quyết định chấp nhận hoặc loại bỏ hướng sửa, đồng thời lưu lại bằng chứng | Code review, pull request và ghi chú thủ công | Khó truy vết nguồn và lý do tin hoặc không tin đề xuất | Medium |

### Hai bước đau nhất

1. **Locate:** Developer phải tìm và đối chiếu nhiều nguồn có độ mới, phiên bản
   và độ tin cậy khác nhau.
2. **Execute:** Developer phải kiểm tra từng claim và khó nhận ra một đề xuất
   nghe hợp lý nhưng không đúng với dependency, kiến trúc hoặc yêu cầu bảo mật.

Đây là hai bước đáng chú ý nhất vì chúng quyết định developer có đang kiểm
chứng bằng bằng chứng thật hay chỉ bị thuyết phục bởi cách AI diễn đạt.

## 7. AI Leverage Point

| Step | AI nên hỗ trợ như thế nào? | Vì sao AI phù hợp? | Rủi ro chính |
|---|---|---|---|
| Locate | Tự động truy xuất documentation chính thức, changelog, issue và nguồn cộng đồng liên quan tới đúng dependency/version | AI có thể hiểu truy vấn tự nhiên và tổng hợp nhiều nguồn nhanh hơn tìm kiếm thủ công | Truy xuất sai nguồn, dùng tài liệu cũ hoặc tạo citation không tồn tại |
| Execute | So sánh đề xuất với codebase, dependency, test và policy; nêu claim nào đã có bằng chứng và claim nào chưa được xác minh | AI phù hợp với đối chiếu context và phát hiện mâu thuẫn trên nhiều artifact | Bỏ sót edge case, đánh giá quá tự tin hoặc làm lộ source code nhạy cảm |

**AI leverage point chính:** Bước Execute.

Giá trị lớn nhất không phải tạo thêm một câu trả lời, mà là biến đề xuất thành
một tập claim có thể kiểm tra, gắn mỗi claim với nguồn hoặc test phù hợp và
hiển thị rõ phần chưa chắc chắn. Locate hỗ trợ thu thập bằng chứng, nhưng
Execute mới là bước tạo ra kết quả kiểm chứng để developer quyết định.

## 8. Product Hypothesis

> Nếu DevVerify giúp developer xác minh hướng sửa lỗi ở bước Execute bằng cách
> đối chiếu đề xuất AI với codebase, dependency, documentation có nguồn và kết
> quả test, thì họ sẽ chuyển từ quy trình tìm kiếm và kiểm tra thủ công phân
> tán sang một workflow kiểm chứng tập trung, vì họ có thể áp dụng code nhanh
> hơn mà vẫn nhìn thấy bằng chứng, rủi ro và phần chưa chắc chắn.

### Tín hiệu sớm nếu hypothesis đúng

1. Developer giảm thời gian từ lúc nhận đề xuất AI đến lúc quyết định chấp
   nhận hoặc loại bỏ đề xuất đó.
2. Người dùng thường xuyên mở nguồn dẫn, chạy test được gợi ý và phát hiện được
   đề xuất sai trước khi merge.

## 9. Assumptions to Validate

| Assumption | Vì sao rủi ro? | Bằng chứng hiện có | Cách validate |
|---|---|---|---|
| A1. Developer gặp pain kiểm chứng đủ thường xuyên | Nếu pain hiếm, họ sẽ tiếp tục dùng quy trình hiện tại | AI được dùng rộng rãi nhưng mức độ kiểm chứng thực tế chưa rõ | Phỏng vấn 8–10 developer và thu diary về các đề xuất AI trong một tuần |
| A2. Locate và Execute là hai bước đau nhất | Pain thật có thể nằm ở tái hiện lỗi, chuẩn bị môi trường hoặc theo dõi kết quả | Phân tích workflow hiện tại mới chỉ là giả thuyết | Quan sát trực tiếp 5 phiên debug có sử dụng AI |
| A3. Có thể truy cập đủ context mà không gây rủi ro bảo mật | Doanh nghiệp có thể không cho phép gửi source code ra ngoài | Các AI coding tool enterprise cho thấy nhu cầu tồn tại | Prototype local-first và phỏng vấn security/engineering lead |
| A4. Người dùng tin một lớp AI dùng để kiểm chứng AI khác | Nếu chỉ tạo thêm một câu trả lời, sản phẩm không giải quyết trust | Nguồn dẫn và test có thể tạo bằng chứng độc lập | A/B test báo cáo có citation/test với câu trả lời chỉ có kết luận |
| A5. Giá trị tiết kiệm thời gian lớn hơn chi phí tích hợp | Nếu setup phức tạp, developer sẽ quay lại search và test thủ công | Chưa có bằng chứng trực tiếp | Làm IDE extension prototype và đo time-to-decision trên task thật |

**Assumption nguy hiểm nhất:** A4 — developer sẽ tin quy trình kiểm chứng nếu
kết luận được gắn với bằng chứng có thể mở, chạy và tự xác nhận. Nếu DevVerify
chỉ dùng AI để đưa ra thêm một phán đoán không kiểm chứng được, toàn bộ product
hypothesis sẽ sụp đổ.

## 10. Kết luận sau khi xem xét

Tôi giữ nguyên `job executor` và `core JTBD` vì cả hai đều mô tả người trực
tiếp thực hiện công việc và mục tiêu tồn tại độc lập với giải pháp. Tôi điều
chỉnh `AI leverage point` từ Confirm sang Execute để bám đúng trình tự job map:
Confirm xác nhận mọi điều kiện đã sẵn sàng, còn Execute mới là lúc developer
thực hiện việc đối chiếu và chạy kiểm chứng.

Product hypothesis được giữ nguyên về giá trị cốt lõi nhưng cập nhật bước can
thiệp thành Execute. Assumption quan trọng nhất vẫn là người dùng có tin và sử
dụng kết quả kiểm chứng khi mỗi kết luận được gắn với nguồn hoặc test có thể tự
xác nhận hay không.

## 11. Version cuối

- **Job executor:** Developer trực tiếp dùng AI để debug hoặc sửa code.
- **Core JTBD:** Xác minh một hướng sửa lỗi có đúng, tương thích và đủ an toàn
  trong context của dự án trước khi áp dụng.
- **Hai bước đau nhất:** Locate và Execute.
- **AI leverage point:** Đối chiếu đề xuất với codebase, dependency,
  documentation có nguồn và test; hiển thị rõ claim đã được xác minh và phần
  chưa chắc chắn.
- **Product hypothesis:** Developer sẽ chuyển sang workflow kiểm chứng tập
  trung nếu DevVerify giúp họ ra quyết định nhanh hơn bằng bằng chứng có thể tự
  kiểm tra.
- **Assumption cần validate đầu tiên:** Developer tin và sử dụng một lớp kiểm
  chứng có citation/test, thay vì xem nó như thêm một câu trả lời AI.

## Nguồn tham khảo

- [Stack Overflow Developer Survey 2025 — AI](https://survey.stackoverflow.co/2025/ai)
- [Kabir et al. — Is Stack Overflow Obsolete?](https://arxiv.org/abs/2308.02312)
