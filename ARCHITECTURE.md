.
├── .github/                                     # Tích hợp tự động hóa riêng cho nền tảng GitHub
│   ├── ISSUE_TEMPLATE/                          # Mẫu khai báo công việc và báo lỗi
│   │   ├── bug_report.md                        # Chi tiết biểu mẫu báo lỗi hệ thống
│   │   ├── feature_request.md                   # Chi tiết biểu mẫu đề xuất tính năng mới
│   │   └── task_card.md                         # Biểu mẫu tạo task công việc thông thường
│   ├── workflow-templates/                       # Các bộ khung CI/CD tái sử dụng cho dự án mới
│   │   ├── ci-pipeline.yml                      # Cấu hình mẫu tự động Build và Test
│   │   └── cd-pipeline.yml                      # Cấu hình mẫu tự động Deploy
│   ├── CODEOWNERS                               # Định nghĩa quyền duyệt code bắt buộc theo phân vùng
│   ├── PULL_REQUEST_TEMPLATE.md                 # Biểu mẫu bắt buộc khi tạo Pull Request
│   └── dependabot.yml                           # Tự động quét và cập nhật thư viện lỗi thời
├── 01-code-management/                          # I. Quản lý Code & Quy trình Phát triển
│   ├── git-branching-workflow.md                # Quy định đặt tên branch, commit và luồng Git
│   ├── code-review-checklist.md                 # Checklist dành cho Reviewer và Author
│   ├── definition-of-ready.md                   # Tiêu chuẩn đầu vào để Task được phép code (DoR)
│   ├── definition-of-done.md                    # Tiêu chuẩn đầu ra để Task được đóng (DoD)
│   └── automated-testing-standards.md           # Tiêu chuẩn viết Unit, Integration và E2E Test
├── 02-architecture-design/                      # II. Kiến trúc & Thiết kế Hệ thống
│   ├── request-for-comments/                    # Thư mục chứa các đề xuất kiến trúc sơ khởi
│   │   └── rfc-template.md                      # Biểu mẫu lấy ý kiến giải pháp kỹ thuật từ team
│   ├── architecture-decision-records/           # Thư mục lưu vết các quyết định kiến trúc đã chốt
│   │   ├── adr-template.md                      # Biểu mẫu ghi nhận quyết định thay đổi công nghệ
│   │   └── 0001-record-architecture-decisions.md # Bản ghi ADR đầu tiên làm mẫu
│   ├── system-design-template.md                # Biểu mẫu thiết kế hệ thống tổng thể (High-Level)
│   ├── api-specification/                       # Tài liệu hóa giao tiếp ứng dụng
│   │   ├── openapi-template.yaml                # File mẫu OpenAPI 3.0 (Swagger) chuẩn REST API
│   │   └── graphql-schema-template.graphql      # File mẫu định nghĩa Schema cho GraphQL
│   └── database-design/                         # Thiết kế tầng dữ liệu
│       ├── schema-design-template.md            # Biểu mẫu giải trình thực thể, quan hệ (ERD)
│       └── migration-guide-template.md          # Quy trình viết script và chạy migration an toàn
├── 03-devops-infra-security/                    # III. Vận hành, Hạ tầng & Bảo mật
│   ├── local-development/                       # Chuẩn hóa môi trường máy cá nhân
│   │   ├── docker-compose.local.yml             # File cấu hình Docker khởi chạy DB/Cache cục bộ
│   │   └── devcontainer.json                    # Cấu hình môi trường đồng nhất qua VS Code
│   ├── environments/                            # Cấu hình các môi trường hệ thống
│   │   ├── .env.example                         # File cấu hình biến môi trường mẫu
│   │   └── config.validator.js                  # Script mẫu để kiểm tra tính hợp lệ của file .env
│   ├── logging-monitoring/                      # Giám sát và cảnh báo
│   │   ├── logging-standard.md                  # Quy định cấu hình Log (Format JSON, Log Levels)
│   │   └── dashboard-metrics-template.json      # File export mẫu Dashboard của Grafana/Datadog
│   ├── security-compliance/                     # Bảo mật và tuân thủ
│   │   ├── shift-left-security-checklist.md     # Checklist bảo mật từ khâu code (SAST/DAST)
│   │   ├── data-privacy-policy.md               # Quy định xử lý dữ liệu nhạy cảm (PII/GDPR)
│   │   └── secret-scanning-config.json          # Cấu hình loại trừ/quét lộ mã bí mật (GitGuardian)
│   └── disaster-recovery/                       # Khôi phục thảm họa
│       ├── dr-playbook-template.md              # Kịch bản ứng cứu khi hệ thống sập hoàn toàn
│       └── backup-strategy-matrix.md            # Ma trận tần suất và phương án backup dữ liệu
├── 04-documentation-handover/                   # IV. Tài liệu & Bàn giao
│   ├── project-onboarding/                      # Tài liệu nhập môn cho thành viên mới
│   │   └── onboarding-guide-template.md         # Quy trình thiết lập máy và bài test tuần đầu
│   ├── release-management/                      # Quản lý phiên bản nâng cấp
│   │   ├── CHANGELOG.md                         # Nhật ký ghi nhận các thay đổi qua từng version
│   │   └── release-notes-template.md            # Biểu mẫu viết thông báo cập nhật cho khách hàng
│   ├── data-migration/                          # Kế hoạch chuyển dịch dữ liệu lớn
│   │   └── data-migration-plan-template.md      # Kịch bản migrate dữ liệu production không gián đoạn
│   └── offboarding-handover/                    # Bàn giao khi rời dự án hoặc chuyển giao công nghệ
│       └── project-handover-template.md         # Danh mục tài sản, tài khoản, mã nguồn cần bàn giao
├── 05-people-risk-management/                   # V. Quản lý Con người & Rủi ro
│   ├── engineering-growth/                      # Phát triển nhân sự kỹ thuật
│   │   ├── developer-1on1-template.md           # Biểu mẫu cuộc họp định kỳ giữa Lead và Dev
│   │   ├── skill-matrix-framework.xlsx          # Bảng đánh giá năng lực theo từng cấp bậc
│   │   └── onboarding-30-60-90-day-plan.md      # Lộ trình mục tiêu 3 tháng đầu cho nhân sự mới
│   ├── risk-governance/                         # Quản trị rủi ro hệ thống và quy trình
│   │   ├── technical-debt-log.md                # Sổ cái theo dõi nợ kỹ thuật (Tech Debt)
│   │   ├── dependency-license-audit.md          # Biểu mẫu kiểm toán giấy phép thư viện bên thứ 3
│   │   └── incident-post-mortem-template.md     # Biểu mẫu mổ xẻ nguyên nhân sau sự cố Production
├── .editorconfig                                # Cấu hình đồng nhất định dạng text (Tab/Space) cho IDE
├── .gitignore                                   # Danh sách các file/thư mục tuyệt đối không commit
├── LICENSE                                      # Bản quyền pháp lý sử dụng repo (ví dụ: MIT License)
└── README.md                                    # Bản đồ hướng dẫn cách sử dụng chính cái Repo Template này
