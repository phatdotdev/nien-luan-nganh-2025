# JOB SERVICE — Nền tảng tuyển dụng việc làm CNTT tích hợp AI

> **Đồ án Niên luận ngành** — Xây dựng hệ thống kết nối ứng viên, nhà tuyển dụng và chuyên gia đánh giá CV; tích hợp gợi ý việc làm bằng mô hình Hybrid Recommendation và hỗ trợ phân tích bằng Google Gemini.

![React](https://img.shields.io/badge/React-19.1-61DAFB?logo=react&logoColor=black)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5.6-6DB33F?logo=springboot&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-AI_Service-009688?logo=fastapi&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.4-4479A1?logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)

## 1. Tổng quan đề tài

JOB SERVICE là một hệ thống tuyển dụng việc làm CNTT theo kiến trúc nhiều dịch vụ. Hệ thống không chỉ hỗ trợ đăng tin và ứng tuyển mà còn giải quyết toàn bộ vòng đời tuyển dụng: xây dựng hồ sơ/CV, tìm kiếm việc làm, theo dõi trạng thái hồ sơ, trao đổi thời gian thực, nhờ chuyên gia đánh giá CV, quản trị dữ liệu và cá nhân hóa nội dung bằng AI.

Điểm nổi bật của đề tài:

- Phân quyền đầy đủ cho **Ứng viên, Nhà tuyển dụng, Chuyên gia và Quản trị viên**.
- Xác thực bằng **JWT HS512 lưu trong HttpOnly Cookie**, có access token và refresh token.
- Quản lý vòng đời tin tuyển dụng và hồ sơ ứng tuyển theo trạng thái.
- Chat và thông báo thời gian thực bằng **WebSocket/STOMP**.
- Hệ thống đánh giá CV giữa ứng viên và chuyên gia.
- Gợi ý việc làm, doanh nghiệp và việc làm tương tự bằng **SBERT + ALS + Popularity**.
- Gợi ý top ứng viên/top công việc bằng **Google Gemini 2.5 Flash**.
- Xuất CV Word và danh sách ứng viên Excel ngay trên frontend.
- Có Docker Compose để khởi động toàn bộ hệ thống bằng một lệnh.

## 2. Những chức năng đã hoàn thành

### 2.1. Chức năng chung và xác thực

- Đăng ký tài khoản ứng viên hoặc nhà tuyển dụng; đăng nhập, đăng xuất và tự làm mới phiên đăng nhập.
- Phân quyền ở cả giao diện và API bằng Spring Security/`@PreAuthorize`.
- Xem, cập nhật thông tin cá nhân; đổi mật khẩu; tải ảnh đại diện và ảnh bìa.
- Tìm kiếm, xem trang cá nhân ứng viên, nhà tuyển dụng và chuyên gia.
- Lưu file upload trên server và ánh xạ thành tài nguyên công khai.

### 2.2. Ứng viên — `JOBSEEKER`

- Tạo và quản lý nhiều CV/hồ sơ.
- Quản lý học vấn, kinh nghiệm, kỹ năng, dự án, chứng chỉ, giải thưởng và hoạt động.
- Xem trước CV và xuất CV thành file Word.
- Tìm kiếm việc làm theo tiêu chí, xem chi tiết, lượt xem, yêu thích và việc làm tương tự.
- Nộp hồ sơ kèm tài liệu, xem lịch sử ứng tuyển và tiến trình xử lý.
- Bổ sung tài liệu hoặc hủy hồ sơ khi cần.
- Nhận danh sách việc làm/doanh nghiệp được cá nhân hóa.
- Gửi yêu cầu để chuyên gia đánh giá CV và xem kết quả đánh giá.
- Nhắn tin với người dùng khác và nhận thông báo thời gian thực.

### 2.3. Nhà tuyển dụng — `RECRUITER`

- Quản lý hồ sơ công ty, ảnh đại diện và ảnh bìa.
- Tạo tin tuyển dụng dạng nháp, xem trước, chỉnh sửa, đăng, đóng hoặc hoàn tất tin.
- Đính kèm hình ảnh và tài liệu cho tin tuyển dụng.
- Xem danh sách ứng viên theo từng bài đăng và chi tiết hồ sơ ứng viên.
- Cập nhật trạng thái tuyển dụng, yêu cầu bổ sung tài liệu và gửi lời mời ứng tuyển.
- Xuất danh sách ứng viên ra Excel và tải CV ứng viên.
- Dùng Gemini để gợi ý các ứng viên phù hợp với tin tuyển dụng.
- Theo dõi thống kê hoạt động tuyển dụng.

### 2.4. Chuyên gia — `EXPERT`

- Quản lý hồ sơ và các lĩnh vực chuyên môn.
- Tiếp nhận danh sách yêu cầu đánh giá CV.
- Chấm điểm và nhận xét chi tiết về tổng quan, kinh nghiệm, kỹ năng, học vấn và đề xuất cải thiện.
- Xem lại lịch sử và chi tiết các đánh giá đã hoàn thành.

### 2.5. Quản trị viên — `ADMIN`

- Dashboard thống kê tổng quan và biểu đồ xu hướng tin tuyển dụng/doanh nghiệp.
- Quản lý tài khoản ứng viên, nhà tuyển dụng và chuyên gia.
- Tạo tài khoản theo vai trò, khóa/mở khóa hoặc xóa tài khoản.
- Xác minh nhà tuyển dụng.
- Quản lý bài đăng tuyển dụng.
- Quản lý dữ liệu dùng chung: địa điểm, thẻ kỹ năng, trường học và chứng chỉ.
- Tạo và gửi thông báo hệ thống.

### 2.6. AI và hệ gợi ý

Hệ thống triển khai một FastAPI microservice riêng, đọc chung dữ liệu MySQL với backend:

1. **Content-Based Filtering:** ghép tiêu đề, mô tả, yêu cầu, quyền lợi, kinh nghiệm, mức lương và địa điểm thành văn bản; dùng `all-MiniLM-L6-v2` tạo vector 384 chiều và tính cosine similarity với hồ sơ ứng viên.
2. **Collaborative Filtering:** dùng ALS từ thư viện `implicit`; tín hiệu được gán trọng số `apply = 10`, `save = 5`, `view = 1`.
3. **Popularity:** chuẩn hóa độ phổ biến từ tổng lượt xem, lưu và ứng tuyển.
4. **Hybrid Ranking:** khi đủ lịch sử tương tác, điểm cuối kết hợp Content-Based 30%, Collaborative Filtering 50% và Popularity 20%; người dùng mới được fallback sang Content-Based/độ phổ biến.
5. **Company Recommendation:** gom nhóm các công việc phù hợp theo nhà tuyển dụng để đề xuất doanh nghiệp.
6. **Similar Jobs:** so sánh vector công việc-công việc và chỉ lấy kết quả vượt ngưỡng tương đồng.
7. **Gemini:** backend tạo prompt từ CV/tin tuyển dụng để gợi ý top công việc hoặc top ứng viên; đây là chức năng tùy chọn, cần API key.

## 3. Kiến trúc hệ thống

```mermaid
flowchart LR
    U[Trình duyệt] -->|HTTP / WebSocket| FE[React 19 + Vite<br/>Nginx]
    FE -->|REST API + HttpOnly Cookie| BE[Spring Boot 3<br/>Security + JPA + STOMP]
    FE <-->|STOMP realtime| BE
    BE --> DB[(MySQL 8.4)]
    BE -->|HTTP nội bộ| AI[FastAPI<br/>SBERT + ALS]
    AI --> DB
    BE -->|Tùy chọn| GM[Google Gemini API]
    BE --> FS[(Upload volume)]
    AI --> MD[(Model artifacts)]
```

Luồng chính:

- Frontend gọi API qua tiền tố `/api/v1`; Nginx/Vite proxy chuyển tiếp đến Spring Boot.
- Spring Boot đảm nhiệm nghiệp vụ, phân quyền, lưu trữ dữ liệu và giao tiếp WebSocket.
- FastAPI đọc dữ liệu việc làm, hồ sơ và tương tác từ MySQL để huấn luyện/xếp hạng.
- Spring Boot gọi FastAPI và ánh xạ danh sách ID được gợi ý thành dữ liệu đầy đủ trả về frontend.

## 4. Công nghệ sử dụng

| Thành phần | Công nghệ chính |
|---|---|
| Frontend | React 19, TypeScript, Vite, Tailwind CSS 4, React Router 7 |
| State/API | Redux Toolkit, RTK Query |
| Hiệu ứng/UI | Framer Motion, Lucide React, React Icons |
| Realtime | STOMP.js, WebSocket |
| Xuất dữ liệu | `docx`, ExcelJS, FileSaver |
| Backend | Java 21, Spring Boot 3.5.6, Spring Security, Spring Data JPA |
| Authentication | JWT HS512, OAuth2 Resource Server, HttpOnly Cookie |
| Mapping/tiện ích | MapStruct, Lombok, Jackson, Gson, Commons IO |
| AI service | Python 3.13, FastAPI, SQLAlchemy, Pydantic |
| Machine Learning | Sentence Transformers/SBERT, scikit-learn, implicit ALS, pandas, SciPy |
| Database | MySQL 8.4 |
| DevOps | Docker, Docker Compose, Nginx, Maven, npm, uv |

## 5. Cấu trúc thư mục

```text
job_service/
├── backend/                     # REST API, bảo mật và nghiệp vụ Spring Boot
│   ├── src/main/java/.../
│   │   ├── configuration/       # Security, CORS, JWT, WebSocket, static files
│   │   ├── controller/          # REST/STOMP controllers
│   │   ├── dto/                 # Request/response objects
│   │   ├── entity/              # JPA entities
│   │   ├── repository/          # Spring Data repositories
│   │   ├── service/             # Business logic
│   │   └── specification/       # Truy vấn tìm kiếm động
│   ├── src/main/resources/
│   │   └── application.yml      # Cấu hình qua biến môi trường
│   └── Dockerfile
├── frontend/                    # SPA React/TypeScript
│   ├── src/components/          # UI và component theo nghiệp vụ
│   ├── src/pages/               # Màn hình theo từng vai trò
│   ├── src/redux/               # Store, slices, RTK Query APIs
│   ├── src/context/             # WebSocket context/provider
│   ├── src/utils/               # Xuất Word/Excel, format, auth
│   ├── nginx.conf               # SPA fallback + reverse proxy
│   └── Dockerfile
├── recommend/                   # Microservice gợi ý FastAPI
│   ├── app/api/                 # Recommendation/training endpoints
│   ├── app/db/                  # SQLAlchemy models
│   ├── app/services/            # Hybrid Recommendation Engine
│   ├── model_artifacts/         # Mô hình/metadata đã huấn luyện
│   ├── setup.sql                # Bảng vector AI
│   └── Dockerfile
├── docker-compose.yml           # Điều phối toàn hệ thống
└── .env.example                 # Mẫu cấu hình môi trường
```

## 6. Chạy toàn bộ dự án bằng Docker Compose

Đây là cách được khuyến nghị vì không cần cài riêng Java, Node.js, Python hay MySQL.

### 6.1. Yêu cầu

- Docker Desktop hoặc Docker Engine có Docker Compose v2.
- Tối thiểu khoảng 8 GB RAM trống cho lần build AI service.
- Internet ở lần build đầu để tải dependency và mô hình SBERT.

### 6.2. Chuẩn bị biến môi trường

Tại thư mục gốc dự án:

```powershell
Copy-Item .env.example .env
```

Linux/macOS:

```bash
cp .env.example .env
```

Mở `.env` và đổi mật khẩu MySQL/JWT key. Nếu muốn dùng Gemini, điền `GEMINI_API_KEY`; để trống thì các chức năng cốt lõi và hệ gợi ý FastAPI vẫn hoạt động.

### 6.3. Build và khởi động

```bash
docker compose up --build -d
```

Lần đầu có thể lâu do image AI phải tải Python packages, PyTorch và mô hình `all-MiniLM-L6-v2`. Theo dõi trạng thái:

```bash
docker compose ps
docker compose logs -f
```

Các địa chỉ sau khi khởi động:

| Dịch vụ | Địa chỉ |
|---|---|
| Giao diện người dùng | <http://localhost:3000> |
| Spring Boot API | <http://localhost:8080/api/v1> |
| FastAPI health check | <http://localhost:8000> |
| FastAPI Swagger UI | <http://localhost:8000/docs> |
| MySQL | `localhost:3306` |

Tài khoản quản trị mặc định được backend tạo ở lần chạy đầu:

```text
Email:    admin.do@gmail.com
Password: 12345678
```

> Hãy đổi thông tin mặc định và các secret trước khi triển khai thật.

### 6.4. Khởi tạo dữ liệu gợi ý

Spring JPA tự tạo/cập nhật schema nghiệp vụ; `setup.sql` tạo bảng `ai_job_vectors`. Sau khi đã có tin tuyển dụng và dữ liệu tương tác, huấn luyện lại mô hình:

```bash
curl -X POST http://localhost:8000/api/v1/admin/retrain
curl -X POST http://localhost:8000/api/v1/admin/retrain-cf
```

- `retrain`: tạo lại embedding SBERT cho các tin tuyển dụng.
- `retrain-cf`: học lại ALS từ lượt xem, lượt lưu và lượt ứng tuyển.
- Nên chạy `retrain` sau khi thêm/cập nhật nhiều tin; chạy `retrain-cf` định kỳ khi đã có đủ tương tác.

### 6.5. Dừng hoặc làm sạch

```bash
docker compose down
```

Lệnh trên giữ nguyên database, file upload và model. Chỉ khi muốn **xóa toàn bộ dữ liệu volume** mới dùng:

```bash
docker compose down -v
```

## 7. Chạy thủ công để phát triển

### 7.1. Yêu cầu cục bộ

- Java JDK 21.
- Node.js 20+ và npm.
- Python 3.13 và `uv`.
- MySQL 8.x, database `job_service`.

### 7.2. Database

Tạo database và bảng AI:

```sql
CREATE DATABASE job_service CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE job_service;

CREATE TABLE IF NOT EXISTS ai_job_vectors (
    job_id BINARY(16) PRIMARY KEY,
    vector JSON
);
```

Mặc định backend dùng `root/root`. Có thể đặt các biến `SPRING_DATASOURCE_URL`, `SPRING_DATASOURCE_USERNAME` và `SPRING_DATASOURCE_PASSWORD` để thay đổi.

### 7.3. Backend

Trên mọi hệ điều hành (với Maven đã cài và có trong `PATH`):

```text
cd backend
mvn spring-boot:run
```

Các cấu hình quan trọng nằm tại `backend/src/main/resources/application.yml` và đều có thể ghi đè bằng biến môi trường:

- `JWT_ACCESS_KEY`, `JWT_REFRESH_KEY`
- `GEMINI_API_KEY`
- `RECOMMENDATION_SERVICE_URL`
- `FILE_UPLOAD_DIR`
- `CORS_ALLOWED_ORIGINS`

### 7.4. Recommendation service

Tạo file `recommend/.env`:

```env
DATABASE_URL=mysql+pymysql://root:root@localhost:3306/job_service?charset=utf8mb4
MODEL_DIMENSIONS=384
```

Khởi động:

```powershell
cd recommend
uv sync
uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### 7.5. Frontend

```powershell
cd frontend
npm ci
npm run dev
```

Truy cập <http://localhost:3000>. Vite đã proxy `/api` và WebSocket đến `http://localhost:8080`, vì vậy không cần sửa URL trong mã nguồn. Khi backend ở máy khác, tạo `frontend/.env.local`:

```env
VITE_API_URL=http://localhost:8080/api/v1
VITE_WS_URL=ws://localhost:8080/api/v1/ws
```

## 8. Nhóm API chính

Tất cả API Spring Boot có base URL `/api/v1`.

| Nhóm | Endpoint tiêu biểu | Mục đích |
|---|---|---|
| Authentication | `/auth/signup`, `/auth/login`, `/auth/refresh`, `/auth/logout` | Xác thực và phiên đăng nhập |
| Users | `/users/**` | Hồ sơ, quản trị tài khoản, công ty, chuyên gia |
| Postings | `/postings/**` | Tin tuyển dụng, tìm kiếm, like/view, trạng thái |
| Resumes | `/resumes`, `/educations`, `/experiences`, `/skills`, ... | CV và thành phần hồ sơ |
| Applications | `/applications/**` | Nộp và xử lý hồ sơ ứng tuyển |
| Feedback | `/feedback/**` | Yêu cầu/chấm đánh giá CV |
| Communication | `/messages/**`, `/notifications/**`, `/ws` | Chat, thông báo, realtime |
| Resources | `/resource/**`, `/upload/**`, `/files/**` | Danh mục và upload file |
| Statistics | `/statistics/**` | Dashboard và biểu đồ |
| Recommendation | `/recommend/postings`, `/recommend/recruiters` | Kết quả gợi ý qua Spring Boot |
| Gemini | `/{resumeId}/job-suggestions`, `/{postId}/candidate-suggestions` | Gợi ý bằng mô hình Gemini |

FastAPI cung cấp:

| Method | Endpoint | Mục đích |
|---|---|---|
| `GET` | `/` | Health check |
| `POST` | `/api/v1/recommendations/` | Gợi ý việc làm hybrid |
| `POST` | `/api/v1/recommendations/companies` | Gợi ý doanh nghiệp |
| `POST` | `/api/v1/recommendations/similar-jobs` | Việc làm tương tự |
| `POST` | `/api/v1/admin/retrain` | Huấn luyện lại Content-Based |
| `POST` | `/api/v1/admin/retrain-cf` | Huấn luyện lại Collaborative Filtering |

## 9. Mô hình dữ liệu nghiệp vụ

Các nhóm thực thể chính:

- **Người dùng:** `User`, `JobSeeker`, `Recruiter`, `Expert`, `Expertise`.
- **Tuyển dụng:** `JobPosting`, `Application`, trạng thái bài đăng và trạng thái ứng tuyển.
- **CV:** `Resume`, `Education`, `WorkExperience`, `Skill`, `Project`, `Certification`, `Award`, `Activity`.
- **Tài nguyên:** `Location`, `Tag`, `School`, `Certificate`, `Image`, `Document`, `Link`.
- **Đánh giá:** `FeedbackRequest`, `FeedbackReview`.
- **Giao tiếp:** `Message`, `Notification`.
- **AI:** `ai_job_vectors`, bảng like/view và dữ liệu application dùng để huấn luyện.

UUID được sử dụng làm khóa chính cho hầu hết thực thể; trên MySQL, Hibernate lưu UUID ở dạng `BINARY(16)`.

## 10. Kiểm thử và kiểm tra chất lượng

```bash
# Backend
cd backend
mvn test

# Frontend
cd frontend
npm run lint
npm run typecheck
npm run build

# Kiểm tra cấu hình Compose
docker compose config
```

`npm run build` tạo bundle chạy ứng dụng. `npm run typecheck` được tách riêng để tiếp tục rà soát các cảnh báo/lỗi kiểu dữ liệu còn tồn tại trong mã giao diện mà không chặn việc đóng gói Docker.

Quy trình kiểm thử thủ công đề xuất:

1. Đăng ký một ứng viên và một nhà tuyển dụng.
2. Đăng nhập admin để xác minh nhà tuyển dụng và tạo dữ liệu danh mục.
3. Nhà tuyển dụng tạo, xem trước và đăng tin.
4. Ứng viên tạo đầy đủ CV, tìm kiếm và nộp hồ sơ.
5. Nhà tuyển dụng xem hồ sơ, đổi trạng thái hoặc yêu cầu tài liệu.
6. Kiểm tra thông báo/chat realtime ở hai trình duyệt.
7. Tạo chuyên gia từ admin, gửi yêu cầu và hoàn tất đánh giá CV.
8. Gọi hai endpoint retrain rồi kiểm tra việc làm, công ty và việc làm tương tự được gợi ý.
9. Kiểm tra xuất CV Word và danh sách ứng viên Excel.

## 11. Điểm mạnh kỹ thuật của đồ án

- Tách AI thành microservice độc lập, backend không bị phụ thuộc trực tiếp vào thư viện ML nặng.
- Phân lớp rõ ràng: Controller → Service → Repository → Database; DTO/Mapper tách entity khỏi dữ liệu trả về.
- Tìm kiếm động qua JPA Specification.
- JWT cookie giảm việc lưu token trong JavaScript; refresh token duy trì phiên làm việc.
- RTK Query quản lý cache/API và tự xử lý refresh khi gặp `401`.
- Realtime sử dụng STOMP user queue cho tin nhắn và thông báo cá nhân.
- Hệ gợi ý có chiến lược cold-start và kết hợp nhiều tín hiệu thay vì chỉ dùng một thuật toán.
- Docker multi-stage build giúp tách môi trường build khỏi image chạy thực tế.
- Named volume bảo toàn database, file người dùng và model sau khi restart container.

## 12. Hạn chế và hướng phát triển

- Bổ sung test unit/integration và bộ dữ liệu seed tự động cho demo.
- Bảo vệ các endpoint retrain bằng API key hoặc mạng nội bộ.
- Đưa tác vụ huấn luyện sang background worker/scheduler thay vì chạy đồng bộ.
- Tự động cập nhật vector khi bài tuyển dụng được tạo hoặc chỉnh sửa.
- Chuẩn hóa cookie `Secure`/`SameSite` theo profile dev và production; triển khai HTTPS.
- Chuyển secret sang Docker Secret/Vault và thay tài khoản admin mặc định.
- Lưu file trên object storage như S3/MinIO khi triển khai nhiều instance.
- Thêm migration bằng Flyway/Liquibase thay cho `ddl-auto=update` ở production.
- Bổ sung monitoring, centralized logging, rate limiting và CI/CD.
- Đánh giá mô hình bằng Precision@K, Recall@K, NDCG@K và A/B testing.

---

**Kết quả:** đồ án đã hình thành một nền tảng tuyển dụng end-to-end có đầy đủ nghiệp vụ cho bốn vai trò, giao tiếp thời gian thực, quản lý CV/ứng tuyển, dashboard quản trị và hai lớp AI (hybrid recommender + Gemini), đồng thời có thể triển khai đồng nhất bằng Docker Compose.
