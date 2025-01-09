### 오토레이아웃

<aside>
💡 단축키 : shift + A

</aside>

- 프레임 속 프레임이 됨
- 패딩값 조절
    <img width="733" alt="image" src="https://github.com/user-attachments/assets/2851f031-f962-4e1e-83ea-0565987402c9" />

    - 디폴트는 좌우/상하 묶어서 설정하도록 되어있음
    - 각각 상하좌우 설정하고싶으면 individual padding 설정

- Frame > Clip Content
    - 프레임 길이를 벗어나는 엘리먼트들은 가려지도록 설정

- Wrap
    - 반응형 페이지처럼 프레임 크기에 따라서 요소들 수직 수평 배치됨

- 버튼에 오토레이아웃 적용
    - 버튼 내부 텍스트를 길게 입력 > 컨테이너 길이가 길어짐
    <img width="380" alt="image" src="https://github.com/user-attachments/assets/0fe36273-bdce-468b-b65a-16308ff76a2b" />

    - 버튼에 크기가 큰 아이콘 삽입하기 : 아이콘 드래그 후 ctrl 눌러서 삽입

- 스마트 그리드 사용하기
    - 프레임 내에 분홍색 마크 뜨는걸로 요소들 조정하면 댐
    - 근데 그냥 오토 레이아웃 설정해서 조작하는게 편한거가틈

- `Ctrl + D`로 복붙해주면 요소 간격 그대로 유지되면서 복붙됨

# section3. 그룹, 프레임, 컴포넌트

[https://www.figma.com/design/xcYZl95Ol9PElEBDrSeArj/0731_%EA%B7%B8%EB%A3%B9-%ED%94%84%EB%A0%88%EC%9E%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%98%88%EC%A0%9C-(Copy)?node-id=0-1&t=g9c4HG5d69tDKzyM-0](https://www.figma.com/design/xcYZl95Ol9PElEBDrSeArj/0731_%EA%B7%B8%EB%A3%B9-%ED%94%84%EB%A0%88%EC%9E%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%98%88%EC%A0%9C-(Copy)?node-id=0-1&t=g9c4HG5d69tDKzyM-0)

- 그룹 : 콘텐츠를 하나의 그룹으로 묶는 기능. 그룹의 크기 색상 속성 모두 같이 변경됨
    
    <aside>
    💡 그룹 단축키 : ctrl + G
    
    </aside>
    
- 프레임 : 컨테이너 개념. 내부 콘텐츠의 속성, 크기에 영향 X
    
    <aside>
    💡 프레임 단축키 : ctrl + alt + G
    
    </aside>
    
- 컴포넌트 : 재사용 가능한 디자인 요소를 묶은 것
    - 컴포넌트를 사용하면 흩어져있는 요소들을 한번에 수정 변경 가능
    - 다른 페이지와 프로젝트에서 재사용 가능
    - 컴포넌트 라이브러리  → 빠른 생성 및 재사용
    
    <aside>
    💡 컴포넌트 단축키 : ctrl + alt + K
    
    </aside>
    

- 요소 위치 변경
    - 그룹 : 요소 위치를 바꿔도 그대로 그룹 유지
    - 프레임 : 요소 위치를 프레임 밖으로 빼면 그대로 프레임 외부 요소가 됨
    - 컴포넌트 : 요소 위치를 컴포넌트 밖으로 빼면 그대로 외부 요소가 됨

- Fill > 색상 변경
    - 그룹 : 안에 있는 전체 요소에 대한 색상을 변경
    - 프레임 : 프레임 자체 배경색을 변경
    - 컴포넌트 : 컴포넌트 배경색 변경

- 크기 변경
    - 그룹 : 요소의 크기도 비례해서 변경됨
    - 프레임 : 프레임 자체 사이즈만 변경, 요소 사이즈는 그대로
    - 컴포넌트 : 요소의 크기도 비례해서 변경됨

### 컴포넌트와 인스턴스

- 컴포넌트 : 재사용하도록 등록한 원본
- 인스턴스 : 원본 컴포넌트를 복제한 오브젝트
<img width="443" alt="image" src="https://github.com/user-attachments/assets/23f2b5f3-f954-4a4e-8b66-38a8e21a4c04" />
인스턴스를 복제해도 인스턴스임

- 컴포넌트와 인스턴스 비교
    - 컴포넌트 색상 변경 시 배경색만 변경
    - 컴포넌트 하위 요소 색상 or 모양 등 속성 변경 시 인스턴스 요소들도 변경됨
    - 인스턴스 속성 변경 시 혼자만 변경됨 → 속성 변경 시 해당 속성은 컴포넌트에 대해 독립적이게 됨
    
    > 컴포넌트에서 복제한 인스턴스의 색상, 크기, 형태,
    테두리 등의 속성을 변경하면 원본 컴포넌트의 속성이
    변경되어도 바뀐 인스턴스의 속성은 컴포넌트에서
    독립되어 영향을 받지 않음
    > 

- 컴포넌트 등록하기
    - 등록된 컴포넌트를 다른 곳에서 재사용 가능
    - Assets > 내가 생성한 컴포넌트가 노출됨 > 드래그 앤 드롭

- 컴포넌트 삭제하기
    - 걍 화면 내 모든 요소 delete하면 등록된거 삭제됨

- 개별 컴포넌트 등록
    - 요소 여러개 선택 > 상단 컴포넌트 아이콘 > Create multiple components 선택

- 버튼 + 텍스트 컴포넌트 등록했는데, 인스턴스에서 사이즈랑 색상은 독립적으로 변경되는데 텍스트 크기는 안되넴 ㅇ.ㅇ??

- 인스턴스 변경을 원본에 적용하기
    - 변경할 인스턴스 선택 > 오른쪽 메뉴 더보기 >
    <img width="700" alt="image" src="https://github.com/user-attachments/assets/cc9e0b60-5814-4ea5-9960-1df8ecffe857" />


- 인스턴스를 컴포넌트로 만들기
    - 인스턴스 우클릭 > Detach instance 선택
    - Detach된 인스턴스는 독립적인 개체가 됨
    <img width="370" alt="image" src="https://github.com/user-attachments/assets/e5b9e0eb-5830-475d-a275-5833f9aeee88" />

    - 해당 버튼 변경 후 컴포넌트로 등록하기
        
        ⇒ [Assets] > 해당 버튼은 [Design]에 속한 것을 알 수 있음
        
- 컴포넌트를 같은 종류 또는 같은 페이지에 모아두면 나중에 찾아서 다시 사용하거나 관리하기 편함

## Variant

컴포넌트의 형태는 같지만 색상 or 크기같은걸 다르게 쓰고싶을때 사용

스타일과 속성값이 다른 경우 유사한 종류의 컴포넌트를 세트로 묶어서 사용할 수 있는 기능

[https://www.figma.com/design/Y9lbFHcPRV6c59SXhEmow7/0801_%EB%B2%A0%EB%A6%AC%EC%96%B8%ED%8A%B8-%EC%98%88%EC%A0%9C-(Copy)?node-id=0-1&t=sHe6OJNC7PLDfe9P-0](https://www.figma.com/design/Y9lbFHcPRV6c59SXhEmow7/0801_%EB%B2%A0%EB%A6%AC%EC%96%B8%ED%8A%B8-%EC%98%88%EC%A0%9C-(Copy)?node-id=0-1&t=sHe6OJNC7PLDfe9P-0)

- 약간 분류 기능이넹 그래서 인스턴스 만들면 variant별로 선택해서 원하는 디자인 바로바로 쓸 수 있도록 하니까 좋구만
