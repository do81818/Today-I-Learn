# Tabnabbing 이란?

- 여러 탭을 이용하여 웹 페이지를 보고 있으면 사용자가 눈치재지 못하는 사이 사용자가 원래 보고 있던 탭(부모 탭)을 변조한다.
- 이를 이용하여 실제 사이트와 유사한 피싱 페이지로 리다이렉트 할 수 있는 공격기법이다.
  - 링크의 target 속성이 \_blank 일때 새롭게 열린 탭에서 기존의 문서의 location을 피싱 사이트로 변경해 정보를 탈취
  - 메일이나 SNS와 같은 오픈 커뮤니티에서 사용 될 수 있음
  - 사용자의 클릭을 유도하여 웹 브라우저의 탭을 피싱 사이트로 이동시키는 기존의 피싱 기법과 달리 탭내빙은 사용자의 페이지에서 아무런 행위를 하지 않아도 탭 중 하나를 피싱 페이지로 로드할 수 있음

# 계정탈취 공격 시나리오

1. 사용자를 속일 수 있는 피싱 페이지를 메일로 전송
   - 사용자의 접속을 유도
2. 피싱 사이트로 접속 시 새 탭이 열리는 동시에 원래 페이지(부모 탭) 페이지 이동
   - 공격자가 해당 사이트 재 로그인 페이지와 비슷하게 만들어둔 피싱 페이지로 이동된다.
3. 원래 로그인 되어 있던 메일 사이트를 비슷하게 만들어둔 피싱 페이지로 이동
4. 사용자가 가짜 페이지에 재 로그인 시도
   - 해당 계정의 아이디와 패스워드가 공격자의 서버로 전송된다.
5. 로그인 된 사용자의 계정이 공격자의 서버로 이동
6. 실제 사이트로 리다이렉트

# 대응 방안

1. **noopener 추가**
   - 헤더와 함께 참조자 정보를 보내지 않도록 하기 때문에 window.opener 를 무효화 한다.
     - 따라서 window.opener를 통해 부모 탭을 참조할 수 없고, location과 같은 자바스크립트 요청을 거부한다.
2. **noreferrer 추가**
   - 링크를 통해 접근 시 포함된 referrer를 전송하지 않도록 하여 링크를 클릭하더라도 referrer 정보가 유출되지 않는다.
   - noopener 와 유사한 동작 구현
3. **nofollow 추가**
   - nofollo 는 사용 중인 브라우저에서 문서에 포함된 링크로 따라가지 않도록 지정하는 속성이다
   - 모든 링크를 따라가지 않게 하려면 meta 요소에 nofollow를 지정하고, 개별적으로 지정하려면 해당 요소의 href 속성에 지정한다.
     - noopener 속성을 사용하여 열린 탭은 부모를 호출할 일이 없고, 같은 스레드일 필요가 없기 때문에 사이트 변조가 거부됨과 더불어 새로운 페이지가 느리다고 부모 탭까지 느려질 일이 없어 효율적이다.
4. **data 속성 추가**
   - data-saferedirecturl 속성을 자동으로 부여해 공격자가 보낸 링크는 href 에 포함되어 있지만 해당 링크를 실제로는 구글에서 보낸 링크로 연결될 수 있도록 조치

# 결론

- `<a>` 태그에 추가적으로 **rel="noopener noreferrer"** 를 추가해야 한다.
- **target** 속성을 가질 수 있는 `<area>`,`<base>`,`<form>` 태그들도 마찬가지
- **window.open()** 으로 창을 여는 경우에도 **window.opener** 에 부모창 참조가 남으므로 이것을 막기 위해 **noopener** 파라미터를 추가해야 한다.

  ```jsx
  window.open("https://www.naver.com", "naver", "width=300, height=300, resizable, scrollbars, noopener");

  // 주의할 점
  // 1. 2번째 파라미터인 windowName을 명시해도 중복창이 뜨게 됨
  // 2. 크롬에서 noopener 파라미터를 추가할 경우 width와 height 선언을 무시하고 미리 설정된 기본 사이즈로 화면을 띄움 (파이어폭스는 정상 동작)
  // 3. noopener 옵션이 추가될 경우, window.open()의 반환값은 null 이므로 부모창 역시 새창 또는 새탭에 대한 제어가 불가능
  //    (새 창에서 창크기를 조정하는 스크립트가 추가되지 않으면 크롬에서는 원하는 크기의 창이 뜨지 않음)

  // 보안은 타협의 대상이 될 수 없음을 꼭 인지해야 한다.
  ```

---

링크 :

- [https://medium.com/@youngminhong/tabnabbing-공격-방어-대책-정리-9276ebf63f94](https://medium.com/@youngminhong/tabnabbing-%EA%B3%B5%EA%B2%A9-%EB%B0%A9%EC%96%B4-%EB%8C%80%EC%B1%85-%EC%A0%95%EB%A6%AC-9276ebf63f94)
- [http://www.igloosec.co.kr/BLOG_Tabnabbing 기법을 통한 계정 탈취?searchItem=&searchWord=&bbsCateId=0&gotoPage=18](http://www.igloosec.co.kr/BLOG_Tabnabbing%20%EA%B8%B0%EB%B2%95%EC%9D%84%20%ED%86%B5%ED%95%9C%20%EA%B3%84%EC%A0%95%20%ED%83%88%EC%B7%A8?searchItem=&searchWord=&bbsCateId=0&gotoPage=18)
