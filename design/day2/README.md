### UI(사용자 인터페이스)

- 모든 시각적 디자인 요소 - 버튼, 아이콘, …
- 사용자가 제품과 쉽게 상호작용하고 정보를 전달받을 수 있도록 하는 것

### UX(사용자 경험)

- 사용자가 제품을 사용하면서 느끼는 전체적인 경험
- 사용자가 제품 사용 시 편안하고 만족스러운 경험을 제공하기 위함
- 어떻게 사용하는지 공부할 필요 없이 쉽고 알기 쉽게 제공

### UI/UX

- 사실 나눌 필요 없음

---

## 교재 - 디자인 시스템 실무 with 피그마

### 디자인 시스템

- 재사용 가능한 디자인 구성 요소의 모음
- 제품과 서비스 품질에 일관성을 유지하고 효율적으로 운영하기 위해 구축
- 장점
    - 시각적 일관성
    - 기능적 일관성
    - 내부 일관성 : 새로운 기능이나 페이지 추가 시 일관적인 디자인 규칙에 따라 제작한다면 사용자는 새로운 방식을 학습할 필요가 없음
    - 외부 일관성 : 보편적인 UI원칙
    - 브랜드 구축
    
- 구조
    - 파운데이션 : 기초 디자인 요소 모음
    - 컴포넌트 : 재사용 가능한 구성요소
    - 패턴
    - 기타
    
- 스타일가이드
    - 색상
    - 타이포그래피 : 서체 모음
    - 아이콘

### 서비스 디자인

- 디자인 전에 일단 어떤 서비스를 만들 것인지부터 정해야 함!!
    - 일상에서 발견한 문제에 먼저 접근해보기
- 디자인 : 사용자의 만족을 위한 계획을 세우는 과정
- 실제 실행 가능한 형태로 구체화 해야 함
- 서비스 디자인 프로세스
    - 더블 다이아몬드 방법론 : 발견 > 구체화 > 아이디어 제안/개발 > 해결책 도출
    <img width="764" alt="image" src="https://github.com/user-attachments/assets/3082c131-29cb-4641-aa47-d2fd500f736f" />

    - 애자일 프로세스 : 요구사항을 끊임없이 반영하고 점진적으로 제품을 업데이트
    <img width="768" alt="image" src="https://github.com/user-attachments/assets/48bd8bc0-a20b-499d-9807-70fe61df5284" />


---

## 피그마

- 실시간 협업 툴
- 스타일 : 색상, 이펙트, 텍스트, 그리드의 네가지 스타일 요소를 지정하여 등록한 뒤 반복 사용
- alt + 드래그 : 복제
- alt + shift + 드래그 : 수평 수직 복제
- 사각형 선택하고 > shift + 드래그 : 정사각형 생성

### 디자인 토큰 시스템

- 토큰 : 디자인 토큰을 플러그인으로 사용하는 방법론
    - **서비스에서 사용할 모든 컬러, 크기, 폰트** 등 정적인 값들을 변수화
- 토큰 플러그인 설치하기
    - Tokens studio for Pigma > Run
    <img width="749" alt="image" src="https://github.com/user-attachments/assets/0dacc6a9-5ffc-41b6-b493-3adff0227214" />


### 컨스트레인트

[https://www.figma.com/design/ZtbXkFXFtCjGcMvwCx6W33/%EC%BB%A8%EC%8A%A4%ED%8A%B8%EB%A0%88%EC%9D%B8%ED%8A%B8-%EC%98%88%EC%A0%9C-(Copy)?node-id=1-2&t=9VAN365w3eNe5Qgg-0](https://www.figma.com/design/ZtbXkFXFtCjGcMvwCx6W33/%EC%BB%A8%EC%8A%A4%ED%8A%B8%EB%A0%88%EC%9D%B8%ED%8A%B8-%EC%98%88%EC%A0%9C-(Copy)?node-id=1-2&t=9VAN365w3eNe5Qgg-0)

- 반응형에 맞게 설정할 수 있음
    - Top: 프레임의 위치에 오브젝트를 고정
    - Bottom: 프레임 아래쪽 위치에 오브젝트 위치 고정
    - Top and Bottom : 오브젝트 사이즈와 상하 프레임에 상대적으로 고정. Y축을 따라 늘어나고 줄어듬
    - Center : Y축 센터에 고정
    - Scale : 프레임 사이즈에 따라 오브젝트 비율과 사이즈 결정
<img width="787" alt="image" src="https://github.com/user-attachments/assets/a13879da-35af-4638-8032-24272d6a50a0" />

아이콘 - left or right

이미지 - left and right / top and bottom, 흠 근데 scale로 해도 ㄱㅊ네

긴 텍스트 - left and right ↔ center로 했을때 차이 비교

### 오토레이아웃

[https://www.figma.com/design/H99V0OE4vRPRsTr9B3U3A3/%EC%98%A4%ED%86%A0%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83-%EC%98%88%EC%A0%9C-(Copy)?node-id=0-1&t=f1Qe54pgXkRcC6J0-0](https://www.figma.com/design/H99V0OE4vRPRsTr9B3U3A3/%EC%98%A4%ED%86%A0%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83-%EC%98%88%EC%A0%9C-(Copy)?node-id=0-1&t=f1Qe54pgXkRcC6J0-0)

디자인 요소간 관계를 유지하면서 화면크기 or 기기 방향에 따라 조정되어 일관적인 레이아웃을 유지하게 해줌

사각형 만들고 오른쪽 버튼 > add auto layout
