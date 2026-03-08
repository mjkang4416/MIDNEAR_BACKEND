diff --git a//Users/kangminjeong/Documents/Playground/MIDNEAR_BACKEND_README.md b//Users/kangminjeong/Documents/Playground/MIDNEAR_BACKEND_README.md
new file mode 100644
--- /dev/null
+++ b//Users/kangminjeong/Documents/Playground/MIDNEAR_BACKEND_README.md
@@ -0,0 +1,198 @@
+# MIDNEAR BACKEND
+
+MIDNEAR 쇼핑몰 서비스의 백엔드 API 서버입니다.  
+상품 탐색부터 주문/결제, 배송, 교환/반품, 리뷰, 관리자 운영까지 커머스 핵심 플로우를 담당합니다.
+
+## 1. 프로젝트 소개
+
+- 프로젝트명: MIDNEAR (커머스 백엔드)
+- 기간: 2024.11 ~ 2025.02
+- 역할: 백엔드 파트장/PM
+- 팀 구성: Backend 3 / Frontend 4 / Designer 1
+
+### 주요 기능
+
+- 상품/카테고리/옵션/재고 관리
+- 장바구니/주문/결제/쿠폰/포인트 처리
+- 배송지/배송 상태 관리
+- 교환/반품/취소/구매확정 플로우
+- 리뷰/문의/공지/매거진 운영
+- 관리자 콘솔(주문·배송·통계·고객지원)
+
+## 2. 서비스 화면
+
+> 아래 이미지 파일은 `docs/images` 경로에 업로드해서 사용하면 됩니다.
+
+### 메거진
+
+![메거진 화면](docs/images/magazine.png)
+
+### 상품 리스트
+
+![상품 리스트 화면](docs/images/shop-product-list.png)
+
+### 상품 상세
+
+![상품 상세 화면](docs/images/product-detail.png)
+
+## 3. 기술 스택
+
+### Backend
+
+- Java 17
+- Spring Boot 3.4.1
+- Spring Security
+- Spring Validation
+- MyBatis 3.0.3
+- JWT (jjwt 0.11.5)
+
+### Database / Cache
+
+- MySQL
+- Oracle JDBC (legacy/연동 용도)
+- Redis
+
+### Infra / DevOps
+
+- AWS S3
+- Jenkins
+- (운영 환경) EC2 / RDS / ALB
+
+### Tools
+
+- Swagger (springdoc-openapi)
+- log4jdbc
+- Actuator
+
+## 4. 아키텍처 설계
+
+```mermaid
+flowchart LR
+    U[User/Admin] --> FE[Frontend]
+    FE --> API[Spring Boot API Server]
+    API --> SEC[Spring Security + JWT]
+    API --> SVC[Service Layer]
+    SVC --> MB[MyBatis Mapper]
+    MB --> DB[(MySQL)]
+    API --> REDIS[(Redis)]
+    API --> S3[(AWS S3)]
+    API --> EXT[External APIs]
+    EXT --> PAY[Payment]
+    EXT --> SMS[SMS]
+    EXT --> MAIL[Email]
+```
+
+## 5. 프로젝트 구조도
+
+```text
+src/main/java/com/midnear/midnearshopping
+├── config                  # Security/CORS/Redis/S3/Swagger 설정
+├── controller              # 사용자 API + 관리자 API
+│   └── productManage       # 주문/배송/교환/반품/취소/구매확정 관리
+├── service                 # 도메인별 비즈니스 로직
+│   ├── order               # 주문/결제
+│   ├── product             # 상품
+│   ├── productManagement   # 관리자 주문/배송/클레임
+│   ├── delivery            # 배송지/배송정책
+│   ├── review              # 리뷰
+│   ├── inquirie            # 문의
+│   ├── oauth               # 소셜 로그인
+│   └── disruptive          # 이상 고객 관리
+├── mapper                  # MyBatis Mapper Interface
+├── domain
+│   ├── dto                 # 요청/응답 DTO
+│   ├── vo                  # DB 매핑 VO
+│   └── enums               # 상태/타입 enum
+├── jwt                     # JWT 유틸/필터
+└── exception               # 공통 응답/에러 처리
+
+src/main/resources
+├── mapper                  # MyBatis XML 쿼리
+├── project.sql             # DB 스키마
+├── logback.xml
+└── log4jdbc.log4j2.properties
+```
+
+## 6. 데이터베이스 설계
+
+### 핵심 엔터티
+
+- 사용자: `users`
+- 상품: `products`, `product_colors`, `sizes`, `product_images`, `categories`
+- 주문/결제: `orders`, `order_products`, `payments`
+- 장바구니: `cart`, `cart_products`
+- 혜택: `coupon`, `user_coupon`, `point`
+- 배송: `delivery_address`
+- 후기/문의: `reviews`, `review_images`, `inquiries`
+
+### 스키마 이미지
+
+![DB 스키마](docs/images/midnear-db-schema.png)
+
+## 7. API 도메인 요약
+
+- User/Auth: 회원가입, 로그인, JWT, 소셜 로그인(OAuth2), 이메일/문자 인증
+- Product: 상품 목록/상세, 카테고리, 색상/사이즈, 코디 상품
+- Order: 주문 생성, 결제, 주문 조회
+- Claim: 취소/교환/반품/구매확정
+- Delivery: 배송지/배송정보 관리
+- Review: 리뷰/리뷰 이미지
+- Admin: 상품/주문/배송/통계/문의/공지/매거진 관리
+
+## 8. 실행 방법
+
+### 사전 요구사항
+
+- JDK 17
+- MySQL
+- Redis (옵션/환경에 따라 필수)
+
+### 실행
+
+```bash
+./gradlew clean build
+./gradlew bootRun
+```
+
+## 9. 환경 변수 (예시)
+
+```bash
+DB_URL=
+DB_USERNAME=
+DB_PASSWORD=
+
+JWT_SECRET=
+
+REDIS_HOST=
+REDIS_PORT=
+
+AWS_ACCESS_KEY_ID=
+AWS_SECRET_ACCESS_KEY=
+AWS_REGION=
+AWS_S3_BUCKET=
+
+MAIL_USERNAME=
+MAIL_PASSWORD=
+
+COOLSMS_API_KEY=
+COOLSMS_API_SECRET=
+```
+
+## 10. 트러블슈팅/개선 포인트
+
+- 관리자 콘솔 다중 조건 검색 안정화 (동적 쿼리 정리)
+- 주문/환불/포인트/쿠폰 처리 정합성 강화 (트랜잭션 경계 명확화)
+- 배포 자동화 기반 운영 리드타임 단축 (Jenkins CI/CD)
+
+---
+
+## 이미지 파일 배치 가이드
+
+README 이미지가 보이도록 아래 경로/파일명으로 업로드하세요.
+
+```text
+docs/images/magazine.png
+docs/images/shop-product-list.png
+docs/images/product-detail.png
+docs/images/midnear-db-schema.png
+```
