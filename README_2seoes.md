# @2SEOES SHOP - 키보드 쇼핑몰 프론트엔드

> 기계식 키보드 매니아를 위한 프리미엄 키보드 쇼핑몰

---

## 프로젝트 개요

| 항목 | 내용 |
|------|------|
| **프로젝트명** | @2SEOES SHOP (KeyBoard Shop) |
| **프로젝트 유형** | 개인 프로젝트 (첫 프로젝트) |
| **담당 역할** | 기획, 디자인, 프론트엔드 전체 구현 |

---

## 기술 스택

| 분류 | 기술 | 설명 |
|------|------|------|
| **Markup** | HTML5 | 시맨틱 마크업, 웹 표준 준수 |
| **Styling** | CSS3 | CSS Custom Properties, Flexbox, 반응형 디자인 |
| **Framework** | Bootstrap 5.3.2 | UI 컴포넌트, Grid 시스템, 반응형 레이아웃 |
| **Icons** | Bootstrap Icons | 아이콘 라이브러리 |
| **Script** | JavaScript (ES6+) | DOM 조작, 이벤트 핸들링, 비동기 처리 |
| **Storage** | localStorage / sessionStorage | 클라이언트 사이드 데이터 영속화 |
| **Server** | nginx 1.28.0 | 정적 웹 서버 |
| **Fonts** | Google Fonts | Playfair Display (세리프), Noto Sans KR (본문) |

---

## 프로젝트 구조

```
html/
├── main.html              # 메인 페이지 (타이핑 애니메이션)
├── login.html             # 로그인
├── join.html              # 회원가입
├── mypage.html            # 마이페이지
├── edit-mypage.html       # 마이페이지 수정
├── shop.html              # 상품 목록 (그리드 뷰)
├── shop-list.html         # 상품 목록 (리스트 뷰)
├── product.html           # 상품 상세
├── admin/                 # 관리자 페이지
│   ├── product-list.html      # 상품 관리 목록
│   ├── product-create.html    # 상품 등록
│   ├── product-detail.html    # 상품 상세 (관리자)
│   ├── product-edit.html      # 상품 수정
│   └── member-list.html       # 회원 관리
└── images/                # 상품 이미지
    ├── keyboard1~3.JPG        # 키보드 이미지
    ├── keycap1~2.JPG          # 키캡 이미지
    ├── switch1~2.JPG          # 스위치 이미지
    └── accessory1~3.JPG       # 액세서리 이미지
```

---

## 구현 기능 상세

### 1. 메인 페이지 (main.html)

**타이핑 애니메이션 효과**
- `@2SEOES SHOP` 텍스트가 한 글자씩 타이핑되는 효과
- CSS `@keyframes`를 활용한 커서 깜빡임 애니메이션
- setTimeout 재귀 호출로 타이핑 구현

```javascript
// 타이핑 효과 구현
const text = '@2SEOES SHOP';
let idx = 0;
function type() {
    if (idx < text.length) {
        brandEl.textContent += text.charAt(idx++);
        setTimeout(type, 150);
    }
}
```

**역할 기반 네비게이션 분기**
- sessionStorage의 `userRole` 값에 따라 UI 동적 변경
- 일반 사용자: Login, Cart 메뉴
- 관리자: My Page, Cart, Admin 드롭다운 메뉴

---

### 2. 인증 시스템

**로그인 (login.html)**
- sessionStorage 기반 클라이언트 사이드 인증
- 관리자 계정 검증 (admin / 1234)
- 로그인 성공 시 `userRole` 세션 저장

```javascript
if (id === 'admin' && pw === '1234') {
    sessionStorage.setItem('userRole', 'admin');
    window.location.href = 'main.html';
}
```

**회원가입 (join.html)**
- First Name, Last Name, Email, Password 입력 폼
- Bootstrap 폼 컴포넌트 활용

**로그아웃**
- sessionStorage에서 `userRole` 제거
- 메인 페이지로 리다이렉트

---

### 3. 상품 목록

**그리드 뷰 (shop.html)**
- Bootstrap Grid 시스템 활용 (row-cols-1, row-cols-sm-2, row-cols-md-3)
- 반응형 레이아웃: 모바일 1열 → 태블릿 2열 → 데스크탑 3열
- localStorage에서 상품 데이터 로드 후 동적 렌더링

**리스트 뷰 (shop-list.html)**
- 가로형 카드 레이아웃 (이미지 + 정보)
- 카테고리 대문자 표시
- 상품 간 구분선(hr) 동적 삽입

**뷰 전환 기능**
- Bootstrap Icons 활용 (bi-list-ul, bi-grid-3x3-gap)
- 현재 뷰에 `active` 클래스 적용

---

### 4. 상품 상세 (product.html)

**URL 파라미터 기반 상품 조회**
```javascript
const urlParams = new URLSearchParams(window.location.search);
const productId = parseInt(urlParams.get('id'));
const product = products.find(p => p.id === productId);
```

**상품 정보 표시**
- 상품 이미지 (img-fluid 반응형)
- 상품명 (Playfair Display 폰트)
- 가격 (toLocaleString 포맷팅)
- 상세 설명

**아코디언 UI**
- Bootstrap Accordion 컴포넌트 활용
- 배송 정보, 교환/반품 안내 토글

**구매 버튼**
- ADD TO CART (장바구니 담기)
- BUY NOW (바로 구매)

---

### 5. 관리자 페이지

#### 5-1. 상품 관리 (product-list.html)

**테이블 형태 상품 목록**
- ID, 썸네일, 상품명, 가격, 재고, 관리 버튼 표시
- Bootstrap Table 컴포넌트 (table-hover, align-middle)

**상품 삭제 기능**
```javascript
function deleteProduct(productId) {
    if (confirm(`상품 ID: ${productId}번 상품을 정말 삭제하시겠습니까?`)) {
        products = products.filter(p => p.id !== productId);
        localStorage.setItem('myShopProducts', JSON.stringify(products));
        renderProductList();
    }
}
```

#### 5-2. 상품 등록 (product-create.html)

**입력 필드**
- 상품명 (text)
- 카테고리 (select: 키보드, 키캡, 스위치, 액세서리)
- 가격 (number)
- 재고 (number)
- 상품 설명 (textarea)
- 이미지 URL (text)

**데이터 저장**
```javascript
const newProduct = {
    id: Date.now(),  // 유니크 ID 생성
    name, category, price, stock, description, imageUrl
};
products.push(newProduct);
localStorage.setItem('myShopProducts', JSON.stringify(products));
```

#### 5-3. 상품 수정 (product-edit.html)

**기존 데이터 바인딩**
- URL 파라미터로 상품 ID 전달
- localStorage에서 해당 상품 조회
- 폼 필드에 기존 값 자동 입력

**데이터 업데이트**
```javascript
const productIndex = products.findIndex(p => p.id === productId);
if (productIndex > -1) {
    products[productIndex] = updatedProduct;
}
localStorage.setItem('myShopProducts', JSON.stringify(products));
```

#### 5-4. 회원 관리 (member-list.html)

**회원 목록 테이블**
- ID, 이름, 이메일, 가입일 표시
- 탈퇴 처리 버튼

**회원 삭제 기능**
- confirm() 확인 후 삭제
- filter()로 해당 회원 제외 후 localStorage 업데이트

---

### 6. 데이터 관리

**localStorage 활용**
- `myShopProducts`: 상품 데이터 배열
- `myShopMembers`: 회원 데이터 배열

**초기 샘플 데이터 자동 생성**
```javascript
// 상품 샘플 데이터
const sampleProducts = [
    { id: Date.now(), name: 'Leopold FC660M PD', category: '키보드', price: 159000, ... },
    { id: Date.now() + 1, name: 'Varmilo VA87M', category: '키보드', price: 179000, ... },
    // ... 총 10개 상품
];

// 회원 샘플 데이터
const sampleMembers = Array.from({length: 10}, (_, i) => ({
    id: 100 + i,
    name: `김회원${i + 1}`,
    email: `member${i + 1}@example.com`,
    joined: `2025-07-${29 - i}`
}));
```

---

## CSS 스타일링

### CSS Custom Properties (변수)
```css
:root {
    --brand-color: #004d00;
    --font-serif: "Playfair Display", serif;
    --font-sans: "Noto Sans KR", sans-serif;
    --text-main: #333;
    --text-subtle: #888;
    --border-color: #eee;
}
```

### 디자인 특징
- **미니멀 디자인**: 깔끔한 여백, 단색 배경
- **타이포그래피**: 제목은 세리프(Playfair Display), 본문은 산세리프(Noto Sans KR)
- **일관된 컬러**: 브랜드 컬러(#004d00), 메인 텍스트(#333), 서브 텍스트(#888)
- **부드러운 인터랙션**: hover 시 색상 변경

---

## 상품 카테고리

| 카테고리 | 상품 예시 |
|----------|-----------|
| **키보드** | Leopold FC660M, Varmilo VA87M, Glorious GMMK Pro |
| **키캡** | GMK Olivia++, PBT Chalk |
| **스위치** | Cherry MX Red, Gateron Yellow |
| **액세서리** | Durock V2 스태빌라이저, 코일 케이블, 데스크 매트 |

---

## 학습 포인트

### JavaScript
- DOM 조작 (querySelector, createElement, innerHTML)
- 이벤트 핸들링 (addEventListener, preventDefault)
- 배열 메서드 (find, filter, findIndex, forEach, map)
- Web Storage API (localStorage, sessionStorage)
- URL 파라미터 처리 (URLSearchParams)
- JSON 직렬화/역직렬화 (JSON.stringify, JSON.parse)
- 타이머 함수 (setTimeout)

### CSS
- CSS Custom Properties
- Flexbox 레이아웃
- CSS 애니메이션 (@keyframes)
- 반응형 디자인

### Bootstrap
- Grid 시스템
- Form 컴포넌트
- Table 컴포넌트
- Dropdown 컴포넌트
- Accordion 컴포넌트
- Button 스타일링

---

## 실행 방법

1. nginx 서버 시작
2. 브라우저에서 `http://localhost/main.html` 접속
3. 관리자 로그인: `admin` / `1234`

---

## 향후 개선 사항

- [ ] 백엔드 연동 (Node.js/Spring Boot)
- [ ] 실제 DB 연결 (MySQL/Oracle)
- [ ] 장바구니 기능 완성
- [ ] 결제 시스템 연동
- [ ] 사용자 회원가입 기능 완성
- [ ] 상품 검색/필터 기능
- [ ] 상품 이미지 업로드 기능
