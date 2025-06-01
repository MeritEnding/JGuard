# 엔터티 설계
1. InterviewSession (면접 세션)
sessionId: String (UUID, 세션 식별자)

interviewStarted: boolean

interviewEnded: boolean

resumeSummary: String (이력서 요약)

currentQuestion: String (현재 질문)

conversation: List<ChatTurn> (질문-답변 내역)

evaluation: List<Evaluation> (피드백 리스트)

riskResult: String (최종 위험도 분석 결과)

기타 내부 상태 (필요하면 Map<String, Object> 형태로 상태 저장 가능)

2. ChatTurn (질문-답변 기록 단위)
questionId: int (질문 번호)

strategyName: String (질문 전략 이름)

question: String

answer: String

speaker: enum (USER, AI)

3. Evaluation (피드백)
criteria: String

grade: String (등급)

feedbackText: String

# API 명세서

1. 이력서 업로드 및 면접 시작
URL: /api/interview/upload

Method: POST

Request:

파일 업로드 (PDF 또는 DOCX)

Multipart/form-data 형식

Response:

세션 ID (면접 세션 구분용)

면접 시작 여부 (true)

면접 종료 여부 (false)

첫 질문 텍스트

대화 내역 (최초 AI 질문 포함)

설명:
사용자가 이력서 파일을 업로드하면 면접 세션이 생성되고, 첫 질문이 반환됩니다.

2. 사용자 답변 제출 및 인터뷰 진행
URL: /api/interview/chat

Method: POST

Request:

JSON 형식

필드:

sessionId (면접 세션 식별용)

userInput (사용자 답변 텍스트)

Response:

세션 ID

면접 시작 여부

면접 종료 여부

다음 질문 텍스트 (면접 종료 시 null)

대화 내역 (최근 사용자 답변과 AI 질문/요약 포함)

설명:
사용자가 답변을 제출하면 서버가 상태를 갱신하고 다음 질문 또는 최종 요약을 응답합니다.

3. 면접 상태 조회 (선택 사항)
URL: /api/interview/status

Method: GET

Request:

Query Parameter: sessionId (면접 세션 식별용)

Response:

세션 ID

면접 시작 여부

면접 종료 여부

현재 질문 (또는 null)

전체 대화 내역

설명:
클라이언트가 현재 면접 세션 상태와 대화 내역을 조회할 수 있습니다.

공통 사항
세션 관리:
모든 API 호출에서 sessionId를 필수로 사용하여 면접 상태를 구분합니다.

오류 처리:

파일 미첨부 또는 형식 오류 시 400 Bad Request

존재하지 않는 sessionId 사용 시 404 Not Found

서버 내부 오류 발생 시 500 Internal Server Error

파일 제한:

최대 파일 크기 제한 적용 (예: 10MB)

지원 파일 형식: PDF, DOCX
