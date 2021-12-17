# 포인트페이 연동 가이드

**목차**
1. [업데이트](#)   
2. [실물가맹점](#)   
3. [포인트 전환 후](#)   
4. [담당자 정보](#)   

## 실물가맹점(링크)

### 결제 연동 가이드

#### 결제팝업 호출 방식(인증)

JS를 통한 팝업창 오픈 방식입니다.

|전달 파라미터|내용|
|------|---|
|`a_code`|매체코드(메크로스 발급아이디)|
|`user_id`|매체 회원아이디 (로그인을 완료한 회원 대상)|
|`add_param`|업체 요청 파라미터 (필요시 추가)|


> |URL|전송 방법|
> |------|:---:|
> |http://www.pointpay.im/pointhub/direct.php|GET|
>
> _*모든 파라미터명은 소문자를 사용합니다._
>
> **예시)**
> ```js
> window.open(
> 'http://www.pointpay.im/pointhub/direct.php?a_code=매체코드&user_id=매체회원아이디', 
> 'width=510, height=800, resizable=0, scrollbars=no, status=0, titlebar=0, toolbar=0, left=435, top=100' 
> );
> ```

## 포인트 전환 후(링크)

### 조회팝업 호출 방식(무인증)

> 현재 이용이 불가능 합니다.

JS를 통한 팝업창 오픈 방식입니다.

|전달 파라미터|내용|
|------|---|
|`a_code`|매체코드(메크로스 발급아이디)|
|`user_id`|매체 회원아이디 (로그인을 완료한 회원 대상)|
|`add_param`|업체 요청 파라미터 (필요시 추가)|
|`user_ci`|고객 CI|
|`user_name`|고객명|
|`user_phone`|고객휴대폰번호|

URL) http://www.pointpay.im/pointhub/direct_ci.php   
전송 방법) POST   

*모든 파라미터명은 소문자를 사용합니다.


예시)
```js
function onSubmit(){
    var pay_form = document.Form;
    var url = "http://www.pointpay.im/pointhub/direct_ci.php";
    window.open("" ,"Form", 
    "width=510, height=800, resizable=0, scrollbars=no, status=0, titlebar=0, toolbar=0, left=435, top=100"); 
    pay_form.action =url; 
    pay_form.method="post";
    pay_form.target="Form";
    pay_form.submit();
}
```
```html
<form name="Form">
    <input type="hidden" name="a_code" value="pointpay" />
    <input type="hidden" name="user_id" value="user_id" />
    <input type="hidden" name="add_param" value="add_param" />
    <input type="hidden" name="user_ci" value="user_ci" />
    <input type="hidden" name="user_name" value="user_name" />
    <input type="hidden" name="user_phone" value="user_phone" />
</form>
```

## 포인트 전환 후(링크)

전환 후 매체 포인트적립 처리(포인트, 캐시 등)를 위한 RETURN URL을 회신해주시면   
결제 정보를 전달드립니다. [담당자 정보]()(링크) 

예시)
http://매체도메인/event/result.php

전송 방법) POST
전달 파라미터)
rw_pay_type 등등

결과 응답코드를 Text값으로 반환해주세요.   
* 응답으로 JS의 alert 사용시 ERROR가발생하니, 참조해주세요.    
   
적립 성공인 경우 응답코드 : 201   
적립 실패인 경우 응답코드 : 202   

적립 실패인 경우 실패사유를 구분자 "|-|" (파이프라인 하이픈 파이프라인)과 함께 작성해주세요.   

예) 성공인 경우   
201   

예) 실패인 경우   
202|-|거래번호 중복   
202|-|아이디 없음   


- 담당자 정보


