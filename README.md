# 비개발자가 예식장 계약날에 모바일청첩장 개발/배포하기 

<p align="center">
<img width="1900" height="1038" alt="image" src="https://github.com/user-attachments/assets/5f346cb3-1715-4cac-b854-8ed99be4baa5" /></p>
<br>


결혼식을 꽤나 급하게 진행하게 되었습니다.

어제 예식장 투어를 진행하며 눈여겨보고 있고 마음에 쏙 들던 예식장이 올해 크리스마스 주간을 제외하곤 한자리만 남은 상태이기에 4개월이 조금 넘게 남은 말도 안되는 기간을 앞두고 계약을 결정하게 되었습니다. 

그래서 어차피 하게 될 일 중 하나인 모바일 청첩장 만들기를 AI Agent들과 함께 미리 해보겠다고 호기롭게 시작해 보았습니다.

### 📅 진행 일정

* **2/29 오후** - 웨딩홀 투어
* **3/1 오전** - 웨딩홀 계약
* **3/1 오후** - 모바일 청첩장 완성 후 배포하기
* **3/2 새벽** - 트러블슈팅 리뷰하며 블로그 글 정리


<br>

> **살짝 타이틀을 달아본다면,,**

## 하루 만에 Antigravity+Kimi로 모바일 청첩장 만들고 카카오링크 API와 Open Graph 태그 설정 완료한 Vercel 커스텀 도메인으로 배포까지 마무리 🎉


<br>

AI 개발 진행은 주로 **Google Antigravity의 Claude Opus 4.6**으로 했지만, **Kimi agent**와 협상해서 뻥 뜯어낸 Moderato 플랜으로 웹사이트 초안을 구성했습니다.

Kimi가 website를 생각보다 잘 만들고, HTML 컴포넌트를 직접 누르며 수정하는 **Select Mode**를 지원해서 더 정확하고 편리하게 개발이 가능하더라구요. 참고로 개발 막바지엔 Opus 4.5 사용량을 다 써버리는 바람에, 남은 작업은 **Gemini 3.1 Pro**로 이어서 진행했습니다.

최종 구현 페이지 스크린샷은 가장 하단에 있고, 스튜디오 사진만 완성본으로 추후 수정하면 될 것 같습니다. 개발 직군이 아니니 구현 자체보다는 **플랫폼 간 기술적 호환성 파악 및 트러블슈팅**에 초점을 둔 프로젝트라고 생각해 주시면 좋겠습니다.

<br>

---


# ① Implementation Details (상세 구현)

<p align="center">
<img width="1830" height="1369" alt="image" src="https://github.com/user-attachments/assets/75266eba-262a-45e7-bee3-d0b5fe3bbd3c" />
<em>Google Antigravity</em></p>

<p align="center">
<img width="1906" height="984" alt="image" src="https://github.com/user-attachments/assets/4fdf2289-6362-4f26-b179-6e20faa232b2" />
<em>Kimi (Select mode)</em></p>

---

### ✅ 구현 메인 스펙

구현한 메인 스펙은 아래와 같습니다.

* **🖼️ 전체 섹션과 갤러리 구성**
  * 메인 커버, 인사말, 신랑신부 소개, 예식 정보, 마음 전하실 곳, 위치 안내까지 흐름을 끊김 없이 완성

* **📅 실시간 D-Day 카운터**
  * 결혼식까지 남은 시간을 실시간으로 보여주는 카운트다운 컴포넌트 탑재

* **🗺️ 편리한 위치 안내 (Maps Integration)**
  * Google Map 임베드로 예식장 위치 바로 확인하고, 네이버 지도 딥링크 버튼을 제공하여 하객들의 길 안내 편의성 극대화

* **🐎 기타 정보 섹션 (Carousel & Accordion)**
  * 셔틀/주차 같은 안내 정보는 좌우 캐러셀(Carousel) 형태, 계좌 정보는 접이식(Accordion) UI로 모바일에서 읽기 편하게 제작

* **💬 원클릭 카카오톡 공유 (Kakao SDK)**
  * 청첩장 안에서 Custom 썸네일과 함께 카카오톡으로 즉시 공유 가능 버튼 추가

* **📷 Guest 사진 업로드**
  * Firebase Storage 연동하여 하객들의 시선에서 직접 촬영한 사진을 쉽게 업로드하여 공유할 수 있는 기능 구현

---

그리고 시간을 주로 잡아먹은 핵심 문제는 **공유하기**였습니다.
카카오톡이 주요 공유 수단이 될 것임이 명확하기에 '링크 복사' 버튼만 제공하는 것이 아닌 **'카카오톡 공유'** 버튼을 끝까지 고집하였고, 수없이 트러블슈팅한 결과 완성했습니다.

<p align="center">
<img width="1900" height="988" alt="image" src="https://github.com/user-attachments/assets/862addff-863f-4dc3-b079-d999fe2940b3" /></p>

<p align="center">
<img width="1900" height="985" alt="image" src="https://github.com/user-attachments/assets/5aa59dbf-eadc-48cb-8d8d-d8a24216172f" /></p>




[Kakao Developers](https://developers.kakao.com/)를 처음 쓰다 보니 감이 잘 안 오는데, 썸네일이 생각보다 한 방에 안 잡히더라고요. 포인트는 썸네일이 1종류가 아니라 2종류라는 점이었습니다.

 **1. 링크를 그냥 복사/붙여넣기 했을 때 보이는 썸네일**
 
   → OG(Open Graph) 태그
 
 **2. 카카오톡 공유하기 버튼을 눌러 공유할 때 보이는 썸네일**
 
   → 카카오링크 API 설정([메시지 템플릿](https://developers.kakao.com/tool))

<p align="center">
<img width="430" height="300" alt="image" src="https://github.com/user-attachments/assets/7ffa1e61-5d37-4c4d-a6e1-fa6466c8fbb5" /><br>
<em>왼쪽은 채팅방에서 클릭과 딥링크도 되지 않는 상태, 우측이 최종 메시지 템플릿 적용본</em></p>


<p align="center">
<img width="430" height="100" alt="image" src="https://github.com/user-attachments/assets/725abcf8-e13f-44c9-bd6c-263ab39466cb" /><br>
<em>다양한 썸네일 구조</em></p>


특히 두 번째가 처음이라 제일 시간이 걸렸습니다. Kakao Developers에서 신규 앱 만들고, JavaScript Key 설정하고, 웹 도메인 등록하고, 메시지 템플릿 빌더에서 공유 템플릿 세팅하고, 컴포넌트 링크 연결, 공유 디버거까지.. 카카오톡에서 수 없이 메시지를 보내고 지우고 반복했네요.


<p align="center">
<img width="340" height="1645" alt="image" src="https://github.com/user-attachments/assets/1158fbfd-8f90-4d18-9e03-11ca9934bb9a" /><br>
<em>수 많은 테스트</em></p>


Deployment 쪽은 오히려 쉬웠습니다. [Vercel](https://vercel.com/)이 GitHub repo 연결해서 호스팅 하는 경로가 워낙 편리해서 너무 잘 쓰고 있고, Vercel에서 구매한 .space 도메인으로 deploy한 버전과 dev 버전을 크롬 분할 뷰로 띄워두고 코드를 조금씩 수정해 나아갔습니다.


<p align="center">
<img width="1900" height="989" alt="image" src="https://github.com/user-attachments/assets/0b3fbbe5-29f5-4bdb-9cf3-87a82caabaaf" /></p>


<p align="center">
<img width="1900" height="983" alt="image" src="https://github.com/user-attachments/assets/22751e0d-d0ef-4392-b3dc-173431fc704a" /></p>


<p align="center">
<img width="1900" height="988" alt="image" src="https://github.com/user-attachments/assets/ac9e6053-0bbc-4de1-bb9f-ac3106df722b" /></p>



하단 구현 스크린샷에 나오겠지만, 요즘 모청 하단에 신랑신부에게 사진을 즉시 보낼 수 있는 기능도 구현해두는 게 트렌드인 것 같더라고요. 그래서 저도 이 기능 구현을 위해 [Firebase](https://console.firebase.google.com/) Storage 인스턴스를 생성했고, 테스트를 몇 회 진행해보며 실제로 서버까지 잘 들어오는 걸 확인했습니다. 하객분들께서 소중히 보내주시는 사진들이 업체 실수로 날아가서 엄청 속상하다는 글도 본 적이 있어서 이 부분은 끝까지 꼼꼼히 잘 확인해야 할 것 같아요. 


<p align="center">
<img width="1920" height="993" alt="image" src="https://github.com/user-attachments/assets/24f0ad12-0252-49e7-a17d-32771e26636a" /></p>


아무튼 이로써 약 반나절 이상을 투자한 개발 일지는 끝났고, 폭등하게 된 저의 Github Contribution graph를 보며 마무리하겠습니다.


<p align="center">
<img width="1125" height="923" alt="image" src="https://github.com/user-attachments/assets/4e678ede-36f5-421f-8b05-e09f6514a1d8" /></p>

---
<br>

# ② Final Results
최종 구현 *(최종 사진은 추후 변경 예정)*

<p align="center">
<img width="420" height="934" alt="image" src="https://github.com/user-attachments/assets/80c25dcf-4ac5-4840-a75e-64767db81b64" /><br>
<img width="420" height="665" alt="image" src="https://github.com/user-attachments/assets/b215f3a4-c76f-4af2-8c96-eb56ae47dc0d" /><br>
<img width="420" height="515" alt="image" src="https://github.com/user-attachments/assets/be3c308d-e38c-4c68-8df5-15e048e3b27c" /><br>
<img width="420" height="820" alt="image" src="https://github.com/user-attachments/assets/c94bb845-08ed-4897-8f62-4a451792026b" /><br>
<img width="420" height="806" alt="image" src="https://github.com/user-attachments/assets/eedbab62-1b95-475e-8852-884bed50c8bb" /><br>
<img width="420" height="926" alt="image" src="https://github.com/user-attachments/assets/61b20a0f-c26d-42e7-addf-caca8b41856f" /><br>
<img width="420" height="575" alt="image" src="https://github.com/user-attachments/assets/29dea14a-83d4-4e09-9220-685271ea03ee" /><br>
<img width="420" height="803" alt="image" src="https://github.com/user-attachments/assets/870038fe-f900-4806-b135-ce02184a2878" /><br>
<img width="420" height="626" alt="image" src="https://github.com/user-attachments/assets/77f27014-6480-4593-9ccc-74ddad9e7478" /><br>
<img width="420" height="374" alt="image" src="https://github.com/user-attachments/assets/de541a94-f88e-48cc-847f-71bec730a480" /></p>          


<br>
<br>
<br>
<br>


# ③ Appendix.

## 📂 프로젝트 구조

```text
wed_invi/
├── index.html                  # 엔트리 HTML (Kakao SDK 및 OG(Open Graph) 태그 포함)
├── package.json                # 주요 패키지 의존성 및 스크립트 관리
├── vite.config.ts              # Vite 기본 설정
├── tailwind.config.js          # 커스텀 웨딩 도메인 테마(컬러, 폰트, 애니메이션) 설정
├── public/                     # 정적 이미지 및 에셋 (최종 웨딩 스튜디오 사진 교체 대상 폴더)
└── src/
    ├── main.tsx                # React 애플리케이션 엔트리포인트
    ├── App.tsx                 # 최상위 컴포넌트, Toast 프로바이더 및 메인 래퍼
    ├── index.css               # 글로벌 여백/타이포 및 Tailwind Base
    ├── lib/                    # 유틸리티 함수 (cn, firebase 등)
    ├── hooks/                  # 커스텀 React Hooks (use-mobile 등)
    ├── components/ui/          # 재사용 가능한 UI 컴포넌트
    └── sections/               # 청첩장 도메인별 기능 컴포넌트
        ├── HeroSection.tsx       # 1. 메인 커버 사진 및 일시
        ├── IntroSection.tsx      # 2. 초대 인사말
        ├── CoupleSection.tsx     # 3. 신랑/신부 및 혼주 연락처
        ├── CeremonySection.tsx   # 4. 예식장 등 세부 정보
        ├── GallerySection.tsx    # 5. 본식 스튜디오 이미지 갤러리
        ├── LocationSection.tsx   # 6. 구글맵, 지도 앱 딥링크
        ├── InfoSection.tsx       # 7. 주차 및 셔틀 버스 등
        ├── GiftSection.tsx       # 8. 축의금 계좌 및 정보
        ├── GuestPhotoSection.tsx # 9. 하객들이 남기는 촬영 사진 
        ├── ShareSection.tsx      # 10. SNS를 통한 모바일 링크 공유
        └── FooterSection.tsx     # 11. 하단 정리용 푸터 영역
```

## 🛠️ 기술 스택

### Frontend 핵심 기술

- Framework: React 19 + TypeScript

- Build Tool: Vite 7

- Styling: Tailwind CSS 3.4

- UI Components: Radix UI Primitives, shadcn/ui

​

### 주요 라이브러리 및 서비스

- Icons: Lucide React

- Toast Notifications: Sonner

- BaaS / Backend: Firebase (Analytics, Storage)

- Social: Kakao SDK (카카오톡 공유 기능)

- Fonts: Noto Serif KR(명조), Noto Sans KR(고딕) 등

​
