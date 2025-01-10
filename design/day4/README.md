## 교재 6. 프로토타입의 활용

### 프레임 인터랙션

[https://www.figma.com/design/l8ML3p0mSzB40A2c79nHxQ/%ED%94%84%EB%A0%88%EC%9E%84-%EC%A0%84%ED%99%98-%EC%9D%B8%ED%84%B0%EB%9E%99%EC%85%98-(Copy)?node-id=1-4&t=B19I8Z4rwKMsyOB0-0](https://www.figma.com/design/l8ML3p0mSzB40A2c79nHxQ/%ED%94%84%EB%A0%88%EC%9E%84-%EC%A0%84%ED%99%98-%EC%9D%B8%ED%84%B0%EB%9E%99%EC%85%98-(Copy)?node-id=1-4&t=B19I8Z4rwKMsyOB0-0)

- 가장 빈번하게 사용되는 인터랙션 : 프레임과 프레임 간의 페이지 전환
    - 프레임 전환 인터랙션 : 같은 오브젝트가 다른 프레임에서 색상이나 그림자와 같은 변화로 페이지가 전환되는 인터랙션

- 우측 상단 Prototype Run버튼 클릭해서 실제 디바이스처럼 테스트해볼 수 있음

- 2개의 페이지가 있고 버튼 클릭 시 페이지(프레임) 이동이 일어나는 예제

### 베리언트로 스위치 인터랙션 제작하기

https://www.figma.com/design/VZwl1A6aD4xmFHITJZ0IdP/0802_%EB%B2%84%ED%8A%BC%EC%9D%B8%ED%84%B0%EB%9E%99%EC%85%98?node-id=0-1&t=7vAiIe3KbFQ9QbrQ-0

- effect : x축 y축 값은 4사분면 기준으로 주면 되는구먼. 요소가 (0,0)위치에 온다고 생각
- 2개 종류의 스위치를 만들어서 Create Component Set 선택하니까 알아서 Variant로 분류되넹 ㅇ0ㅇ
    - Variant 나뉜 버튼에 인터랙션 “change to” 등록
- 위 프레임 인터랙션 예제와 달리, 1개의 페이지로 설정되어있음
    - **프레임 전환 없이** 화면 내에서 버튼만 전환이 일어남!

### 캐러셀 인터랙션

https://www.figma.com/design/bSjKEAMUCItfQjs65oyr2O/0802_%EC%BA%90%EB%9F%AC%EC%85%80%EC%9D%B8%ED%84%B0%EB%9E%99%EC%85%98?node-id=0-1&t=XyaksBV3hFPdGPxs-0
<img width="772" alt="image" src="https://github.com/user-attachments/assets/cfdf7c94-f57a-4faa-8788-8a8fb6709c71" />

- 프레임 외부에 배치된 사각형 부분은 실제 화면에서 잘려서 노출됨
- 드래그 시 인터랙션 발생
- **드래그하는 중에 손가락 터치를 멈추고 떼면 특정 포인트로 다시 돌아가는 성질이 있음**

### 오버플로우 스크롤링 인터랙션

https://www.figma.com/design/4Zlhy2KBqOZwyhn4bAll7t/0802_%EC%98%A4%EB%B2%84%ED%94%8C%EB%A1%9C%EC%9A%B0-%EC%8A%A4%ED%81%AC%EB%A1%A4%EB%A7%81-%EC%9D%B8%ED%84%B0%EB%9E%99%EC%85%98?node-id=0-1&t=z82h38Hx8gkWzOmz-0

- **스크롤하는 중에 멈추면 그 위치 그대로 정지하는 성질이 있음**

- 프레임 내부 원 복제 : Ctrl + D
    
    ✓ 오토레이아웃이 지정되어 있어 원의 간격은 처음 원과의 간격을 그대로 유지한 채 복제됨
    

- Frame Seletion : 프레임이 오버플로우 될 영역 선택
    
    ※ 이 옵션을 선택하지 않으면 오버플로우 스크롤이 작동하지 않음 
    
- Scroll Behavior 설정
<img width="302" alt="image" src="https://github.com/user-attachments/assets/3cabfc12-0f08-412b-bf50-3759b6c549bb" />

### 모달 창 인터랙션

<aside>
💡 모달 창이란?

- 팝업 창 : 새로운 브라우저로 페이지를 띄우는 것

- 모달 창 : 특정 영역에 원하는 크기로 새로운 디자인을 띄우는 것 → 페이지 내에 레이어를 이용해 페이지를 띄움 
 ⇒ 부모 창이 사라지면 모달 창도 사라짐

</aside>

- 프로토타이핑 연결
    - 내비게이션 옵션 : [Navigate to] → [Open overlay]

- 원하는 위치에 모달 창 띄우기
    - 연결된 선 부분 클릭 > 위치 옵션 : [Centered] → [Manual]

- 모달 창 인터랙션 애니메이션 옵션
    - Move in, 화살표 (⬆️)
    - ease-out 세부 속성
    
    <aside>
    💡 ease-in : 천천히 부드럽게 시작하여 점점 빠르게 속성값에 도달 
    ease-out : ease-in과 반대로 빠르게 시작하여 점점 천천히  속성값에 도달
    
    </aside>
    

- 모달 창 외부 터치 시 모달 창이 닫히게 만들라면 ‘Close when clicking outside’를 체크
    <img width="588" alt="image" src="https://github.com/user-attachments/assets/2116d179-6331-4779-aa31-2c4ad0193010" />


- 모달 창 닫기 클릭 시 인터랙션 옵션
    - 애니메이션 옵션 : [Move out], 화살표 (⬇️)

https://www.figma.com/design/MJJqKm9dg0UST45ScZ3qGB/0802_%EB%AA%A8%EB%8B%AC-%EC%B0%BD-%EC%9D%B8%ED%84%B0%EB%9E%99%EC%85%98?node-id=1-2&t=EnJPbr313TbbikN5-0

### 내비게이션 드로워 인터랙션

https://www.figma.com/design/k7vqM332KbplFToUmuZyqR/0802_%EB%82%B4%EB%B9%84%EA%B2%8C%EC%9D%B4%EC%85%98-%EB%93%9C%EB%A1%9C%EC%9B%8C-%EC%9D%B8%ED%84%B0%EB%9E%99%EC%85%98?node-id=0-1&t=ZrtMhxSl9c7bc9qs-0

<aside>
💡 내비게이션 드로워란?
- 화면의 왼쪽 가장자리에서 서랍처럼 열리고 닫히는 메뉴바같은 패널
- 평상시에는 화면 밖에 배치되어 보이지 않지만 메뉴 항목을 눌렀을 때 슬라이딩되면서 나타남

</aside>

- 드로워 위치 : Top Left or Bottom으로 설정
