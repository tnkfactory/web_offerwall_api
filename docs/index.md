## TNK 모바일 웹 충전소 연동 가이드 

### 1. 모바일 웹 충전소 URL
```
https://api3.tnkfactory.com/tnk/offerwall.web.main?md_user_nm={md_user_nm}&app_id={app_id}
```

| **매크로** | **설명** | **최대 길이** | **비고** |
| --- | --- | --- | --- |
| {app_id} | 매체 APP ID | string(32) | 매체별로 고정값(상수) 이다. (TNK 대시보드에서 확인 - App ID) |
| {md_user_nm} | 매체사의 회원ID (사용자 ID) | string(256) | Callback URL에 파라미터(md_user_nm)로 전달 된다. 포인트 적립 대상ID. |


  
### 2. Callback URL (Postback URL)

사용자가 광고 참여를 통하여 획득한 포인트를 매체사 서버로 실시간 전송 합니다.

호출방식 : HTTP POST

Parameters

| **파라미터** | **상세 내용** | **최대길이** |
| --- | --- | --- |
| seq_id | 포인트 지급에 대한 고유한 ID 값이다. URL이 반복적으로 호출되더라도 이 값을 사용하여 중복지급여부를 확인할 수 있다. | string(50) |
| pay_pnt | 사용자에게 지급되어야 할 포인트 값이다. | long |
| md_user_nm | 사용자 식별을 하기 위하여 전달되는 값이다.(1의 md_user_nm과 같다.) | string(256) |
| md_chk | 전달된 값이 유효한지 여부를 판단하기 위하여 제공된다. 이 값은 app_key + md_user_nm + seq_id 의 MD5 Hash 값이다. (App Key는 TNK대시보드에서 확인) | string(32) |
| app_id | 사용자가 참여한 광고앱의 고유 ID 값이다. | long |
| pay_dt | 포인트 지급시각이다. (System milliseconds) 예) 1577343412017 | long |
| app_nm | 참여한 광고명 이다. | string(120) |
| pay_amt | 정산되는 금액. | long |
| actn_id | - 0 : 설치형- 1 : 실행형- 2 : 액션형- 5 : 구매형 | int |

리턴값 처리

TNK 서버에서는 위 URL을 호출하고 HTTP 리턴코드로 200이 리턴되면 정상적으로 처리되었다고 판단합니다.
만약 200이 아닌 값이 리턴된다면 TNK 서버는 비정상처리로 판단하고 이후에는 5분 단위 및 1시간 단위로 최대 24시간 동안 반복적으로 호출합니다.

- 중요! 동일한 Request가 반복적으로 호출될 수 있으므로 seq_id 값을 사용하시어 반드시 중복체크를 하셔야합니다.
- Callback URL 등록은 TNK 대시보드에서 설정합니다.
- 연동관련 기술문의는 [tech@tnkfactory.com](mailto:tech@tnkfactory.com)으로 연락 바랍니다.
