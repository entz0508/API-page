## __ADA API__

*DOC LIST*
* [ADA API](/fit_ada?pass=fitadavmfhwprxm!1)

*<strong>서버 to 클라이어트 결과</strong>*

	1. 응답 결과는 JSON 으로 전송한다
	2. 성공 실패는 res 값으로 구분한다.
       정상 : res == 0
       그외 : res != 0
	   예)  { "res":0 , ..... }
	3. 회원의 인증
		유져는 토큰을 디바이스에 저장해서 인증에 이용한다.
		일정기간내에 저장된 토큰을 보내면 그대로 인증된다.(account는 device당 1개의 토큰을 갖는다.)
		일정기간이 경과한 토큰에 대해서는 인정하지 않는다. 다시 로그인
		매처리시 토큰을 같이 보내고. 이를 유져의 키값으로 인증한다.
		다른 디바이스를 이용해서 로그인시 토큰을 신규 생성한다.
	4. 회원가입시 공통 체크
		clientUID                   length:0~64, 기기고유코드
		os                            enum('android','ios','web')
		accessToken              not null, length:40
	5. 로그인인증시 공통 체크
		clientUID                   length:0~64, 기기고유코드
		os                            enum('android','ios','web')
		accessToken              not null, length:40
