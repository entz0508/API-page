
## ERROR CODE <a id="error" href="#error">¶

### [에러코드정의] <a id="errorCode" href="#errorCode">¶
    // 일반
    '0'                         // 성공 , 에러없음
    '-1'                        // DB 결과 값 에러
    '-2'                        // 알수 없는 에러
    
    // 파라미터
    '-900000'                 // 유효한 파라미터가 아님
    
    // 계정
    '-200010'                // 계정 없음
    '-200012'                // 차단된 계정
    '-200001'                // 계정 중복
    '-200002'                // 계정 생성 실패
    '-200010'                // 계정 없음
    '-200012'                // 차단된 계정
    '-2000009'              // 엑세스 토큰 생성 실패

    // 아이템
    '-210001'               // 상품 정보 없음
    '-210301'               // 장바구니 등록 실패, 이미 구매한 상품
    '-210302'               // 장바구니 삭제 실패
    '-210401'               // 장바구니 구매 실패, 이미 구매하였거나 금액이 부족.
    '-210201'               // 신규 등록 실패 (상품 Like 등록)

    // 카드
    '-220001'               // 카드 정보가 유효하지 않음

    //장바구니
    '-210305'               //장바구니에 상품이 없음


* * *
