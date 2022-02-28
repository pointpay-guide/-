# 📝실물가맹점 연동 가이드

- [실물가맹점](#실물가맹점-연동-가이드)   
- [컨텐츠가맹점](../../../guide-2/#컨텐츠가맹점-연동-가이드)   
- [담당자 정보](../../../Responsibility/#담당자-정보)   

---

## 목차
- [업데이트](#업데이트)   
- [포인트 전환 - 인증](#포인트전환---인증)   
- [포인트 전환 - 무인증](#포인트-전환---무인증)   
- [포인트 전환 후](#포인트-전환-후)   

<br/>

## 업데이트
- 21.12.17 연동가이드 제작
- 22.02.04 무인증조회 추가

<br/>

## 포인트전환 - 인증

#### 결제팝업 호출

JS를 통한 팝업창 오픈 방식입니다.

|서버|URL|전송 방법|
|--|------|:---:|
|개발|http://dev.pointpay.im/pointhub/direct|GET|
|상용|https://ssl.pointpay.im/pointhub/direct|GET|


|전달 파라미터|내용|필수여부|
|------|---|---|
|`a_code`|매체코드(포인트페이 발급아이디)|O|
|`user_id`|매체 회원아이디 (로그인을 완료한 회원 대상)|O|
|`add_param`|업체 요청 파라미터 (필요시 추가)||


 _*모든 파라미터명은 소문자를 사용합니다._


 **예시)**
 ```js
 window.open(
 'https://ssl.pointpay.im/pointhub/direct?a_code=매체코드&user_id=매체회원아이디', 
 'cert2_popup', 'width=765, height=895, resizable=auto, scrollbars=yes, status=no'
 );
 ```
 
<br/>


## 포인트 전환 - 무인증

### 1. 조회팝업 호출

JS를 통한 팝업창 오픈 방식입니다.

|서버|URL|전송 방법|
|--|------|:---:|
|개발|http://dev.pointpay.im/pointhub/direct_ci|POST|
|상용|https://ssl.pointpay.im/pointhub/direct_ci|POST|

|전달 파라미터|내용|필수여부|
|------|---|---|
|`a_code`|매체코드(포인트페이 발급아이디)|O|
|`user_id`|매체 회원아이디 (로그인을 완료한 회원 대상)|O|
|`add_param`|업체 요청 파라미터 (필요시 추가)|

 _*모든 파라미터명은 소문자를 사용합니다._
 
  **예시)**
```js
function onSubmit(){
    var pay_form = document.Form;
    var url = "https://ssl.pointpay.im/pointhub/direct_ci";
    window.open("" ,"Form", 
    "width=765, height=895, resizable=auto, scrollbars=yes, status=no"); 
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
</form>
```

<br/>

### 2. 서버간 연동(S2S)

고객 중요정보는 서버간 연동을 통해 전달 받습니다.

#### URL : 가맹점에서 알려준 URL
### REQUEST
|파라미터|내용|필수여부|
|------|---|---|
|`token`|업체 전달 key값(md5암호화)|O|
|`en_user_id`|고객아이디(md5암호화)|O|

> token 값과 en_user_id 값으로 유효성체크를 해주세요.

### RESPONSE
> time out : 5초
>
|파라미터|내용|필수여부|예시|
|------|---|---|---|
|`ret_code`|리턴코드|O|성공 : 201 / 실패: 202|
|`ret_msg`|리턴메시지||실패일경우 전달|
|`user_ci`|고객 CI|O||
|`user_name`|고객명|O|박주영(UTF-8)|
|`user_phone`|고객휴대폰번호|O|01012345678|


> 고객 중요정보값이 넘어오지않거나 202응답을 받으면 포인트 전환이되지 않습니다.

<br/>

## 포인트 전환 후

전환 후 매체 포인트적립 처리(포인트, 캐시 등)를 위한 RETURN URL을
[담당자](../../../Responsibility/#담당자-정보) 에게 전달해주시면 됩니다.


 **예시)**
```
http://매체도메인/event/result.php
```

|전달 파라미터|설명|타입|예시|
|------|--------|------|------|
|`rw_pay_type`|결제구분|Varchar|KT|
|`rw_user_id`|매체 회원아이디|Varchar|pointpay
|`rw_price`|전환포인트|Int|10000
|`rw_reg_datetime`|거래시간|Date|2021-12-17 11:25:36
|`rw_a_code`|거래매체코드|Varchar|포인트페이 발급아이디
|`rw_orderno`|거래번호|Varchar|ME2021121733235510625855
|`utc_code`|매체 요청파라미터|Varchar|ETC

 _*모든 파라미터명은 소문자를 사용합니다._

### 상태 리턴

결과 응답코드를 Text값으로 반환해주세요.   
> 응답으로 JS의 alert 사용시 ERROR가발생하니, 참고해주세요.    
   
</br>

#### 적립 성공인 경우 응답코드
```
201
```

#### 적립 실패인 경우 응답코드 

적립 실패인 경우 실패사유를 구분자 "|-|" (파이프라인 하이픈 파이프라인)과 함께 작성해주세요.   
```
202|-|거래번호 중복
202|-|거래번호 중복   
202|-|아이디 없음
```



