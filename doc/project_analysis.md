# AI 기반 상황 인식 무음 커뮤니케이션 지원 시스템
## 요구사항 분석서

| 항목 | 내용 |
|------|------|
| 문서번호 | [안서연]요구사항분석서_260515_Doc-001 |
| 작성일 | 2026-05-15 |
| 버전 | v1.0 |
| 소속 | 한국항공대학교 소프트웨어학과 |
| 팀명 | |
| 팀원 | |
| 교수 | |

---

## 제/개정 이력

| 버전 | 날짜 | 작성자 성명 | 제/개정사항 | 비고 |
|------|------|------------|------------|------|
| v1.0 | 2026-05-15 | 안서연 | 요구사항 분석서 최초 작성 | |

---

## 목차

1. 서론
   - 1.1 목적 및 범위
   - 1.2 용어 정의
   - 1.3 참조 문서
2. 시스템 개요
   - 2.1 소프트웨어 컨텍스트(Context)
   - 2.2 기능 분류 및 설명
3. 요구사항 명세
   - 3.1 정적 분석
   - 3.2 CRC 카드
   - 3.3 동적 분석
4. 인터페이스 분석
5. 제약사항
6. 요구사항 추적표
7. 참고문헌 및 부록

---

## 1. 서론

### 1.1 목적 및 범위

이 문서는 AI 기반 상황 인식 무음 커뮤니케이션 지원 시스템(SilentTalk)에서 각 요구사항이 무엇인지 조사하고, 정의하는 문서이다. 이 문서는 기능적, 비기능적, 인터페이스에 요구되는 사항들을 분석하고 명세한다.

### 1.2 용어 정의

| 용어 | 설명 |
|------|------|
| Computer Vision (CV) | 카메라 등 이미지/영상 입력으로부터 시각적 정보를 인식·분석하는 AI 기술 |
| NLP | 자연어 처리 (Natural Language Processing). 텍스트를 이해하고 생성하는 AI 기술 |
| AAC | 보완대체의사소통 (Augmentative and Alternative Communication). 언어 장애인을 위한 의사소통 지원 기술 |
| TTS | 텍스트 음성 변환 (Text-to-Speech). 텍스트를 음성으로 출력하는 기술 |
| 버튼 기반 인터페이스 | 타이핑 없이 미리 정의된 버튼을 선택하여 의사를 표현하는 UI 방식 |
| 상황 인식 | 카메라 입력으로 현재 장소 및 환경(카페, 병원 등)을 자동으로 파악하는 기능 |
| 온디바이스 처리 | 서버 전송 없이 기기 내부에서 AI 연산을 수행하는 방식 |
| 스트리밍 | 소리나 동영상 등의 멀티미디어 파일을 전송하고 재생하는 방식 |
| API | 응용 프로그램에서 사용할 수 있도록 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스 |

### 1.3 참조 문서

| 문서명 | 비고 |
|--------|------|
| project_definition.md | 프로젝트 배경, 핵심 기능, 유사 소프트웨어 분석 |
| project_quality.md | 사용자·개발자·관리자 관점 품질 요소 정의 |
| project_management_plan.md | 개발 절차, 일정, 팀 구성 등 |
| project_requirements_specification.md | 기능적·비기능적·인터페이스 요구사항 정의 |

---

## 2. 시스템 개요

### 2.1 소프트웨어 컨텍스트(Context)

#### 2.1.1 Actor Table

| Actor | Role |
|-------|------|
| 사용자 | 청각장애인 및 난청 사용자로, 이 시스템을 통해 상황 맞춤 문장을 선택하여 의사를 표현하는 주체이다. |
| 관리자 | 이 시스템의 서비스를 관리하고, 문장 데이터셋 및 AI 모델을 운영·유지보수하는 사람이다. |
| 시스템 | 카메라 입력을 분석하고 AI 기반 문장을 생성·추천하여 사용자와 관리자에게 기능을 제공한다. |
| 외부 NLP API | OpenAI API 등 외부 AI 서비스로, 시스템과 연동되어 자연어 문장 생성 및 추천 기능을 지원한다. |

#### 2.1.2 UseCase Diagram

```
<<SilentTalk>>

[사용자] ──── 회원가입을 한다.         (U_01)
         ──── 로그인을 한다.           (U_02)
         ──── 상황/장소를 인식한다.     (U_03)
         ──── 문장 버튼을 선택한다.     (U_04)  ──<include>── 문장을 출력한다.   (U_05)
         ──── 대화 흐름을 추천받는다.   (U_06)
         ──── 제스처로 UI를 조작한다.   (U_07)
         ──── 즐겨찾기를 관리한다.      (U_08)
         ──── 학습 데이터를 관리한다.   (U_09)
         ──── 프로필을 관리한다.        (U_10)

[관리자] ──── 문장 데이터를 관리한다.  (U_11)

[시스템] ──── 문장을 자동 생성한다.    (U_12)
         ──── 사용 패턴을 학습한다.     (U_13)
```

---

### 2.2 기능 분류 및 설명

#### 2.2.1 UseCase Description

---

**Use Case Name : 회원가입을 한다.　ID : U_01　Importance Level : High**

- **Primary Actor :** 사용자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 사용자가 이메일과 비밀번호를 입력하여 계정을 생성하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 서비스 이용을 위해 회원가입을 원한다.
- **Trigger :** 사용자가 회원가입 버튼을 누른다.
- **Relationships**
  - Association : 사용자
- **Normal Flow of Events :**
  1. 사용자는 이메일, 비밀번호를 입력한다.
  2. 사용자는 회원가입 버튼을 누른다.
  3. 시스템은 회원가입이 성공한 경우 메인 화면으로 이동한다.
- **Alternate / Exceptional Flows :**
  - 2.a1 : 공란이 있을 경우 시스템은 회원가입 실패 이유를 화면에 출력한다.
  - 2.a2 : 동일한 이메일이 존재할 경우 시스템은 회원가입 실패 이유를 화면에 출력한다.

---

**Use Case Name : 로그인을 한다.　ID : U_02　Importance Level : High**

- **Primary Actor :** 사용자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 사용자가 아이디와 비밀번호를 입력하여 로그인하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 서비스 이용을 위해 로그인을 원한다.
- **Trigger :** 사용자가 로그인 버튼을 누른다.
- **Relationships**
  - Association : 사용자
- **Normal Flow of Events :**
  1. 사용자는 아이디, 비밀번호를 입력한다.
  2. 사용자는 로그인 버튼을 누른다.
  3-1. 만약 로그인이 성공했다면 → S-1 : 로그인 성공
  3-2. 만약 로그인이 실패했다면 → S-2 : 로그인 실패
- **Subflows :**
  - S-1 : 로그인 성공
    1. 시스템은 메인 화면으로 이동한다.
  - S-2 : 로그인 실패
    1. 시스템은 로그인 실패 이유를 화면에 출력한다.
- **Alternate / Exceptional Flows :**
  - 2.a1 : 아이디 또는 비밀번호가 일치하지 않을 경우 시스템은 오류 메시지를 출력한다.

---

**Use Case Name : 상황/장소를 인식한다.　ID : U_03　Importance Level : High**

- **Primary Actor :** 시스템
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 시스템이 카메라 입력을 통해 현재 장소 및 환경을 자동 인식하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 현재 상황에 맞는 문장을 자동으로 추천받기를 원한다.
  - 시스템 : 카메라 입력을 분석하여 장소를 분류한다.
- **Trigger :** 앱 실행 시 카메라가 활성화된다.
- **Relationships**
  - Association : 시스템
- **Normal Flow of Events :**
  1. 시스템은 카메라 입력을 실시간으로 분석한다.
  2. 시스템은 CV 모델을 통해 현재 장소(카페, 병원, 학교 등)를 분류한다.
  3. 시스템은 인식 결과를 1초 이내에 화면에 반영하고 상황 맞춤 문장 목록을 갱신한다.
- **Alternate / Exceptional Flows :**
  - 3.a1 : 조명이 어둡거나 인식 불가 상황일 경우 시스템은 기본 문장 목록을 표시한다.

---

**Use Case Name : 문장 버튼을 선택한다.　ID : U_04　Importance Level : High**

- **Primary Actor :** 사용자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 사용자가 화면에 표시된 추천 문장 버튼을 선택하여 의사를 전달하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 타이핑 없이 버튼 선택만으로 빠르게 의사를 표현하기를 원한다.
- **Trigger :** 사용자가 문장 버튼을 누른다.
- **Relationships**
  - Association : 사용자
  - Include : 문장을 출력한다. (U_05)
- **Normal Flow of Events :**
  1. 사용자는 화면에 표시된 추천 문장 버튼을 선택한다.
  2. 시스템은 선택된 문장을 1초 이내에 상대방이 볼 수 있는 형태로 출력한다.
  3. 시스템은 선택된 문장을 대화 이력에 추가한다.
- **Alternate / Exceptional Flows :**
  - 2.a1 : 외부 API 연동 지연 시 시스템은 로딩 인디케이터를 표시하고 캐시된 결과를 우선 제공한다.

---

**Use Case Name : 문장을 출력한다.　ID : U_05　Importance Level : High**

- **Primary Actor :** 시스템
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 시스템이 선택된 문장을 화면에 출력하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 시스템 : 선택된 문장을 상대방이 확인할 수 있도록 화면에 표시한다.
- **Trigger :** 사용자가 문장 버튼을 선택한다.
- **Normal Flow of Events :**
  1. 시스템은 선택된 문장을 화면 대화 영역에 표시한다.
  2. 시스템은 대화 이력을 업데이트한다.

---

**Use Case Name : 대화 흐름을 추천받는다.　ID : U_06　Importance Level : High**

- **Primary Actor :** 사용자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 시스템이 이전 대화 내용을 분석하여 다음 문장을 자동 추천하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 대화 맥락에 맞는 다음 문장을 자동으로 추천받기를 원한다.
- **Trigger :** 사용자가 문장을 선택한 후 대화 맥락이 변경된다.
- **Relationships**
  - Association : 사용자, 시스템
- **Normal Flow of Events :**
  1. 시스템은 이전 대화 이력을 분석한다.
  2. 시스템은 NLP 모델을 통해 다음 문장 후보를 생성한다.
  3. 시스템은 2초 이내에 추천 문장 목록을 갱신하여 화면에 표시한다.
- **Alternate / Exceptional Flows :**
  - 3.a1 : 외부 API 응답 지연 시 이전 추천 목록을 유지하고 갱신 완료 후 화면을 업데이트한다.

---

**Use Case Name : 제스처로 UI를 조작한다.　ID : U_07　Importance Level : Mid**

- **Primary Actor :** 사용자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 사용자가 손동작을 통해 문장을 선택하거나 UI를 조작하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 터치 없이 손동작만으로 빠르게 UI를 조작하기를 원한다.
- **Trigger :** 사용자가 카메라 앞에서 정의된 손동작을 취한다.
- **Relationships**
  - Association : 사용자
- **Normal Flow of Events :**
  1. 시스템은 카메라를 통해 사용자의 손동작을 인식한다.
  2. 만약 사용자가 문장 선택 제스처를 취한다면 → S-1 : 문장 선택
  3. 만약 사용자가 화면 전환 제스처를 취한다면 → S-2 : 화면 전환
- **Subflows :**
  - S-1 : 문장 선택
    1. 시스템은 해당 제스처에 매핑된 문장을 선택하여 출력한다.
  - S-2 : 화면 전환
    1. 시스템은 해당 제스처에 매핑된 화면으로 전환한다.
- **Alternate / Exceptional Flows :**
  - 1.a1 : 제스처 인식 실패 시 시스템은 오류 피드백을 시각적으로 표시한다.

---

**Use Case Name : 즐겨찾기를 관리한다.　ID : U_08　Importance Level : Mid**

- **Primary Actor :** 사용자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 사용자가 자주 사용하는 문장을 즐겨찾기로 등록하고 관리하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 자주 사용하는 문장을 즐겨찾기에 등록하여 빠르게 접근하기를 원한다.
- **Trigger :** 사용자가 즐겨찾기 버튼을 누른다.
- **Normal Flow of Events :**
  1. 사용자는 문장 버튼의 즐겨찾기 아이콘을 선택한다.
  2. 시스템은 해당 문장을 즐겨찾기 목록에 추가한다.
  3. 시스템은 즐겨찾기 목록을 별도 영역에 표시한다.
- **Alternate / Exceptional Flows :**
  - 1.a1 : 이미 즐겨찾기에 등록된 문장인 경우 시스템은 즐겨찾기를 해제한다.

---

**Use Case Name : 학습 데이터를 관리한다.　ID : U_09　Importance Level : Mid**

- **Primary Actor :** 사용자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 사용자가 자신의 문장 사용 학습 데이터를 조회하고 초기화 또는 삭제하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 자신의 학습 데이터를 확인하고 필요 시 초기화하기를 원한다.
- **Trigger :** 사용자가 학습 데이터 관리 메뉴를 선택한다.
- **Normal Flow of Events :**
  1. 사용자는 학습 데이터 관리 화면으로 이동한다.
  2. 만약 사용자가 전체 초기화를 원한다면 → S-1 : 전체 초기화
  3. 만약 사용자가 개별 항목을 삭제하길 원한다면 → S-2 : 개별 삭제
- **Subflows :**
  - S-1 : 전체 초기화
    1. 시스템은 확인 메시지를 표시한다.
    2. 사용자가 확인을 누르면 시스템은 모든 학습 데이터를 삭제한다.
  - S-2 : 개별 삭제
    1. 사용자는 삭제할 항목을 선택한다.
    2. 시스템은 해당 항목을 삭제한다.

---

**Use Case Name : 프로필을 관리한다.　ID : U_10　Importance Level : Mid**

- **Primary Actor :** 사용자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 사용자가 자신의 프로필 및 설정 정보를 조회하고 수정하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 사용자 : 자신의 프로필 및 앱 설정을 자유롭게 수정하기를 원한다.
- **Trigger :** 사용자가 프로필 설정 메뉴를 선택한다.
- **Normal Flow of Events :**
  1. 사용자는 프로필 및 설정 화면으로 이동한다.
  2. 사용자는 원하는 항목(이름, 언어, 글자 크기 등)을 수정한다.
  3. 사용자는 저장 버튼을 누른다.
  4. 시스템은 변경 사항을 저장하고 확인 메시지를 출력한다.

---

**Use Case Name : 문장 데이터를 관리한다.　ID : U_11　Importance Level : Mid**

- **Primary Actor :** 관리자
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 관리자가 시스템에서 사용하는 문장 데이터셋을 추가, 수정, 삭제하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 관리자 : 서비스 품질 유지를 위해 문장 데이터를 최신 상태로 관리하기를 원한다.
- **Trigger :** 관리자가 문장 데이터 관리 메뉴를 선택한다.
- **Normal Flow of Events :**
  1. 관리자는 문장 데이터 관리 화면으로 이동한다.
  2. 만약 관리자가 문장을 추가하길 원한다면 → S-1 : 문장 추가
  3. 만약 관리자가 문장을 삭제하길 원한다면 → S-2 : 문장 삭제
- **Subflows :**
  - S-1 : 문장 추가
    1. 관리자는 장소 카테고리와 문장 내용을 입력한다.
    2. 시스템은 해당 문장을 DB에 저장한다.
  - S-2 : 문장 삭제
    1. 관리자는 삭제할 문장을 선택한다.
    2. 시스템은 해당 문장을 DB에서 삭제한다.

---

**Use Case Name : 문장을 자동 생성한다.　ID : U_12　Importance Level : High**

- **Primary Actor :** 시스템
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 시스템이 인식된 장소와 대화 맥락을 결합하여 자연스러운 문장을 자동 생성하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 시스템 : 외부 NLP API와 연동하여 상황에 맞는 문장을 생성한다.
- **Trigger :** 장소 인식 결과 또는 대화 맥락이 변경된다.
- **Normal Flow of Events :**
  1. 시스템은 장소 인식 결과와 현재 대화 맥락 데이터를 수집한다.
  2. 시스템은 외부 NLP API(OpenAI 등)에 문장 생성을 요청한다.
  3. 시스템은 응답받은 문장을 화면 추천 목록에 표시한다.
- **Alternate / Exceptional Flows :**
  - 2.a1 : API 타임아웃(5초) 발생 시 캐시된 추천 문장 또는 기본 문장 목록을 제공한다.

---

**Use Case Name : 사용 패턴을 학습한다.　ID : U_13　Importance Level : High**

- **Primary Actor :** 시스템
- **Use Case Type :** Detail, essential
- **Brief Description :** 이 Use-Case는 시스템이 사용자의 문장 선택 빈도를 기록하고 학습하여 추천 순서를 조정하는 Use Case를 표현한다.
- **Stakeholders and Interests**
  - 시스템 : 사용자별 사용 데이터를 축적하여 개인화된 추천을 제공한다.
- **Trigger :** 사용자가 문장 버튼을 선택한다.
- **Normal Flow of Events :**
  1. 시스템은 사용자가 선택한 문장과 선택 횟수를 DB에 기록한다.
  2. 시스템은 기록된 사용 빈도를 기반으로 다음 추천 시 노출 순서를 조정한다.
  3. 시스템은 사용자별 학습 데이터를 독립적으로 저장한다.

---

## 3. 요구사항 명세

### 3.1 정적 분석

```
Class SilentTalk

┌─────────────────┐       ┌──────────────────────┐
│     사용자       │       │       사용자 DB        │
│─────────────────│       │──────────────────────│
│- 아이디 : String │ 1   * │- 아이디 : String      │
│- 비밀번호 : String├──────▷│- 비밀번호 : String    │
│- 이메일 : String │       │- 이메일 : String      │
│─────────────────│       │──────────────────────│
│+ 회원가입() : void│       │+ 회원가입 요청() : void│
│+ 로그인() : void │       │+ 로그인 요청() : void  │
└────────┬────────┘       └──────────────────────┘
         │ 1
         │
         │ 1..*
┌────────▽────────┐       ┌──────────────────────┐
│   문장 선택      │       │      대화 이력 DB      │
│─────────────────│       │──────────────────────│
│- 선택문장 : String│ 1   * │- 문장번호 : Integer   │
│- 선택시간 : Date ├──────▷│- 사용자ID : String    │
│─────────────────│       │- 문장내용 : String    │
│+ 버튼선택() : void│       │──────────────────────│
│+ 문장출력() : void│       │+ 문장저장() : void    │
└────────┬────────┘       │+ 이력조회() : void    │
         │                └──────────────────────┘
         │ 1
         │ 1..*
┌────────▽────────┐       ┌──────────────────────┐
│   학습 데이터    │       │      문장 DB           │
│─────────────────│       │──────────────────────│
│- 사용자ID : String│       │- 문장번호 : Integer   │
│- 문장ID : Integer│ 1   * │- 장소분류 : String    │
│- 선택횟수 : Integer├─────▷│- 문장내용 : String    │
│─────────────────│       │- 언어 : String        │
│+ 패턴학습() : void│       │──────────────────────│
│+ 순서조정() : void│       │+ 문장추가() : void    │
│+ 데이터삭제():void│       │+ 문장삭제() : void    │
└─────────────────┘       └──────────────────────┘

┌─────────────────┐       ┌──────────────────────┐
│   상황 인식      │       │    외부 NLP API        │
│─────────────────│       │──────────────────────│
│- 장소분류 : String│       │- API주소 : String     │
│- 신뢰도 : Float  │ 1   1 │──────────────────────│
│─────────────────├──────▷│+ 문장생성요청() : void  │
│+ 장소인식() : void│       │+ 대화추천요청() : void  │
│+ 결과갱신() : void│       └──────────────────────┘
└─────────────────┘
```

---

### 3.2 CRC 카드

---

**Class Name : 사용자　ID : 01　Type : Concrete, Domain**

- **Description :** 청각장애인 및 난청 사용자로, 시스템을 통해 의사를 표현하는 주체이다.
- **Associated Use Case :** U_01, U_02, U_04, U_07, U_08, U_09, U_10

| Responsibilities | Collaborators |
|------------------|---------------|
| 회원가입() : void | 사용자 DB |
| 로그인() : void | 사용자 DB |
| 버튼선택() : void | 문장 선택 |
| 제스처입력() : void | 상황 인식 |

**Attributes**
- 아이디 : String
- 비밀번호 : String
- 이메일 : String

**Relationships**
- Generalization (a-kind-of) : 회원가입, 로그인
- Aggregation (has-parts) : 학습 데이터
- Other Associations : 문장 선택

---

**Class Name : 사용자 DB　ID : 02　Type : Concrete, Domain**

- **Description :** 회원인 사용자의 계정 정보를 저장하는 DB이다.
- **Associated Use Case :** U_01, U_02

| Responsibilities | Collaborators |
|------------------|---------------|
| 회원가입 요청() : void | 사용자 |
| 로그인 요청() : void | 사용자 |
| 사용자 정보 수정() : void | 사용자 |

**Attributes**
- 아이디 : String
- 비밀번호 : String
- 이메일 : String

**Relationships**
- Aggregation (has-parts) : 사용자
- Other Associations : 학습 데이터 DB

---

**Class Name : 상황 인식　ID : 03　Type : Concrete, Domain**

- **Description :** 카메라 입력을 분석하여 현재 장소 및 환경을 자동으로 인식하는 모듈이다.
- **Associated Use Case :** U_03, U_12

| Responsibilities | Collaborators |
|------------------|---------------|
| 장소인식() : void | 시스템, 사용자 |
| 결과갱신() : void | 문장 DB |
| 제스처인식() : void | 사용자 |

**Attributes**
- 장소분류 : String
- 신뢰도 : Float
- 인식시간 : Date

**Relationships**
- Other Associations : 문장 DB, 외부 NLP API

---

**Class Name : 문장 선택　ID : 04　Type : Concrete, Domain**

- **Description :** 사용자가 화면에서 추천 문장 버튼을 선택하여 의사를 전달하는 기능이다.
- **Associated Use Case :** U_04, U_05

| Responsibilities | Collaborators |
|------------------|---------------|
| 버튼선택() : void | 사용자 |
| 문장출력() : void | 대화 이력 DB |
| 즐겨찾기등록() : void | 사용자 |

**Attributes**
- 선택문장 : String
- 선택시간 : Date

**Relationships**
- Aggregation (has-parts) : 사용자
- Other Associations : 대화 이력 DB, 학습 데이터

---

**Class Name : 학습 데이터　ID : 05　Type : Concrete, Domain**

- **Description :** 사용자의 문장 선택 빈도를 기록하고 개인화 추천에 활용하는 데이터이다.
- **Associated Use Case :** U_09, U_13

| Responsibilities | Collaborators |
|------------------|---------------|
| 패턴학습() : void | 시스템 |
| 순서조정() : void | 문장 DB |
| 데이터삭제() : void | 사용자 |

**Attributes**
- 사용자ID : String
- 문장ID : Integer
- 선택횟수 : Integer

**Relationships**
- Aggregation (has-parts) : 사용자
- Other Associations : 문장 DB

---

**Class Name : 문장 DB　ID : 06　Type : Concrete, Domain**

- **Description :** 장소별·상황별 추천 문장 데이터를 저장·관리하는 DB이다.
- **Associated Use Case :** U_03, U_04, U_11, U_12

| Responsibilities | Collaborators |
|------------------|---------------|
| 문장추가() : void | 관리자 |
| 문장삭제() : void | 관리자 |
| 문장조회() : void | 사용자, 시스템 |

**Attributes**
- 문장번호 : Integer
- 장소분류 : String
- 문장내용 : String
- 언어 : String

**Relationships**
- Other Associations : 상황 인식, 대화 이력 DB

---

**Class Name : 대화 이력 DB　ID : 07　Type : Concrete, Domain**

- **Description :** 사용자의 대화 내용을 저장하고 AI 추천에 활용하는 DB이다.
- **Associated Use Case :** U_05, U_06

| Responsibilities | Collaborators |
|------------------|---------------|
| 문장저장() : void | 시스템 |
| 이력조회() : void | 사용자, 시스템 |
| 이력삭제() : void | 사용자 |

**Attributes**
- 문장번호 : Integer
- 사용자ID : String
- 문장내용 : String
- 저장시간 : Date

**Relationships**
- Other Associations : 문장 DB, 외부 NLP API

---

**Class Name : 외부 NLP API　ID : 08　Type : Concrete, Domain**

- **Description :** OpenAI API 등 외부 자연어 처리 서비스로, 문장 생성 및 대화 흐름 추천을 지원한다.
- **Associated Use Case :** U_06, U_12

| Responsibilities | Collaborators |
|------------------|---------------|
| 문장생성요청() : void | 시스템 |
| 대화추천요청() : void | 시스템 |

**Attributes**
- API주소 : String
- 타임아웃 : Integer

**Relationships**
- Other Associations : 상황 인식, 대화 이력 DB

---

**Class Name : 관리자　ID : 09　Type : Concrete, Domain**

- **Description :** 시스템 서비스를 운영·관리하며 문장 데이터셋을 유지보수하는 주체이다.
- **Associated Use Case :** U_11

| Responsibilities | Collaborators |
|------------------|---------------|
| 문장추가() : void | 문장 DB |
| 문장삭제() : void | 문장 DB |
| 데이터관리() : void | 사용자 DB |

**Attributes**
- 관리자ID : String
- 비밀번호 : String

**Relationships**
- Other Associations : 문장 DB, 사용자 DB

---

### 3.3 동적 분석

#### 3.3.1 회원가입을 한다.

```
sd:회원가입을 한다.

   사용자              :사용자 DB
     |                    |
     |  1. 회원가입 요청   |
     |─────────────────▶  |
     |                    |
alt  |                    |
[가입성공]      success    |
     |◀─────────────────  |
[공란 有]        fail      |
     |◀─────────────────  |
[중복 有]        fail      |
     |◀─────────────────  |
```

#### 3.3.2 로그인을 한다.

```
sd:로그인을 한다.

   사용자              :사용자 DB
     |                    |
     |  1. 로그인 요청     |
     |─────────────────▶  |
     |                    |
alt  |                    |
[로그인 성공]   success    |
     |◀─────────────────  |
[로그인 실패]    fail      |
     |◀─────────────────  |
```

#### 3.3.3 상황/장소를 인식한다.

```
sd:상황/장소를 인식한다.

   시스템        :상황 인식       :문장 DB
     |               |               |
     |  장소인식()    |               |
     |──────────────▶|               |
     |               |  문장조회()   |
     |               |──────────────▶|
     |               |    success    |
     |               |◀──────────── |
     |    success     |               |
     |◀──────────────|               |
```

#### 3.3.4 문장 버튼을 선택한다.

```
sd:문장 버튼을 선택한다.

   사용자       :문장 선택     :대화 이력 DB
     |               |               |
     |  버튼선택()   |               |
     |──────────────▶|               |
     |               |  문장저장()   |
     |               |──────────────▶|
     |               |    success    |
     |               |◀─────────────|
     |    success     |               |
     |◀──────────────|               |
```

#### 3.3.5 대화 흐름을 추천받는다.

```
sd:대화 흐름을 추천받는다.

   시스템     :대화 이력 DB    :외부 NLP API
     |               |               |
     |  이력조회()   |               |
     |──────────────▶|               |
     |    success     |               |
     |◀──────────────|               |
     |          대화추천요청()        |
     |───────────────────────────────▶
     |              success           |
     |◀───────────────────────────────
```

#### 3.3.6 제스처로 UI를 조작한다.

```
sd:제스처로 UI를 조작한다.

   사용자         :상황 인식
     |               |
     |  제스처입력() |
     |──────────────▶|
     |               |
alt  |               |
[문장선택]  success   |
     |◀──────────────|
[화면전환]  success   |
     |◀──────────────|
[인식실패]   fail     |
     |◀──────────────|
```

#### 3.3.7 즐겨찾기를 관리한다.

```
sd:즐겨찾기를 관리한다.

   사용자       :문장 선택     :문장 DB
     |               |               |
     | 즐겨찾기등록() |               |
     |──────────────▶|               |
     |               |  문장저장()   |
     |               |──────────────▶|
     |               |    success    |
     |               |◀─────────────|
     |    success     |               |
     |◀──────────────|               |
```

#### 3.3.8 학습 데이터를 관리한다.

```
sd:학습 데이터를 관리한다.

   사용자         :학습 데이터
     |               |
     |  데이터삭제() |
     |──────────────▶|
     |               |
opt  |               |
[삭제]     success   |
     |◀──────────────|
```

#### 3.3.9 문장을 자동 생성한다.

```
sd:문장을 자동 생성한다.

   시스템        :상황 인식    :외부 NLP API    :문장 DB
     |               |               |               |
     |  장소인식()   |               |               |
     |──────────────▶|               |               |
     |    success     |               |               |
     |◀──────────────|               |               |
     |         문장생성요청()         |               |
     |───────────────────────────────▶               |
     |              success           |               |
     |◀───────────────────────────────               |
     |                            문장저장()          |
     |────────────────────────────────────────────── ▶
     |                              success           |
     |◀──────────────────────────────────────────────
```

#### 3.3.10 사용 패턴을 학습한다.

```
sd:사용 패턴을 학습한다.

   시스템         :학습 데이터    :문장 DB
     |               |               |
     |  패턴학습()   |               |
     |──────────────▶|               |
     |               |  순서조정()   |
     |               |──────────────▶|
     |               |    success    |
     |               |◀─────────────|
     |    success     |               |
     |◀──────────────|               |
```

---

## 4. 인터페이스 분석

| 분류 | 인터페이스 | 설명 |
|------|-----------|------|
| 사용자 인터페이스 | 모바일 앱 (Android/iOS) | 버튼 기반 추천 문장 선택, 카메라 미리보기, 대화 이력 표시 화면 제공 |
| 하드웨어 인터페이스 | 디바이스 전면/후면 카메라 | 장소 인식 및 제스처 인식에 활용. 1080p 이상 권장, 720p 지원 |
| 소프트웨어 인터페이스 | 외부 NLP API (OpenAI 등) | 자연어 문장 생성 및 대화 흐름 추천 기능 연동 |
| 소프트웨어 인터페이스 | PostgreSQL | 사용자 계정, 문장 데이터, 학습 데이터 저장 |
| 소프트웨어 인터페이스 | RESTful API | 모바일 앱과 백엔드 서버 간 통신 |

---

## 5. 제약사항

| 분류 | 제약사항 |
|------|---------|
| 운영 환경 | Android 10 이상 및 iOS 14 이상 환경에서 동작해야 한다. |
| 앱 용량 | 앱 설치 용량은 200MB 이하로 유지해야 한다. |
| 응답 시간 | 온디바이스 처리 기준 상황 인식 1초, 버튼 반응 300ms, 문장 추천 갱신 2초 이내를 충족해야 한다. |
| 보안 | 개인정보는 AES-256 이상으로 암호화 저장하며, 서버 전송 시 TLS 1.2 이상을 적용해야 한다. |
| 개인정보 | 개인정보보호법 및 장애인차별금지법을 준수하여 개발해야 한다. |
| 오프라인 | 네트워크 미연결 환경에서도 기본 버튼 기반 의사 표현 기능은 동작해야 한다. |

---

## 6. 요구사항 추적표

| 요구사항 | U_01 | U_02 | U_03 | U_04 | U_05 | U_06 | U_07 | U_08 | U_09 | U_10 | U_11 | U_12 | U_13 |
|---------|------|------|------|------|------|------|------|------|------|------|------|------|------|
| FR-001 (장소 자동 인식) | | | O | | | | | | | | | O | |
| FR-002 (상황 맞춤 문장 목록 생성) | | | O | | | | | | | | | O | |
| FR-003 (인식 결과 실시간 갱신) | | | O | | | | | | | | | | |
| FR-004 (버튼 선택 의사 전달) | | | | O | O | | | | | | | | |
| FR-005 (1초 이내 문장 출력) | | | | O | O | | | | | | | | |
| FR-006 (즐겨찾기 등록) | | | | | | | | O | | | | | |
| FR-007 (대화 흐름 자동 추천) | | | | | | O | | | | | | O | |
| FR-008 (대화 맥락 변경 시 추천 갱신) | | | | | | O | | | | | | | |
| FR-009 (상황 맞춤 문장 자동 생성) | | | O | | | | | | | | | O | |
| FR-010 (생성 문장 편집 인터페이스) | | | | O | | | | | | | | | |
| FR-011 (한국어/영어 지원) | | | | | | | | | | | O | O | |
| FR-012 (제스처 인식) | | | | | | | O | | | | | | |
| FR-013 (제스처 문장 선택 및 UI 조작) | | | | | | | O | | | | | | |
| FR-014 (제스처 매핑 설정 화면) | | | | | | | O | | | | | | |
| FR-015 (사용 문장 및 횟수 기록) | | | | | | | | | | | | | O |
| FR-016 (사용자별 학습 데이터 독립 저장) | | | | | | | | | O | | | | O |
| FR-017 (학습 데이터 초기화/삭제) | | | | | | | | | O | | | | |
| FR-018 (회원가입) | O | | | | | | | | | | | | |
| FR-019 (로그인) | | O | | | | | | | | | | | |
| FR-020 (프로필 조회/수정) | | | | | | | | | | O | | | |

---

## 7. 참고문헌 및 부록

- [부록 1] SilentTalk 프로젝트 정의서 (project_definition.md)
- [부록 2] SilentTalk 대상 시스템 품질 요소 (project_quality.md)
- [부록 3] SilentTalk 프로젝트 관리 계획서 (project_management_plan.md)
- [부록 4] SilentTalk 요구사항 정의서 (project_requirements_specification.md)
