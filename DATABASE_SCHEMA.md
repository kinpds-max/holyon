# ⛪ 홀리온 (HolyOn) - 데이터베이스 스키마 설계 (Draft)

기독교 전용 데이팅 앱 '홀리온'을 위한 관계형 데이터베이스(RDBMS) 설계안입니다.

---

## 🏗️ 1. 핵심 테이블 구조

### 👤 Users (사용자 기본 정보)
| 컬럼명 | 타입 | 설명 |
| :--- | :--- | :--- |
| `id` | UUID (PK) | 고유 사용자 ID |
| `email` | String (Unique) | 로그인 이메일 |
| `phone` | String | 본인 인증된 휴대폰 번호 |
| `nickname` | String | 앱 내 닉네임 (예: 홀리한 꽃사슴) |
| `birth_date` | Date | 만나이 계산 및 연령 제한 |
| `gender` | Enum | 남성, 여성 |
| `location` | String | 거주 지역 (시/도 단위) |
| `mbti` | String | MBTI (예: ENFP, INFJ) |
| `personality_type` | String | 홀리온 성격 테스트 결과 유형 |
| `church_info` | JSON | { name: '교회명', denomination: '교단', role: '교사/찬양팀 등' } |
| `purity_pledge` | Boolean | 혼전순결 서약 여부 (선택) |
| `is_verified` | Boolean | 교회 출석 및 신앙 공동체 인증 여부 |

### 📖 Faith_Profiles (43가지 신앙 질문)
*홀리온의 핵심인 43가지 심층 질문에 대한 답변 데이터*
| 컬럼명 | 타입 | 설명 |
| :--- | :--- | :--- |
| `user_id` | UUID (FK) | 사용자 ID |
| `q1_worship` | Text | 나에게 예배란? |
| `q2_vision` | Text | 하나님 안에서 나의 비전 |
| `q3_habits` | Text | 경건의 시간(QT) 습관 |
| `q4_marriage` | Text | 기독교적 결혼관 |
| `favorite_verse` | String | 가장 좋아하는 성경 구절 |
| `favorite_praise` | String | 내 영혼의 찬양 (CCM/찬송가) |
| `hobbies` | String | 여호와께서 주신 달란트/취미 |
| `testimony` | Text | 나의 신앙 간증 (Faith Journey) |
| ... | ... | (총 43개 질문 컬럼) |

### ✨ Daily_Matches (하루 두 번 추천)
| 컬럼명 | 타입 | 설명 |
| :--- | :--- | :--- |
| `id` | UUID (PK) | 추천 고유 ID |
| `user_id` | UUID (FK) | 추천을 받는 사용자 |
| `target_id` | UUID (FK) | 추천된 상대방 |
| `match_slot` | Enum | AM (오전), PM (오후) |
| `match_date` | Date | 추천된 날짜 |
| `status` | Enum | Pending, Liked, Passed |

### 💬 Conversations (채팅 및 연결)
| 컬럼명 | 타입 | 설명 |
| :--- | :--- | :--- |
| `id` | UUID (PK) | 채팅방 ID |
| `user1_id` | UUID (FK) | 참여자 1 |
| `user2_id` | UUID (FK) | 참여자 2 (서로 Like 시 생성) |
| `is_church_revealed` | Boolean | 교회 정보 공개 여부 (대화 진행도에 따라 자동 활성화) |
| `created_at` | Timestamp | 대화 시작 시간 |

---

## 🛡️ 2. 안전/신뢰 시스템 (Safety)

1.  **Denomination Whitelist**: 주요 교단(통합, 합동, 기감 등) 리스트를 관리하여 이단/사이비 원천 차단.
2.  **Acquaintance Blocking**: 연락처 기반 지인 차단 기능 (Privacy).
3.  **Real-time Monitoring**: 24시간 운영자 신고 처리 및 악성 유저 차단.

---

## 🚀 3. 다음 단계 제안
이 스키마를 바탕으로 **Mock API 명세서**를 작성하거나, 웹에서 실제 데이터를 시뮬레이션해볼 수 있도록 **Local Storage 기반의 프로토타입**을 확장할 수 있습니다.
