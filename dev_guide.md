# 초성 맞추기 게임 개발 가이드

## 📋 프로젝트 개요

**프로젝트명**: 한글 초성 맞추기 게임  
**버전**: 2.0 (힌트 시스템 적용)  
**개발 도구**: Google Antigravity (AI 코딩 도구)  
**기술 스택**: HTML5, CSS3, JavaScript (Vanilla)

---

## 🎯 프로젝트 목표

관중들이 참여할 수 있는 인터랙티브한 한글 초성 맞추기 게임 제작
- 7개 카테고리 (나라, 음식, 동물, 영화, 스포츠, 직업, 과일)
- 각 카테고리당 50개 문제 (총 350개)
- 힌트 시스템으로 정답 유일성 보장
- 간결한 UI/UX (정답 보기, 다음 문제 2개 버튼)

---

## 📁 파일 구조

```
chosung_game_v2.html (단일 HTML 파일)
├── HTML (구조)
├── CSS (스타일)
└── JavaScript (로직)
```

---

## 🏗️ 핵심 아키텍처

### 1. 데이터 구조

```javascript
const wordData = {
    '카테고리명': [
        { word: '단어', hint: '힌트 설명' },
        { word: '단어2', hint: '힌트 설명2' }
    ]
};
```

**예시:**
```javascript
'음식': [
    { word: '김밥', hint: '밥과 재료를 김으로 만 음식, 3000원' },
    { word: '떡볶이', hint: '빨간색 분식' },
    { word: '짜장면', hint: '검은색 중국음식' }
]
```

### 2. 핵심 함수

#### 2.1 초성 추출 함수
```javascript
function getChosung(word) {
    const chosungList = ['ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ'];
    let result = '';
    
    for (let i = 0; i < word.length; i++) {
        const code = word.charCodeAt(i) - 44032;
        if (code > -1 && code < 11172) {
            result += chosungList[Math.floor(code / 588)];
        }
    }
    return result.split('').join(' ');
}
```

**동작 원리:**
- 한글 유니코드 범위: 44032 ~ 55203
- 초성 인덱스 = (문자코드 - 44032) / 588
- 공백으로 구분하여 가독성 향상

#### 2.2 게임 시작 함수
```javascript
function startGame(category) {
    currentCategory = category;
    currentWords = [...wordData[category]].sort(() => Math.random() - 0.5);
    currentIndex = 0;
    // 화면 전환 로직
    displayQuestion();
}
```

#### 2.3 문제 표시 함수
```javascript
function displayQuestion() {
    const currentItem = currentWords[currentIndex];
    const chosung = getChosung(currentItem.word);
    
    document.getElementById('chosungDisplay').textContent = chosung;
    document.getElementById('hintDisplay').textContent = currentItem.hint;
    document.getElementById('answerDisplay').textContent = '';
    document.getElementById('progressDisplay').textContent = `${currentIndex + 1} / ${currentWords.length}`;
}
```

---

## 🎨 UI/UX 설계

### 화면 구성

#### 1. 시작 화면 (카테고리 선택)
```
+----------------------------------+
|     🎯 초성 맞추기 게임           |
+----------------------------------+
| [🌍 나라]  [🍔 음식]  [🐾 동물]  |
| [🎬 영화]  [⚽ 스포츠] [💼 직업]  |
| [🍎 과일]                        |
+----------------------------------+
```

#### 2. 게임 화면
```
+----------------------------------+
| [← 카테고리 선택]                 |
| 카테고리: 음식                    |
|                                  |
| +------------------------------+ |
| |  💡 힌트                     | |
| |  밥과 재료를 김으로 만 음식,  | |
| |  3000원                      | |
| +------------------------------+ |
|                                  |
| +------------------------------+ |
| |        ㄱ   ㅂ               | |
| +------------------------------+ |
|                                  |
|         (정답: 김밥)             |
|                                  |
| [정답 보기]     [다음 문제]       |
|                                  |
|          1 / 50                  |
+----------------------------------+
```

### 색상 팔레트

```css
주요 색상:
- 보라색 그라데이션: #667eea → #764ba2 (배경)
- 힌트 박스: #ffeaa7 → #fdcb6e (노란색 계열)
- 초성 표시: #667eea (보라색)
- 정답: #28a745 (초록색)
- 버튼 호버: 3px 상승 효과
```

### 반응형 디자인

```css
/* 모바일 (768px 이하) */
@media (max-width: 768px) {
    .chosung-display { font-size: 3em; }
    .hint-text { font-size: 1.3em; }
    .answer-display { font-size: 2em; }
}
```

---

## 📊 데이터 작성 가이드

### 힌트 작성 원칙

1. **구체적 수치 활용**
   - ✅ "가격 3,000원"
   - ✅ "높이 10m"
   - ✅ "인구 5000만"

2. **고유한 특징 강조**
   - ✅ "빨간색 분식"
   - ✅ "검은 소스의 중국 음식"
   - ✅ "갈기가 있는 아프리카의 왕"

3. **중복 방지**
   - ❌ "맛있는 음식" (너무 일반적)
   - ✅ "간장 양념한 소고기 구이" (구체적)

4. **난이도 조절**
   - 쉬움: "빨간색 채소, 토마토는 과일"
   - 중간: "이탈리아 요리, 도우 위에 치즈"
   - 어려움: "16세기 유럽에서 시작된 발효 음식"

### 카테고리별 힌트 전략

| 카테고리 | 힌트 유형 | 예시 |
|---------|----------|------|
| 나라 | 수도, 랜드마크, 위치 | "에펠탑이 있는 나라" |
| 음식 | 색상, 가격, 재료 | "빨간 고추장, 가격 3000원" |
| 동물 | 생김새, 서식지, 습성 | "줄무늬가 있는 백수의 왕" |
| 영화 | 감독, 배우, 줄거리 | "놀란 감독, 꿈속의 꿈" |
| 스포츠 | 도구, 장소, 방식 | "라켓으로 노란 공을 치는" |
| 직업 | 장소, 역할, 도구 | "법정에서 판결하는 직업" |
| 과일 | 색상, 모양, 맛 | "노란색 길쭉한 열대 과일" |

---

## 🔧 기능 구현 상세

### 1. 카테고리 선택

```javascript
// 7개 카테고리 버튼 생성
categories = ['나라', '음식', '동물', '영화', '스포츠', '직업', '과일'];

// 클릭 시 해당 카테고리 게임 시작
onclick="startGame('음식')"
```

### 2. 문제 랜덤 섞기

```javascript
// Fisher-Yates 셔플 알고리즘 활용
currentWords = [...wordData[category]].sort(() => Math.random() - 0.5);
```

### 3. 정답 보기 토글

```javascript
let showingAnswer = false;

function showAnswer() {
    if (!showingAnswer) {
        document.getElementById('answerDisplay').textContent = currentItem.word;
        showingAnswer = true;
    }
}
```

### 4. 다음 문제 순환

```javascript
function nextQuestion() {
    currentIndex++;
    if (currentIndex >= currentWords.length) {
        currentIndex = 0;
        currentWords.sort(() => Math.random() - 0.5); // 재섞기
        alert('모든 문제를 완료했습니다!');
    }
    displayQuestion();
}
```

### 5. 진행 상황 표시

```javascript
document.getElementById('progressDisplay').textContent = 
    `${currentIndex + 1} / ${currentWords.length}`;
```

---

## 🚀 확장 기능 (Phase 2)

### 1. 관리자 페이지

```javascript
// 기능 목록
- 문제 CRUD (Create, Read, Update, Delete)
- 카테고리 관리
- 힌트 품질 검증
- 중복 검사
- CSV 임포트/엑스포트
```

**예상 UI:**
```
+----------------------------------+
| 관리자 페이지                     |
+----------------------------------+
| 카테고리: [음식 ▼]               |
|                                  |
| 단어: [______]                   |
| 힌트: [___________________]      |
|                                  |
| [추가] [수정] [삭제]              |
|                                  |
| 현재 문제 목록 (50개)             |
| 1. 김밥 - 3000원...              |
| 2. 떡볶이 - 빨간색...            |
+----------------------------------+
```

### 2. 타이머 기능

```javascript
let timeLimit = 30; // 30초 제한
let timer = null;

function startTimer() {
    timer = setInterval(() => {
        timeLimit--;
        if (timeLimit <= 0) {
            clearInterval(timer);
            showAnswer(); // 자동으로 정답 표시
        }
    }, 1000);
}
```

### 3. 점수 시스템

```javascript
let score = 0;
let correctAnswers = 0;

function calculateScore(timeTaken) {
    // 빠를수록 높은 점수
    let points = Math.max(100 - timeTaken * 2, 10);
    score += points;
    correctAnswers++;
}
```

### 4. 멀티플레이어 모드

```javascript
// WebSocket 또는 Firebase 활용
const players = [
    { name: '플레이어1', score: 0 },
    { name: '플레이어2', score: 0 }
];

function updateLeaderboard() {
    // 실시간 순위 업데이트
}
```

### 5. 난이도 선택

```javascript
const difficulty = {
    easy: { hintVisible: true, timeLimit: 60 },
    medium: { hintVisible: true, timeLimit: 30 },
    hard: { hintVisible: false, timeLimit: 15 }
};
```

---

## 📱 배포 가이드

### 1. AWS Lightsail 배포

```bash
# SSH 접속
ssh bitnami@jvibeschool.com

# 웹 디렉토리로 이동
cd /home/bitnami/htdocs

# 파일 업로드 (로컬에서)
scp chosung_game_v2.html bitnami@jvibeschool.com:/home/bitnami/htdocs/

# 권한 설정
chmod 644 chosung_game_v2.html

# URL 접근
# https://jvibeschool.com/chosung_game_v2.html
```

### 2. GitHub Pages 배포

```bash
# 리포지토리 생성
git init
git add chosung_game_v2.html
git commit -m "Initial commit: 초성 맞추기 게임 v2"

# GitHub 리포지토리에 푸시
git remote add origin https://github.com/jvisualschool/chosung-game.git
git branch -M main
git push -u origin main

# Settings > Pages > Source: main branch 선택
# URL: https://jvisualschool.github.io/chosung-game/chosung_game_v2.html
```

### 3. 도메인 연결

```bash
# DNS 설정 (jvibeschool.com)
# A 레코드: game.jvibeschool.com → Lightsail IP
# 또는
# CNAME: game.jvibeschool.com → jvisualschool.github.io
```

---

## 🧪 테스트 시나리오

### 1. 기능 테스트

| 테스트 항목 | 예상 결과 | 상태 |
|------------|----------|------|
| 카테고리 선택 | 해당 카테고리 문제 표시 | ✅ |
| 초성 표시 | 공백으로 구분된 초성 | ✅ |
| 힌트 표시 | 노란 박스에 힌트 표시 | ✅ |
| 정답 보기 | 초록색으로 정답 표시 | ✅ |
| 다음 문제 | 새로운 문제로 전환 | ✅ |
| 진행 상황 | 1/50, 2/50... 표시 | ✅ |
| 문제 완료 | 알림 후 재시작 | ✅ |

### 2. UI/UX 테스트

- [x] 모바일 반응형 (360px ~ 768px)
- [x] 태블릿 반응형 (768px ~ 1024px)
- [x] PC 화면 (1024px+)
- [x] 버튼 호버 효과
- [x] 색상 대비 (접근성)
- [x] 폰트 가독성

### 3. 데이터 검증

```javascript
// 중복 단어 체크
function checkDuplicates() {
    const allWords = [];
    for (let category in wordData) {
        wordData[category].forEach(item => {
            if (allWords.includes(item.word)) {
                console.error(`중복: ${item.word}`);
            }
            allWords.push(item.word);
        });
    }
}

// 힌트 길이 체크
function checkHintLength() {
    for (let category in wordData) {
        wordData[category].forEach(item => {
            if (item.hint.length < 5) {
                console.warn(`힌트 너무 짧음: ${item.word}`);
            }
            if (item.hint.length > 50) {
                console.warn(`힌트 너무 김: ${item.word}`);
            }
        });
    }
}
```

---

## 🐛 알려진 이슈 및 해결

### Issue #1: 초성 추출 오류
**문제:** 영어, 숫자 섞인 단어 처리 불가  
**해결:** 한글만 초성 추출, 나머지는 그대로 표시
```javascript
if (code > -1 && code < 11172) {
    result += chosungList[Math.floor(code / 588)];
} else {
    result += word[i]; // 한글 아닌 문자는 그대로
}
```

### Issue #2: 모바일 키보드 이슈
**문제:** 키보드 올라올 때 화면 레이아웃 깨짐  
**해결:** viewport 설정 조정
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

### Issue #3: 긴 힌트 텍스트
**문제:** 힌트가 너무 길면 박스 밖으로 나감  
**해결:** word-break 적용
```css
.hint-text {
    word-break: keep-all;
    overflow-wrap: break-word;
}
```

---

## 📚 참고 자료

### 한글 유니코드
- 한글 유니코드 범위: U+AC00 ~ U+D7A3 (44032 ~ 55203)
- 초성 19개, 중성 21개, 종성 28개
- 계산식: ((초성 × 21) + 중성) × 28 + 종성 + 0xAC00

### JavaScript 배열 셔플
- Fisher-Yates 알고리즘
- 시간 복잡도: O(n)

### CSS 그라데이션
- linear-gradient(135deg, color1, color2)
- 135도는 좌상단 → 우하단 대각선

---

## 💬 Google Antigravity 프롬프트 예시

### 초기 개발 요청
```
한글 초성 맞추기 게임을 만들어줘.

요구사항:
1. 7개 카테고리 (나라, 음식, 동물, 영화, 스포츠, 직업, 과일)
2. 각 카테고리당 50개 단어
3. 각 단어마다 힌트 제공 (고유성 보장)
4. 초성을 공백으로 구분하여 표시
5. 정답 보기, 다음 문제 2개 버튼
6. 반응형 디자인
7. 보라색 그라데이션 테마
8. 단일 HTML 파일로 구현

데이터 구조:
{ word: '김밥', hint: '3000원짜리 음식' }
```

### 기능 추가 요청
```
현재 게임에 다음 기능을 추가해줘:

1. 타이머 기능 (30초 제한)
2. 정답/오답 통계
3. 점수 시스템 (빠를수록 높은 점수)
4. 로컬스토리지에 최고 점수 저장
5. 소리 효과 (정답/오답)

UI 개선:
- 타이머는 우상단에 표시
- 점수는 좌상단에 표시
- 정답 시 초록색 깜빡임 효과
```

### 관리자 페이지 요청
```
관리자 페이지를 추가해줘:

기능:
1. 문제 목록 보기 (카테고리별)
2. 문제 추가 (단어 + 힌트)
3. 문제 수정
4. 문제 삭제
5. CSV 내보내기/가져오기
6. 중복 단어 체크
7. 힌트 길이 검증

비밀번호 보호:
- 간단한 비밀번호 인증 (admin123)
- localStorage에 인증 상태 저장
```

### 스타일 개선 요청
```
UI를 더 모던하게 개선해줘:

1. 카드 디자인 적용 (그림자, 둥근 모서리)
2. 버튼 애니메이션 강화
3. 정답 표시 시 confetti 효과
4. 다크 모드 지원
5. 커스텀 폰트 적용 (Noto Sans KR)
6. 로딩 애니메이션
```

---

## ✅ 체크리스트

### 개발 완료 항목
- [x] 기본 게임 구조
- [x] 7개 카테고리 × 50개 문제
- [x] 초성 추출 함수
- [x] 힌트 시스템
- [x] 정답 보기/다음 문제 버튼
- [x] 진행 상황 표시
- [x] 반응형 디자인
- [x] 문제 랜덤 섞기

### 개발 예정 항목
- [ ] 관리자 페이지
- [ ] 타이머 기능
- [ ] 점수 시스템
- [ ] 로컬스토리지 연동
- [ ] 소리 효과
- [ ] 다크 모드
- [ ] 멀티플레이어
- [ ] 통계 페이지

---

## 📞 문의 및 지원

**개발자**: 정작가 (jvisualschool@gmail.com)  
**GitHub**: https://github.com/jvisualschool/  
**웹사이트**: https://www.jvisualschool.com/

---

## 📄 라이센스

MIT License - 자유롭게 사용 및 수정 가능

---

**작성일**: 2026-01-07  
**버전**: 2.0  
**문서 형식**: Markdown
