# 🌐 홀리온 (HolyOn) - API 명세서 (Mock)

프론트엔드와 백엔드 간의 핵심 상호작용을 위한 RESTful API 설계 초안입니다.

---

## 🔐 1. 인증 및 프로필 (Auth & Profile)

### `POST /api/v1/auth/signup`
-   **설명**: 회원가입 및 본인 인증 (기독교 교단 확인 포함)
-   **Body**: `{ email, password, phone, church_info: { name, denomination } }`

### `GET /api/v1/user/profile`
-   **설명**: 내 프로필 및 43가지 신앙 질문 답변 조회
-   **Response**: `{ nickname, questions: [...], is_verified: true }`

---

## ✨ 2. 매칭 및 추천 (Matching)

### `GET /api/v1/matches/daily`
-   **설명**: 하루 두 번(오전/오후)의 추천 상대 리스트 조회
-   **Response**: 
    ```json
    {
      "slot": "PM",
      "recommended_user": {
        "id": "user-123",
        "nickname": "홀리한 꽃사슴",
        "age": 27,
        "church_role": "교사",
        "favorite_verse": "빌립보서 4:13",
        "favorite_praise": "오직 예수 뿐이네 (소진영)",
        "hobbies": ["CCM 작사", "필사", "테니스"],
        "testimony": "방황하던 저를 만나주신 주님...",
        "top_answers": ["나에게 예배는 삶의 원동력입니다", "..."]
      }
    }
    ```

### `POST /api/v1/matches/{target_id}/like`
-   **설명**: 상대방에게 '좋아요' 보내기
-   **Response**: `{ "status": "success", "is_mutual": true/false }` (서로 선택 시 대화방 생성)

---

## 💬 3. 메시지 (Messaging)

### `GET /api/v1/chats`
-   **설명**: 활성화된 채팅방 리스트 조회 (서로 수락된 경우만)

### `POST /api/v1/chats/{chat_id}/messages`
-   **설명**: 메시지 전송 (홀리온은 미전달 피드백 등 투명한 관리)
-   **Body**: `{ content: "안녕하세요, 반갑습니다!" }`

---

## 🏹 4. 핵심 로직 및 정책 (Core Logic)
-   **신앙 가치관 기반 추천**: 답변(43개 질문)의 유사도 및 신앙적 일치도 알고리즘 적용.
-   **교회 정보 단계적 공개**: 대화 7회 이상 상호 교환 시 '출석 교회' 자동 공개.
-   **이단 차단**: 블랙리스트 교단 가입자 자동 차단 및 실시간 운영자 신고 처리.
