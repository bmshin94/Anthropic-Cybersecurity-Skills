# Anthropic Cybersecurity Skills — 분석 & 수익화 정리 (한국어)

> 저장소: https://github.com/mukul975/Anthropic-Cybersecurity-Skills
> (현재 작업본은 포크: https://github.com/bmshin94/Anthropic-Cybersecurity-Skills)
> 정리일: 2026-09-28

---

## 1. 이게 뭐하는 저장소인가

**AI 에이전트에게 사이버보안 전문가의 "업무 매뉴얼"을 통째로 먹여주는 지식 라이브러리.**
코드 실행 프로그램이 아니라 **문서(글) 모음**이 핵심이다.

### 기본 정보 (실측)
| 항목 | 내용 |
|---|---|
| 원본 저장소 | `mukul975/Anthropic-Cybersecurity-Skills` |
| 성격 | 커뮤니티 프로젝트 (Anthropic 공식 아님) |
| 스킬 개수 | 818개 (`skills/` 하위 디렉토리 818, `SKILL.md` 818) |
| 보안 도메인 | 34개 |
| 프레임워크 매핑 | 6개 (MITRE ATT&CK, NIST CSF 2.0, MITRE ATLAS, D3FEND, NIST AI RMF, MITRE F3) |
| 파이썬 스크립트 | 1,096개 |
| 참조 문서 | 1,456개 |
| 표준 | agentskills.io 오픈 표준 |
| 라이선스 | Apache-2.0 (합법·허가된 용도만) |

### 폴더 구조
```
Anthropic-Cybersecurity-Skills/
├── README.md              # 프로젝트 소개
├── index.json             # 818개 스킬 색인 (자동 생성, ~447KB)
├── AGENTS.md              # AI 에이전트용 사용 규칙
├── CONTRIBUTING.md/SCOPE.md
├── skills/                # ★핵심★ 818개 스킬 폴더
│   └── <스킬이름>/
│       ├── SKILL.md       # YAML 헤더 + 마크다운 절차서
│       ├── scripts/       # 동작하는 파이썬 도구
│       ├── references/    # 표준·워크플로우 심화 문서
│       └── assets/        # 리포트 템플릿
├── mappings/              # 프레임워크 커버리지 매핑
├── tools/                 # 검증·색인 생성 스크립트
└── docs/                  # 추가 문서
```

### 스킬 하나의 구성
`SKILL.md`(YAML 프론트매터 + When to Use/Prerequisites/Instructions/Examples 본문)
+ `scripts/agent.py`(실제 동작 도구) + `references/`(심화) + `assets/`(템플릿).

예) `detecting-sql-injection-via-waf-logs`는 WAF 로그를 받아 `UNION SELECT`,
`OR 1=1`, `SLEEP()` 등 15개+ SQLi 패턴을 정규식으로 탐지하고, 동일 IP 공격을
"캠페인"으로 묶어 JSON 리포트를 생성한다.

### 어떨 때 쓰나
- SOC/보안관제, 위협 헌팅, SIEM 상관분석
- 디지털 포렌식(DFIR), 메모리/디스크 분석
- 레드팀/모의해킹 (합법·서면 허가 전제)
- 클라우드 보안, AI 보안(LLM 레드티밍/프롬프트 인젝션)
- 컴플라이언스(ISO 27001, NIST RMF) 매핑 근거

### 나에게 주는 도움
- AI 에이전트에 붙이면 즉시 "보안 분석가 두뇌"가 생긴다.
- 818개 실무 절차서 + 동작 코드가 공개된 학습 자료.
- 각 스킬이 MITRE/NIST 실제 ID에 매핑돼 보고서 근거로 인용 가능.

> ⚠️ 공격/이중용도 기술 포함 → 본인 소유이거나 서면 허가받은 시스템에만 사용.

---

## 2. 쉬운 비유

- **AI 에이전트** = 요리는 되는데 레시피가 없는 신입 요리사.
- **이 저장소** = 미슐랭 셰프의 레시피북 818장 (+ 자동 조리기구 = 파이썬 스크립트).
- 신입에게 레시피북을 주면 → 갑자기 시니어처럼 일관되게 분석한다.

핵심 3가지
1. 코드가 아니라 "문서(글)"가 주인공. 사람·AI 둘 다 읽는 매뉴얼.
2. AI는 818개 제목만 가볍게 훑고(각 ~30토큰) 필요한 것만 펼침(500~2000토큰) → 느리지 않음(점진적 공개).
3. 모든 스킬에 국제 표준 번호표(MITRE/NIST)가 붙어 근거 인용이 쉬움.

---

## 3. 질문 답변

**설치/사용법**
```bash
npx skills add mukul975/Anthropic-Cybersecurity-Skills     # 추천
# 또는
git clone https://github.com/mukul975/Anthropic-Cybersecurity-Skills.git
```
클로드 코드는 `.claude-plugin/`으로 플러그인화 가능. 커서/코파일럿/코덱스 등
agentskills.io 지원 도구는 클론만 해두면 자동 스캔·로드. 사람은 `SKILL.md`를
직접 읽고 `scripts/agent.py`를 실행.

**플러그인? 스킬? MCP?**
- 본질은 **스킬**(agentskills.io 표준 818개).
- 동시에 **플러그인**으로도 패키징됨(`.claude-plugin/plugin.json`).
- **MCP는 아님** (별도 실행 서버가 아니라 읽히는 마크다운 묶음).

**API 토큰?**
- 스킬 자체는 불필요(텍스트 파일).
- 스크립트가 외부 서비스(AWS WAF, VirusTotal 등)를 호출할 때, 또는 이 스킬을
  물려 돌리는 AI(클로드 API 등)를 쓸 때만 해당 키가 필요.

**왜 유명한가**
1) "largest" 규모(818개), 2) 이름에 "Anthropic"(검색·신뢰 유리, 단 비공식),
3) 6개 프레임워크 실제 매핑, 4) AI 에이전트/MCP 트렌드 중심, 5) 적극적 홍보
(설문·플레이그라운드·awesome-list 등재).

**로컬 에이전트 구축에 도움?**
매우 도움. `skills/`를 지식 베이스로 물리면 보안 전용 에이전트 완성.
점진적 공개 구조라 작은 컨텍스트에서도 효율적. `index.json`으로 RAG 색인도 용이.

**React/PHP로 만들 수 있나**
스킬 문서는 언어 무관(마크다운), 스크립트는 파이썬. 이를 활용하는 앱은 React/PHP로
충분히 제작 가능(검색 대시보드, 서빙 API, 회원제 사이트 등). 단, `agent.py` 실행은
파이썬 런타임 필요 → 프론트/오케스트레이션은 React/PHP, 실행은 파이썬 마이크로서비스
하이브리드가 현실적.

---

## 4. 수익화 아이디어 (상세)

핵심 전략: 스킬(콘텐츠)은 오픈소스라 그 자체로는 못 판다. 돈은 그 "위에" 얹는
① 실행 인프라(SaaS) ② 검증·큐레이션 ③ 통합·자동화 ④ 교육/지원 에서 나온다.

### Tier 1 — 바로 실현 가능
1. **SaaS "보안 에이전트 API"** — 818 스킬 + LLM을 묶어 "로그 넣으면 분석 리포트"를
   월 구독제로 판매. React 대시보드 + PHP/파이썬 백엔드. 타깃: 보안팀 없는 중소기업.
2. **프리미엄 스킬팩 / 마켓플레이스** — 기본 무료 위에 산업별 특화 검증 스킬팩
   (금융 F3 사기탐지, 의료 HIPAA 등)을 유료 판매.
3. **교육 콘텐츠** — "AI로 배우는 실전 보안 분석" 강의/부트캠프. 스킬 = 즉석 커리큘럼.

### Tier 2 — 중기
4. **컴플라이언스 리포트 자동화** — 프레임워크 매핑으로 조직별 ATT&CK/NIST 커버리지
   리포트 자동 생성 → 컨설팅·감사 대응 유료 툴.
5. **MSSP 백엔드** — 스킬셋 무장 에이전트를 1차 트리아지 자동화에 투입, 절감분 수익화.
6. **VS Code/커서 확장 (Pro)** — 코딩 중 보안 스킬 호출. 무료 + Pro 구독.

### Tier 3 — 확장/커뮤니티
7. **플레이그라운드 프리미엄** (README의 Casky.ai 모델).
8. **인증/뱃지 프로그램**.
9. **기업 온프렘 라이선스 + 지원 계약** (오픈소스 무료, 설치·커스터마이징·지원 유료).

---

## 5. 참고 링크
- 원본: https://github.com/mukul975/Anthropic-Cybersecurity-Skills
- 표준: https://agentskills.io
- 라이선스: Apache-2.0
