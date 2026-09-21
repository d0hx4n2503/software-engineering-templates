# E2E Test Case Template

> Template chuẩn, dùng chung cho QA/QC ở mọi ngôn ngữ/dự án (web, mobile, API, blockchain, smart contract...). Áp dụng được cho cả test **manual** và test **tự động hóa** (Playwright, Cypress, Selenium...) — vì bản chất đây là tài liệu thiết kế test case, không phải code.

---

## 0. E2E nằm ở đâu trong bức tranh testing?

E2E nằm trên đỉnh **test pyramid** (Unit → Integration → E2E): càng lên cao, số lượng test càng ít nhưng càng chậm, càng tốn chi phí viết/chạy/bảo trì. Nguyên tắc chọn loại test:

| Tầng | Kiểm tra gì | Tốc độ | Khi nào dùng |
|---|---|---|---|
| Unit | 1 function/class, cô lập hoàn toàn | Rất nhanh (ms) | Logic nghiệp vụ, edge case, tính toán |
| Integration | Sự tương tác giữa 2+ component/service | Trung bình | API-DB, contract giữa các module |
| E2E | Toàn bộ luồng người dùng, hệ thống thật | Chậm, tốn kém | Luồng nghiệp vụ quan trọng (checkout, login, onboarding) |

→ **Không dùng E2E để test hết mọi edge case UI** (tốn kém, dễ flaky) — việc đó để lại cho Unit/Integration. E2E chỉ nên cover các luồng **rủi ro cao – tác động lớn – tần suất dùng nhiều**.

---

## 1. Nguyên tắc khi viết E2E test case

1. **Đứng ở góc nhìn người dùng cuối** — test phải phản ánh hành vi thực tế (VD: thêm hàng vào giỏ → áp mã giảm giá → thanh toán), không chỉ kiểm tra logic kỹ thuật đơn lẻ.
2. **Format nhất quán** — mọi test case dùng chung cấu trúc: Test ID, Title, Preconditions, Steps, Expected Result... để cả team dễ review, người mới dễ tiếp cận.
3. **Test data rõ ràng, không hardcode giá trị động** (timestamp, random ID...) — dùng mock data/API để input luôn ổn định và test có thể lặp lại y hệt mỗi lần chạy.
4. **Kiểm soát flaky test chủ động** — ưu tiên smart-wait (chờ theo điều kiện/element) thay vì fixed delay/sleep; khi test fail flaky, ghi lại timestamp + snapshot môi trường để trace sau.
5. **Modular & reusable** — tách các bước lặp lại (login, add-to-cart...) thành step/function dùng chung, để khi 1 flow đổi (VD: thêm 2FA) chỉ cần sửa 1 chỗ.
6. **Selector ổn định** — ưu tiên `data-test="..."` / accessibility id thay vì CSS/XPath dễ vỡ khi đổi giao diện.
7. **"Stranger test"** — trước khi merge, đưa test case cho một người chưa từng tham gia viết. Nếu họ chạy được mà không cần hỏi lại câu nào → test case đạt chuẩn.
8. **Chỉ test những gì hệ thống thật cần** — E2E chạy trên môi trường giống production nhất có thể (network, middleware, backend service thật hoặc staging tương đương), không mock quá sâu như unit test.

---

## 2. Thiết kế test case theo 3 khối (Design Blocks)

Trước khi viết từng test case, nên đi qua 3 bước để đảm bảo bao phủ đủ:

1. **User Functions** — liệt kê các hành động người dùng khởi tạo (và các sub-system liên quan); ghi rõ input/output của từng hành động; xác định hành động nào độc lập, hành động nào có thể tái sử dụng.
2. **Conditions** — với mỗi user function, liệt kê các điều kiện ảnh hưởng (thời gian, trạng thái dữ liệu, quyền user, môi trường...).
3. **Test Cases** — với mỗi condition, tạo ít nhất 1 test case; mỗi điều kiện nên là 1 test case riêng biệt để dễ trace khi fail.

---

## 3. Test Case Template (copy phần dưới cho từng case)

```markdown
### [TC-XXX-000] <Tên ngắn gọn mô tả điều được kiểm thử>

| Field              | Details |
|--------------------|---------|
| **Test Case ID**   | TC-XXX-000 |
| **Title**          | Verify <hành vi cụ thể cần xác nhận> |
| **Module/Feature** | <VD: Checkout / Auth / Wallet Connect> |
| **Priority**       | Critical / High / Medium / Low |
| **Test Type**      | Positive / Negative / Edge case |
| **Preconditions**  | 1. ... <trạng thái hệ thống, user, dữ liệu phải có trước khi test> |
| **Test Data**      | User: ... / Input: ... / Config: ... (giá trị cố định, không random) |
| **Test Steps**     | 1. ... 2. ... 3. ... (từng bước rõ ràng, có thể copy-paste để tái hiện) |
| **Expected Result**| Kết quả đầy đủ: UI hiển thị gì, giá trị/số liệu chính xác bao nhiêu, side-effect nào xảy ra (email, log, event...) |
| **Actual Result**  | *(điền khi thực thi)* |
| **Pass/Fail**      | *(điền khi thực thi)* |
| **Postconditions** | Trạng thái hệ thống/DB sau khi test chạy xong (VD: order xuất hiện trong history, số dư/inventory thay đổi) |
| **Notes/Edge Risks**| Rủi ro cạnh biên cần lưu ý thêm (double-submit, input đặc biệt, race condition...) |
```

---

## 4. Checklist trước khi chốt test case

- [ ] Preconditions đủ cụ thể (user nào, môi trường nào, trạng thái dữ liệu nào) — không viết chung chung kiểu "user đã đăng nhập".
- [ ] Test Steps đủ chi tiết để người khác chạy lại y hệt, không cần đoán.
- [ ] Expected Result mô tả **toàn bộ** kết quả (UI + số liệu + side-effect), không chỉ "thành công".
- [ ] Test Data cố định, không hardcode giá trị động (timestamp, ID random...).
- [ ] Postconditions có kiểm tra trạng thái backend/DB, không chỉ UI.
- [ ] Đã cover đủ 3 loại case: Positive / Negative / Edge case cho flow quan trọng.
- [ ] Đã pass "stranger test" (người ngoài đọc hiểu và chạy được không cần hỏi).

---

## 5. Quy trình áp dụng E2E (4 giai đoạn)

**Planning (Lập kế hoạch)**
- Phân tích yêu cầu nghiệp vụ/chức năng.
- Xác định luồng ưu tiên theo: Risk (rủi ro) – Impact (tác động) – Frequency (tần suất dùng).
- Xây dựng test plan + test case, chuẩn bị môi trường giống production, chuẩn bị test data, chốt exit criteria (tiêu chí để coi là "đạt").

**Pre-requisite (Điều kiện tiên quyết)**
- Toàn bộ system/module con đã pass integration/system test riêng lẻ.
- Các sub-system đã được ghép nối hoạt động như 1 ứng dụng hoàn chỉnh.
- Môi trường test giống production đã sẵn sàng.

**Test Execution (Thực thi)**
- Chạy test case, ghi nhận kết quả pass/fail.
- Report bug vào tool quản lý (Jira, AzDO...).
- Re-verify sau khi bug được fix.

**Test Closure (Đóng giai đoạn test)**
- Tổng hợp báo cáo test.
- Đánh giá lại theo exit criteria đã đặt ra.
- Đóng phase test.

---

## 6. Test Metrics nên theo dõi

- **Test case preparation status** — số test case đã sẵn sàng / tổng số cần có.
- **Test progress** — số test case đã chạy theo tần suất định kỳ (tuần/sprint) so với mục tiêu.
- **Defect status** — % bug open/closed theo mức độ nghiêm trọng và ưu tiên.
- **Test environment availability** — thời gian môi trường test thực sự sẵn sàng so với lịch đã cấp phát.

---

## 7. Checklist môi trường & vận hành (áp dụng chung cho pipeline)

**Trước khi chạy:**
- Reset database/state về trạng thái sạch trước mỗi lần chạy.
- Đảm bảo môi trường test khớp với production (firewall, config...) để tránh bug ẩn.
- Tự động hoá bước reset (script), không làm tay.

**Khi chạy & báo cáo:**
- Theo dõi qua CI/CD dashboard (Jenkins, GitHub Actions...).
- Log đầy đủ: stack trace, network request, console output khi fail.
- Tự động chụp screenshot khi fail.
- Report tập trung vào xu hướng (trend), không chỉ đếm pass/fail.

**Bảo trì liên tục:**
- Chạy test tự động trên mọi commit (CI/CD).
- Định kỳ review test chậm/flaky để tối ưu hoặc refactor.
- Ưu tiên test theo risk – impact – frequency, tránh test thừa cho việc nhỏ nhặt (VD: màu nút).

---

## 8. Ví dụ mẫu (tham khảo)

```markdown
### [TC-CHK-004] Verify successful checkout with a valid discount code applied

| Field              | Details |
|--------------------|---------|
| **Test Case ID**   | TC-CHK-004 |
| **Title**          | Verify successful checkout with a valid discount code applied |
| **Module/Feature** | Checkout / Payment |
| **Priority**       | Critical |
| **Test Type**      | Positive / Functional |
| **Preconditions**  | 1. User đã đăng ký và đăng nhập. 2. Có ít nhất 1 item trong giỏ. 3. Mã giảm giá hợp lệ "SAVE20" (giảm 20%) tồn tại trong hệ thống. 4. Phương thức thanh toán test được cấu hình ở staging. |
| **Test Data**      | User: test_user@example.com / Password: Test@1234 / Mã: SAVE20 / Sản phẩm: "Wireless Headphones – $120" |
| **Test Steps**     | 1. Vào trang giỏ hàng. 2. Xác nhận item "Wireless Headphones" hiển thị giá $120. 3. Nhập mã "SAVE20" vào ô promo code, bấm Apply. 4. Xác nhận giảm $24, tổng còn $96. 5. Bấm "Proceed to Checkout". 6. Nhập thông tin giao hàng. 7. Chọn shipping tiêu chuẩn. 8. Nhập thẻ test, bấm "Place Order". |
| **Expected Result**| Trang xác nhận đơn hàng hiển thị: giá gốc $120, giảm –$24, tổng $96. Email xác nhận gửi tới test_user@example.com trong vòng 2 phút. |
| **Actual Result**  | *(điền khi thực thi)* |
| **Pass/Fail**      | *(điền khi thực thi)* |
| **Postconditions** | Đơn hàng xuất hiện trong lịch sử đơn của user. Tồn kho "Wireless Headphones" giảm 1. Mã SAVE20 vẫn còn hiệu lực (không phải single-use). |
| **Notes/Edge Risks**| Kiểm tra ô mã giảm giá chỉ nhận alphanumeric. Kiểm tra hành vi khi áp mã 2 lần. Xác nhận giảm giá không áp vào phí ship. |
```

---

## 9. Nguồn tham khảo

1. DeviQA — *How to Write E2E Test Cases: Templates, Examples & Best Practices*: https://www.deviqa.com/blog/how-to-build-e2e-test-cases/
2. Microsoft (ISE) — *Engineering Fundamentals Playbook, E2E Testing*: https://microsoft.github.io/code-with-engineering-playbook/automated-testing/e2e-testing/
3. BrowserStack — *What is End To End (E2E) Testing*: https://www.browserstack.com/guide/end-to-end-testing
4. Katalon — *What Is End-to-End Testing? Definition, Tools & Best Practices*: https://katalon.com/resources-center/blog/end-to-end-e2e-testing
5. TestGrid — *What is End to End Testing - Detailed Guide*: https://testgrid.io/blog/end-to-end-testing-a-detailed-guide/
6. Martin Fowler — *TestPyramid*: https://martinfowler.com/bliki/TestPyramid.html