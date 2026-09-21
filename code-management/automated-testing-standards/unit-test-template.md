# Unit Test Template

> Template chuẩn, áp dụng chung cho mọi ngôn ngữ (JS/TS, Python, Java, C#, Rust, Go...). Unit test luôn do dev viết, kiểm tra **1 function/method/class duy nhất, cô lập hoàn toàn** khỏi DB, network, service ngoài, hay UI.

---

## 1. Unit test là gì?

Một unit test kiểm tra **1 đơn vị nhỏ nhất có thể test độc lập** — thường là 1 function, 1 method, hoặc 1 class. Tính "cô lập" (isolation) là cốt lõi: unit test **không** gọi DB thật, không gọi service ngoài, không load UI. Nhờ vậy nó nhanh, xác định (deterministic), và khi fail thì biết ngay chỗ hỏng.

Unit test nằm ở **đáy test pyramid** — số lượng nhiều nhất, chạy nhanh nhất, rẻ nhất để viết/bảo trì. Nó **không** thay thế được integration test hay E2E test:

| Unit test không cover được | Vì sao |
|---|---|
| Lỗi tích hợp/contract giữa các component | Vì mọi dependency đều bị mock/stub |
| Lỗi cấu hình, wiring | Nằm ngoài phạm vi 1 unit |
| Race condition, vấn đề hiệu năng | Cần môi trường thật để bộc lộ |
| Bug chỉ xuất hiện ở môi trường cụ thể | Unit test chạy trong môi trường giả lập |

→ Unit test là **nền móng**, nhưng vẫn cần bổ sung Integration test + E2E test (xem thêm `e2e-testcase-template.md`) để bao phủ đầy đủ.

---

## 2. Nguyên tắc FIRST (Robert C. Martin — *Clean Code*)

| Nguyên tắc | Ý nghĩa |
|---|---|
| **F — Fast** | Chạy trong mili-giây, để có thể chạy liên tục khi code. |
| **I — Independent/Isolated** | Test không phụ thuộc lẫn nhau hay vào thứ tự chạy. Đây là nguyên tắc hay bị vi phạm nhất — VD: 2 test cùng đọc/ghi 1 state chung sẽ pass riêng lẻ nhưng fail khi chạy chung suite. |
| **R — Repeatable** | Chạy ở máy nào, lúc nào cũng ra cùng 1 kết quả — không phụ thuộc dependency ngoài (network, thời gian hệ thống...). |
| **S — Self-validating** | Test tự quyết định pass/fail (qua assertion), không cần người đọc log bằng mắt để kết luận. |
| **T — Timely** | Viết test gần với thời điểm viết code — lý tưởng là viết test **trước** khi code (TDD). |

---

## 3. Cấu trúc AAA (Arrange – Act – Assert)

Mỗi test nên chia rõ 3 phần, giúp đọc lướt là hiểu ngay:

1. **Arrange** — chuẩn bị: khởi tạo object, input, test double (mock/stub) cần thiết.
2. **Act** — thực thi: gọi 1 hành động duy nhất đang được test (thường là 1 lần gọi hàm/method).
3. **Assert** — kiểm tra: so sánh kết quả thực tế với kỳ vọng.

**Quy tắc quan trọng:**
- Chỉ 1 Act, không đi qua lại giữa Act và Assert.
- Ưu tiên 1 assertion logic / test — nếu 1 test check 5 điều cùng lúc, khi fail sẽ không biết điều nào sai.
- Không lẫn logic xử lý vào phần Arrange (VD: gọi hàm cần test ngay trong lúc setup).

```
function test_<method>_<scenario>_<expected_behavior>() {
    // Arrange
    ...

    // Act
    ...

    // Assert
    ...
}
```

---

## 4. Quy ước đặt tên test

Tên test nên có đủ 3 thành phần: **method/hành vi đang test — kịch bản (scenario) — kết quả kỳ vọng**.

```
<MethodName>_<Scenario>_<ExpectedBehavior>
```

Ví dụ: `ProcessPayment_InsufficientFunds_ThrowsPaymentException`

Cách đặt tên này giúp khi test fail trên CI, chỉ cần đọc tên là biết ngay cái gì hỏng, không cần đọc code implementation.

---

## 5. Test double (mock, stub, fake, spy, dummy)

Test double là vật thay thế cho dependency thật, giúp unit thực sự chạy cô lập (theo Gerard Meszaros, *xUnit Test Patterns*):

| Loại | Mục đích | Kiểm tra gì |
|---|---|---|
| **Dummy** | Chỉ lấp đầy tham số, không dùng đến | Không kiểm tra gì |
| **Fake** | Bản triển khai nhẹ, hoạt động thật (VD: in-memory DB) | Gián tiếp qua state |
| **Stub** | Trả về giá trị dựng sẵn | State |
| **Spy** | Stub có ghi lại các lần gọi | State + lời gọi (sau khi chạy) |
| **Mock** | Có kỳ vọng (expectation), fail nếu bị gọi sai cách | Hành vi tương tác (interaction) |

**Nguyên tắc:** chỉ mock ranh giới thật sự bên ngoài (DB, API, filesystem...), **không mock quá nhiều class nội bộ** — càng mock nhiều, test càng dễ vỡ khi refactor (brittle test) và càng ít giá trị thực.

---

## 6. Test Case Template (copy phần dưới cho từng case)

```markdown
### [UT-XXX-000] <Method/Function đang test> — <Scenario>

| Field                | Details |
|-----------------------|---------|
| **Test ID**           | UT-XXX-000 |
| **Target**            | <Module.ClassName.methodName> |
| **Scenario**          | Positive / Negative / Edge case / Error handling |
| **Arrange**           | Input, test double (mock/stub) cần chuẩn bị |
| **Act**               | Lệnh gọi method/function đang test (1 lần duy nhất) |
| **Assert**            | Kết quả kỳ vọng (return value, exception, trạng thái mock được gọi đúng cách) |
| **Mocks/Stubs dùng**  | Liệt kê dependency nào bị thay bằng test double, vì sao |
| **Coverage mục tiêu** | Happy path / Edge case cụ thể / Error path |
```

---

## 7. Checklist trước khi merge unit test

- [ ] Test tuân thủ FIRST (đặc biệt: có test nào phụ thuộc test khác hoặc thứ tự chạy không?).
- [ ] Có đúng 1 Act, và assertion tập trung vào 1 hành vi logic.
- [ ] Tên test mô tả rõ method – scenario – expected behavior.
- [ ] Chỉ mock dependency ngoài (DB/API/service), không mock tràn lan class nội bộ.
- [ ] Đã cover: happy path, ít nhất 1 edge case, và error/exception path.
- [ ] Test không test lại thư viện ngoài (framework, SDK) — chỉ test logic của mình.
- [ ] Không dùng `new Date()`/`Date.now()`/random trực tiếp trong test — mock hoặc inject thời gian/giá trị ngẫu nhiên để test repeatable.
- [ ] Test có thể chạy độc lập (chạy riêng 1 mình vẫn pass y như chạy cả suite).

---

## 8. Framework theo ngôn ngữ (tham khảo)

| Framework | Ngôn ngữ | Đặc điểm |
|---|---|---|
| JUnit 5 | Java | Chuẩn de facto trên JVM, dùng annotation |
| pytest | Python | `assert` thuần, fixture, ít boilerplate |
| NUnit / xUnit | C#/.NET | Attribute-based (NUnit) / tối giản (xUnit) |
| Jest | JavaScript/TypeScript | Mocking built-in, snapshot testing, nhanh |
| Vitest | JavaScript/TypeScript | Tương thích Jest API, tối ưu cho Vite/ESM |

Nguyên tắc chọn: theo hệ sinh thái ngôn ngữ của team, không chọn theo "framework nhiều tính năng nhất" — tính nhất quán trong team quan trọng hơn.

---

## 9. Ví dụ mẫu (Python/pytest, tham khảo)

```markdown
### [UT-CALC-001] Calculator.add — cộng 2 số dương

| Field                | Details |
|-----------------------|---------|
| **Test ID**           | UT-CALC-001 |
| **Target**            | Calculator.add(a, b) |
| **Scenario**          | Positive |
| **Arrange**           | `calculator = Calculator()` |
| **Act**               | `result = calculator.add(2, 3)` |
| **Assert**            | `assert result == 5` |
| **Mocks/Stubs dùng**  | Không cần — hàm thuần logic, không có dependency ngoài |
| **Coverage mục tiêu** | Happy path |
```

```python
def test_add_two_positive_numbers_returns_sum():
    # Arrange
    calculator = Calculator()

    # Act
    result = calculator.add(2, 3)

    # Assert
    assert result == 5
```

---

## 10. Nguồn tham khảo

1. Autemos — *Unit Testing Explained: AAA Pattern, FIRST & Frameworks*: https://www.autemos.com/en/blogs/unit-test
2. Robert C. Martin — *Clean Code* (nguồn gốc FIRST principles), Chương 9: https://www.oreilly.com/library/view/clean-code-a/9780136083238/
3. Bill Wake — *3A – Arrange, Act, Assert* (nguồn gốc AAA pattern, 2001): https://xp123.com/3a-arrange-act-assert/
4. Semaphore — *The Arrange, Act, and Assert (AAA) Pattern in Unit Test Automation*: https://semaphore.io/blog/aaa-pattern-test-automation
5. Vladimir Khorikov — *Unit Testing: Principles, Practices, and Patterns* (Manning): https://livebook.manning.com/book/unit-testing/chapter-3
6. Gerard Meszaros — *xUnit Test Patterns* (định nghĩa Test Double: Dummy/Fake/Stub/Spy/Mock): http://xunitpatterns.com/Test%20Double.html
7. Martin Fowler — *TestPyramid*: https://martinfowler.com/bliki/TestPyramid.html
8. Microsoft (ISE) — *Engineering Fundamentals Playbook, Unit Testing*: https://microsoft.github.io/code-with-engineering-playbook/automated-testing/unit-testing/
9. javascript-unit-testing-best-practices (GitHub, andredesousa): https://github.com/andredesousa/javascript-unit-testing-best-practices
10. ISTQB Glossary — *Component Testing*: https://glossary.istqb.org/en_US/term/component-testing