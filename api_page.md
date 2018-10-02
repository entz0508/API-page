
# __ADA API__

## DOC LIST
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
		clientUID                length:0~64, 기기고유코드
		os                       enum('android','ios','web')
		accessToken              not null, length:40
	5. 로그인인증시 공통 체크
		clientUID                length:0~64, 기기고유코드
		os                       enum('android','ios','web')
		accessToken              not null, length:40


*<strong>개발 서버 주소</strong>*

                DEV Server IP : https://apidev.doralab.co.kr (MySQL)
                ADB DEV Server IP : 35.194.222.5 (Node API)

*<strong>API 목록</strong>*

### [Account](#Account)
1. 회원가입 - [/ada/account/join](#/ada/account/join) - POST, 작업 완료.
2. 로그인 - [/ada/account/login](#/ada/account/login) - POST, 작업 완료.
3. 계정정보 가져오기 - [/ada/account/getInfo](#/ada/account/getInfo) - POST, 작업 완료.
   
### [Boutique](#Boutique)
1. 카테고리 리스트(브랜드 카테고리 리스트) - [/ada/boutique/categoryList](#/ada/boutique/categoryList) - POST , 작업 완료.
2. 브랜드 리스트 - [/ada/boutique/brandList](#/ada/boutique/brandList) - POST , 작업 완료.
3. 브랜드 디테일 - [/ada/boutique/brandDetail](#/ada/boutique/brandDetail) - POST , 작업 완료.
4. 배너 리스트 - [/ada/boutique/bannerList](#/ada/boutique/bannerList) - POST, 작업 완료.
5. 상품 리스트 - [/ada/boutique/productList](#/ada/boutique/productList) - POST, 작업 완료.
6. 상품 상세보기 - [/ada/boutique/productDetail](/ada/boutique/productDetail) - POST, 작업 완료.
7. 피쳐드 리스트 - [/ada/boutique/featuredList](#/ada/boutique/featuredList) - POST, 작업 완료.
8. 피쳐드 디테일 - [/ada/boutique/featuredDetail](#/ada/boutique/featuredDetail) - POST, 작업 완료.
9. 컨텐츠 뷰어 - [/ada/boutique/contentsView](#/ada/boutique/contentsView) - POST , 작업 완료.
10. 태그 리스트 - [/ada/boutique/tagList](#/ada/boutique/tagList)  - POST, 작업 완료

### [Wish](#Wish)
1. wish 등록/삭제 - [/ada/wish/edit](#/ada/wish/edit) - POST, 작업 완료.
2. wish 리스트 - [/ada/wish/list](#/ada/wish/list) - POST, 작업 완료.
3. like 등록/삭제 - [/ada/like/edit](#/ada/like/edit) - POST, 작업 완료.

### [Cart](#Cart)<br>
1. 장바구니 등록/구매처리 - [/ada/cart/edit](#/ada/cart/edit) - POST, 작업 완료
2. 장바구니/구매 리스트 - [/ada/cart/list](#/ada/cart/list) - POST, 작업 완료
3. 장바구니 삭제 처리 - [/ada/cart/delete](#/ada/cart/delete) - POST, 작업 예정
4. 장바구니 전체 구매 - [/ada/cart/buyAll](#/ada/cart/buyAll) - POST, 작업 예정

### [Wardrobe](#Wardrobe)
1. 옷장 리스트 - [/ada/wardrobe/list](#/ada/wardrobe/list) - 상위 구매리스트와 상동.
2. 아이템 착용 / 해제 - [/ada/wardrobe/wear](#/ada/wardrobe/wear) - 추후 작업 예정 - 1

### [Card](#Card) -- API Mockup
1. 프레임 리스트 - [/ada/card/frameList](#/ada/card/frameList) - POST, 작업 중
2. 카드 리스트 - [/ada/card/cardList](#/ada/card/cardList) - POST, 작업 중
3. 좋아요 - [/ada/card/like](#/ada/card/like) - POST, 작업 중
4. 별점 - [/ada/card/star](#/ada/card/star) - POST, 작업 중
5. 버튼 - [/ada/card/button](#/ada/card/button) - POST, 작업 중

### [ETC](#Etc)
1. api서버 통신테스트 - [/ada/etc/test](#/ada/etc/test) - POST ,완료.

### [ERROR CODE](#errorCode)<br>
* * * * *


## Account <a id="Account" href="#Account">¶</a>

### 1. 회원가입 [/ada/account/join] / POST <a id="/ada/account/join" href="#/ada/account/join">¶</a>

*info*
    기능 : ADA 회원가입
    수정예정 : 입력 파라미터가 추가 될 수 있음. 출력 결과 값의 개인정보 노출 부분이 변경 될 수 있음.
    확인사항 : signup_id 값 중복 불가.


*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        - 이하 contents var -
        signup_path : string            // 가입 경로 enum('google','facebook','guest')
        signup_id : string              // 가입 경로의 유져ID(sns user Key) 또는 guest 일 경우 client_id 값
        nick_name : string              // 유저 닉네임
        country : string                // 국가(iso국가코드) ex.'KO'
        age : int                       // 나이(가입연령코드) ex. 0
        gender : int                    // 성별(남자 1,여자 2) ex. 2
        head : int                      // 머리 사이즈(0~10) ex. 0
        arm_l : int                     // 팔 사이즈(0~10) ex. 0
        arm_b : int                     // 팔 사이즈 (0~10) ex. 0
        leg_l : int                     // 다리 사이즈(0~10) ex. 0
        leg_b : int                     // 다리 사이즈(0~10) ex. 0
        skin : int                      // 피부(0~10) ex. 0
        top : int                       // 상체(0~10) ex. 0
        hair : int                      // 헤어 타입(0~10) ex. 0
        bottom : int                    // 하체(0~10) ex. 0
        shoes : int                     // 신발 타입(0~10) ex. 0

*return value*

    // 성공
    {
    "res": 0,
    "data": {
        "userInfo": {
            "accountID": 100000112,
            "accessToken": "8ce64bff119be83f25bd84954d64f0889a447a41"   // accessToken
        }
    }
}

    //실패
    {
        "res": int,                                                                 // 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중


### 2. 로그인 [/ada/account/login] / POST <a id="/ada/account/login" href="#/ada/account/login">¶</a>

*inf*o

    기능 : ADA 회원 로그인 이후 유저 계정 정보를 리턴 값으로 노출
    수정예정 : 입력 파라미터가 추가 될 수 있음. 출력 결과 값의 개인정보 노출 부분이 변경 될 수 있음. 


*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        - 이하 contents var -
        signup_path : string            // 가입 경로 enum('google','facebook','guest')
        signup_id : string              // 가입 경로의 유져ID(sns user Key) 또는 guest 일 경우 client_id 값

*return value*
>
>    // 성공
>    {
>    "res": 0,
>    "data": {
        "userInfo": {
            "accountID": 100000112,
            "accessToken": "454ab6a16cafb992fed6f50dd7fb4a7a5c907835",
            "nickName": "adatester",
            "birthDay": "2018-08-16 00:00:00",
            "gender": 2,
            "country": "KO",
            "profileImage": null,
            "age": 0,
            "head": 1,
            "armL": 0,
            "armB": 0,
            "legL": 1,
            "legB": 1,
            "skin": 1,
            "hair": 2,
            "top": 1,
            "bottom": 1,
            "shoes": 0
        }
    }
}

>
    //실패
    {
        "res": int,                                                                 // 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

* res

    0 : 성공　
    ? : 정리중

### 3. 계정정보 가져오기 [/ada/account/getInfo] / POST <a id="/ada/account/getInfo" href="#/ada/account/getInfo">¶</a>

*info*

    기능 : 유저 계정 정보 가져오기

*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
    
*return value*

    // 성공
    {
        "res": 0,
        "data": {
            "userInfo": {
                "nickName": "cutter2",
                "birthday": "2018-08-16 00:00:00",
                "gender": 2,
                "country": "KO",
                "profileImage": null,
                "age": 0,
                "head": 0,
                "armL": 0,
                "armB": 0,
                "legL": 0,
                "legB": 0,
                "skin": 0,
                "hair": 0,
                "top": 0,
                "bottom": 0,
                "shoes": 0
            },
            "userStat": {
                "level": 4,
                "exp": 762,
                "gold": 514,
                "diamond": 284,
                "ticket": 9,
                "ticketMax": 35,
                "cartCount": 4
            }
        }
    }

    //실패
    {
        "res": int,                                                                 // 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중


## Boutique <a id="Boutique" href="#Boutique">¶</a>

### 1. 카테고리 리스트(브랜드 카테고리 리스트) [/ada/boutique/categoryList] / POST <a id="/ada/boutique/categoryList" href="#/ada/boutique/categoryList">¶</a>

*info*

    기능 : 부티끄 진입 전 카테고리 리스트 를 출력
    수정예정 : 입력 파라미터가 추가 될 수 있음. 출력 결과 값의 개인정보 노출 부분이 변경 될 수 있음. / account 작업이 완료 된 후 return 형식이 변경 될 수 있음.


*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        brand_id : int                  // brand ID

                - 숫자 0 입력 시 아이템 전체의 카테고리 리스트 출력,
                - brand ID 입력 시 (ex. 100004) 해당 브랜드의 카테고리 리스트 출력.

*return value*

	// 성공
	{
        "res": 0,
        "msg": "search success",
        "data": {
            "categoryList": [
                {
                    "categoryID": 1,
                    "categoryName": "Tops",
                    "gender": 0
                },
                {
                    "categoryID": 2,
                    "categoryName": "Pants",
                    "gender": 0
                },
                {
                    "categoryID": 3,
                    "categoryName": "Skirt",
                    "gender": 2
                },
                {
                    "categoryID": 4,
                    "categoryName": "Dress",
                    "gender": 2
                },
                ... 카테고리 반복
            ]
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중

### 2. 브랜드 리스트 [/ada/boutique/brandList] / POST <a id="/ada/boutique/brandList" href="#/ada/boutique/brandList">¶</a>

*info*

    기능 : 부티끄 진입 전 브랜드 리스트 를 출력
    수정예정 : 입력 파라미터가 추가 될 수 있음. 출력 결과 값의 개인정보 노출 부분이 변경 될 수 있음. / account 작업이 완료 된 후 return 형식이 변경 될 수 있음.


*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        gender_id : int                // 성별 ID , 1 : 남성 / 2 : 여성 그 외에 0 : 전체


*return value*

    // 성공
    {
        "res": 0,
        "msg": "search success",
        "data": {
            "brandList": [
                {
                    "brandID": 100004,
                    "brandName": "Balenciaga",
                    "brandImage": "bl_blc_70004_1.png",
                    "brandThumImage": "be_blc_70004_1.png"
                },
                {
                    "brandID": 100009,
                    "brandName": "Calvin Klein",
                    "brandImage": "bl_ck_70009_1.png",
                    "brandThumImage": "be_ck_70009_1.png"
                },
                {
                    "brandID": 100002,
                    "brandName": "Chanel",
                    "brandImage": "bl_cnl_70002_1.png",
                    "brandThumImage": "be_cnl_70002_1.png"
                },
                {
                    "brandID": 100005,
                    "brandName": "Chloe",
                    "brandImage": "bl_cle_70005_1.png",
                    "brandThumImage": "be_cle_70005_1.png"
                },
                ... 브랜드 리스트 반복
            ]
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중

### 3. 브랜드 디테일 [/ada/boutique/brandDetail] / POST <a id="/ada/boutique/brandDetail" href="#/ada/boutique/brandDetail">¶</a>

*info*

    기능 : 부티끄 진입 전 카테고리 리스트 를 출력
    수정예정 : 입력 파라미터가 추가 될 수 있음. 출력 결과 값의 개인정보 노출 부분이 변경 될 수 있음. /  account 작업이 완료 된 후 return 형식이 변경 될 수 있음.


*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        brand_id                        // 브랜드 ID

*return value*

	// 성공
	{
        "res": 0,
        "msg": "search success",
        "data": {
            "brandName": "Balenciaga",
            "description": "Balenciaga is a luxury fashion house founded in Spain by Cristóbal Balenciaga, a Spanish designer born in the Basque Country, Spain. He had a reputation as a couturier of uncompromising standards and was referred to as \"the master of us all\" by Christian Dior. His bubble skirts and odd, feminine, yet ultra-modern shapes were trademarks of the house.</n>The House of Balenciaga is now owned by the French multinational company Kering.",
            "brandURL": "www.balenciaga.com",
            "brandImage": "bl_blc_70004_1.png",
            "brandThumImage": "be_blc_70004_1.png",
            "likeCount": 140,
            "categoryList": [
                {
                    "categoryID": 1,
                    "categoryName": "Tops",
                    "gender": 0
                },
                {
                    "categoryID": 2,
                    "categoryName": "Pants",
                    "gender": 0
                },
                {
                    "categoryID": 3,
                    "categoryName": "Skirt",
                    "gender": 2
                },
                ... 카테고리 리스트 반복
            ]
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중


### 4. 배너 리스트 [/ada/boutique/bannerList] / POST <a id="/ada/boutique/bannerList" href="#/ada/boutique/bannerList">¶</a>

*info*

    기능 : 브랜드 선택 후 브랜드의 상세 페이지 노출
    수정예정 : 입력 파라미터가 추가 될 수 있음. 출력 결과 값의 개인정보 노출 부분이 변경 될 수 있음. / account 작업이 완료 된 후 return 형식이 변경 될 수 있음.


*parameter*

        client_uid : string		// 필수, 디바이스 UID
        os : string				// 필수, OS : enum(ios/android/web)
        access_token : string	// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        place_id : int          // 배너 위치 ID
        brand_id : int          // 브랜드별 구분이 필요한 경우 브랜드 ID, 그 외에는 0(전체)
        gender_id : int         // 성별 정보가 존재 할 경우 , 1 : 남성 / 2 : 여성 그 외에 0 : 전체
        age : int               // 나이 정보가 존재 할 경우 , 그 외에는 0 : 전체


*return value*

       // 성공
        {
            "res": 0,
            "msg": "search success",
            "data": {
                "bannerList": [
                    {
                        "title": "베너제목",
                        "imageURL": "http://35.194.222.5/fit/3_2_banner_sample.png",
                        "contents_id": 1,
                        "linkURL": ""
                    },
                    {
                        "title": "베너제목3",
                        "imageURL": "http://35.194.222.5/fit/a4db65b05f41815608282dfd58b3dcf5.jpg",
                        "contents_id": 3,
                        "linkURL": ""
                    },
                    {
                        "title": "베너제목2",
                        "imageURL": "http://35.194.222.5/fit/3_2_banner_sample.png",
                        "contents_id": 5,
                        "linkURL": "https://sports.news.naver.com/kbaseball/news/read.nhn?oid=477&aid=0000127100"
                    },
                    {
                        "title": "베너제목4",
                        "imageURL": "http://35.194.222.5/fit/518526_2_800x.jpg",
                        "contents_id": 4,
                        "linkURL": ""
                    }
                ]
            }
        }

       //실패
       {
           "res": int,						// 0 이 아닌 값,
           "msg": string.					// 오류 메세지
           "data": null
       }

*res*

       0 : 성공　
       ? : 정리중


### 5. 상품 리스트 [/ada/boutique/productList] / POST <a id="/ada/boutique/productList" href="#/ada/boutique/productList">¶</a>

*info*

    기능 : 부티크 내 상품 리스트를 출력
		   로그인된 경우 토큰값을 전달하면, wish 선택여부, 장바구니 등록여부, 구매여부를 확인할수 있다.
		   
    수정 : account 적용완료(2018.08.17)

    기획서 : ada_gdd_m1_부띠끄 진입 경로와 부띠끄 메뉴 2.1.1 / 2.1.2
	search_type :
		0 : all(전체)
		1: sale(할인) - 할인률(sales_tb.DIS_RATIO)이 등록된 상품만 출력
		2: new(신상) - 발매일(sales_tb.RELEASE_DATE,관리자 수동 변경가능)기준 10일 이내 제품만 출력(등록되지 않은 날은 pass하고 날짜카운트)
		3: category - 카테고리별 출력, 카테고리 리스트의 카테고리 ID(cate_val)를 별도로 전달
		4: brand - 브랜드별 출력
		5: featured - 피쳐드에 등록된 상품리스트(각 상품은 한개이상의 피쳐드에 중복 등록가능)


*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        search_type : int               // all 0, sale 1, new 2, category 3, brand 4, featured 5
        cate_val : int                  // tagType 1, category ID
        subcate_val : int               // tagType 2, sub category ID
        collection_val : int            // tagType 3, collection ID
        brand_val : int                 // tagType 4, brand ID
        style_val : int                 // tagType 5, style ID
        color_val : int                 // tagType 6, color ID
        featured_val : int              // tagType 7, featured ID
        sort_type : int                 // 정렬타입 - 신상품순 0, 낮은가격순 1, 높은가격순 2, 인기순 3 (인기순은 추후 적용)
        append_val : int				// 리스트 호출시 출력 개수
        gender : int					// 조회할 성별(0 전체, 1 남성, 2 여성)
        page_val : int                  // API 최초 호출 시 0 으로 입력 되어야 함. 이 후 1씩 증가

*return value*

	// 성공
	{
        "res": 0,
        "msg": "search success",
        "data": {
            "productCount": 293,				// 상품개수
            "itemList": [
                {
                    "productID": 757,									// 상품 ID
                    "productName": "Item Name 757",						// 상품명
                    "productImage": "http://35.194.222.5/fit/파일명",	// 상품이미지
					"productImageWidth": 600,							// 이미지 width
					"productImageHeight": 800,							// 이미지 height
                    "payMethod": 1,										// 판매재화 타입 : 골드 1, 다이아 2
                    "brandID": 100004,									// 브랜드 ID
                    "brandName": "Balenciaga",							// 브랜드명
                    "price": 130,										// 판매가격
					"saleType":0,				// 세일여부 : 세일 1, 일반 0
					"liked": 1,				// 좋아요 여부 : 체크 1, 미체크 0
					"wished": 0,				// wish 선택여부 : 1 선택, 0 미선택
					"cartType": 0				// 장바구니 상태 : 1 장바구니 포함상품, 2 구매완료 상품, 0 장바구니 및 구매 이력없음
                },
                .. 상품 리스트 반복
            ]
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중

### 6. 상품 상세 보기 [/ada/boutique/productDetail] / POST <a id="/ada/boutique/productDetail" href="#/ada/boutique/productDetail">¶</a>

*info*

    기능 : 입력된 상품 ID 값의 상세 정보를 표기.
		   로그인된 경우 토큰값을 전달하면, wish 선택여부, 장바구니 등록여부, 구매여부를 확인할수 있다.
		   
    수정 : account 적용완료(2018.08.17)

*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        sales_id : int					// 해당 상품의 ID
    

*return value*

	// 성공
	{
        "res": 0,
        "msg": "search success",
        "data": {
            "itemDetail": {
                "saleID": 112,									// 상품ID
                "pdtID": 497,									// 아이템ID
                "pdtName": "Item Name 497",						// 상품명
                "payMethod": 1,									// 판매타입
                "price": 210,									// 가격
                "expPoint": 3,									// 경험치
                "introduction": null,							// 상품설명
                "moreIntroduction": "MOREINFOMATION???",		// 상세설명
                "discountRatio": 0,								// 할인률(0보다 크면 할인상태)
                "buyLink": "http://www.naver.com",				// 상품 온라인 경로
                "brandID": 100002,								// 브랜드 아이디
                "brandName": "Chanel",							// 브랜드 명
                "likeCount": 25268,								// 상품 좋아요합계
                "liked": 0,                                            // 좋아요 여부 1: 체크 0 : 미 체크
				"wished": 0,					// wish 등록여부 : 1 등록, 0 미등록
				"cartType": 0					// 장바구니 상태 : 1 장바구니 등록, 2 구매완료, 0 장바구니 및 구매 이력없음
            },
            "pdtOption": [
                {
                    "subitemID": 6874,								// 상품옵션코드
                    "colorID": 1,									// 색생ID
                    "codeName": "Azure",							// 색상명
                    "codeValue": "#4872ba",							// RGB코드
                    "codeImage": "http://35.194.222.5/fit/wfe_col_21014_ylw_1.png",			// 샘플이미지 경로(준비안됨)
                    "attachList": [
                        {
							"attachType": 0,															// 색상 기본이미지 0, 그외 1
							"attachThum": "http://35.194.222.5/fit/thum/F_PRD_ONE_18PFGZ001_01.png",	// 썸네일 이미지
							"attachPath": "http://35.194.222.5/fit/F_PRD_ONE_18PFGZ001_01.png"			// 상세 이미지
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        }
                    ]
                },
                {
                    "subitemID": 6875,
                    "colorID": 6,
                    "codeName": "Gold",
                    "codeValue": "#988f60",
                    "codeImage": "wfe_col_21005_gld_1.png",
                    "attachList": [
                        {
                            "attachType": 0,
                            "attachPath": "/item/attach/item.gif"
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        }
                    ]
                },
                {
                    "subitemID": 6876,
                    "colorID": 8,
                    "codeName": "Grey",
                    "codeValue": "#8b949c",
                    "codeImage": "wfe_col_21007_gry_1.png",
                    "attachList": [
                        {
                            "attachType": 0,
                            "attachPath": "/item/attach/item.gif"
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        },
                        {
                            "attachType": 1,
                            "attachPath": "/item/attach/item.gif"
                        }
                    ]
                },
                ... 아이템 옵션 리스트 반복
            ],
            "tagList": [
                {
                    "tagType": 1,					// tag 타입
                    "tagValue": 8,					// tag 값
                    "tagName": "Inner wear"			// tag 이름
                },
                {
                    "tagType": 2,
                    "tagValue": 43,
                    "tagName": "Legging"
                },
                {
                    "tagType": 3,
                    "tagValue": 100002,
                    "tagName": "Chanel"
                },
                ... 태그 리스트 반복
            ]
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중

### 7. 피쳐드 리스트 [/ada/boutique/featuredList] / POST <a id="/ada/boutique/featuredList" href="#/ada/boutique/featuredList">¶</a>

*info*

    기능 : 피쳐드 리스트를 출력.
    수정 예정 사항 : cover image value 값이 full path 로 변경 될 수 있음. / account 작업이 완료 된 후 return 형식이 변경 될 수 있음.

*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        gender_id : int                 // gender ID  - gender id 에 따라 featured list 가 변경 됨. (여자:2 / 남자:1)

*return value*

	// 성공
	{
        "res": 0,
        "msg": "search success",
        "data": {
            "featuredList": [
                {
                    "featuredID": 3,
                    "title": "SUN! COLOUR! SHOP SUMMER3",
                    "message": "Bold prints and rich colour inform SS18 extravagance, as we head to the world's most iconic stretch of shoreline, Sydney's Bondi Beach, Here...",
                    "cover_image": "http://35.194.222.5/fit/1_1_featured_sample.png",
                    "tagCount": 0
                },
                {
                    "featuredID": 2,
                    "title": "SUN! COLOUR! SHOP SUMMER2",
                    "message": "Bold prints and rich colour inform SS18 extravagance, as we head to the world's most iconic stretch of shoreline, Sydney's Bondi Beach, Here...",
                    "cover_image": "http://35.194.222.5/fit/1_1_featured_sample.png",
                    "tagCount": 0
                },
                {
                    "featuredID": 1,
                    "title": "SUN! COLOUR! SHOP SUMMER1",
                    "message": "Bold prints and rich colour inform SS18 extravagance, as we head to the world's most iconic stretch of shoreline, Sydney's Bondi Beach, Here...",
                    "cover_image": "http://35.194.222.5/fit/1_1_featured_sample.png",
                    "tagCount": 0
                }
            ]
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중

### 8. 피쳐드 디테일 [/ada/boutique/featuredDetail] / POST <a id="/ada/boutique/featuredDetail" href="#/ada/boutique/featuredDetail">¶</a>

*info*

    기능 : 피쳐드 리스트 API 에서의 결과 값 featured ID 를 파라미터로 사용하여 해당 피쳐드의 상세 내용을 노출
    수정 예정 사항 : account 작업이 완료 된 후 return 형식이 변경 될 수 있음.

*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        featured_id : int               // featured ID

*return value*

	// 성공
	{
        "res": 0,
        "msg": "search success",
        "data": {
            "header": "Shop Summer1",
            "contents": "<meta http-equiv=\"content-type\" content=\"text/html;charset=utf-8\" />\n<div class=\"header_container-7\">\n<div class=\"header\">\n<img class=\"hidden-xl hidden-md visible-sm visible-xs\" src=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/00-xsm.jpg\" /></div></div>\n<div class=\"baseline col12 alpha omega\">\n<div class=\"article-content \">\n<div class=\"baseline col12 alpha omega\">\n<div class=\"container\">\n<div class=\"baseline col2 col-md-1 col-sm-1 col-xs-12\">&nbsp;</div>\n<div class=\"baseline col8 col-md-10 col-sm-10 col-xs-12 alpha omega\">\n<article> <div class=\"baseline col12 alpha omega\">\n<div class=\"article-container cards\">\n<link href=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/css/listing2.min.css\" rel=\"stylesheet\" type=\"text/css\" />\n<link href=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/css/templates-2.1.min.css\" rel=\"stylesheet\" type=\"text/css\" />\n<link href=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/css/dynamicProducts-3.0.1.min.css\" rel=\"stylesheet\" type=\"text/css\" />\n<script src=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/css/dynamicProducts-3.0.1.min.js\"></script>\n<div id=\"item-template\"><article class=\"baseline col3 col-md-4 col-sm-4 col-xs-6 alpha omega listing-item mouse-over-element ab-available-sizes-product-card\"><div class=\"listing-item-image\"><a class=\"listing-item-link no-underline\" data-ffref=\"{{ffref}}\" href=\"%7b%7bProductUrl%7d%7d.html\" target=\"_self\"><img alt=\"{{Description2}}\" class=\"imageRollover lazy-img lazy-loaded js-rollover absolute crossfade\" src=\"http://:0/\" /><img alt=\"{{Description3}}\" class=\"absolute rolloverActive\" src=\"http://:1/\" /><noscript>&lt;img src=\"//:2\"&gt;</noscript></a></div><a class=\"listing-item-content listing-item-link no-underline\" data-ffref=\"{{ffref2}}\" href=\"%7b%7bProductUrl2%7d%7d.html\"><h5 class=\"listing-item-content-brand\">{{DesignerName}}</h5><p class=\"listing-item-content-description\">{{Description}}</p><span class=\"listing-item-content-price\">{{PriceDisplay}}</span></a></article></div>\n<div class=\"tmpl_intro-1\"><div class=\"title\"><h1 class=\"bold center\"><span id=\"tmpl-head\">Cover story!<br />What’s modest now?</span></h1></div><div class=\"text book center\"><span id=\"tmpl-sell\">SS18 reveals new ways to conceal. From kaftan silhouettes to the ultra-slick trench, here are 6 ways to cover up this season…</span></div><div class=\"credit center\"><span id=\"tmpl-byline\"><strong>PHOTOGRAPHY</strong> SEAN AND SENG AT MANAGEMENT + ARTISTS.<strong> STYLING</strong> MARK VASSALLO. <strong>MODEL</strong> NORA ATTAL AT VIVA LONDON. <strong>HAIR</strong>PAOLO SOFFIATTI AT BRYANT ARTISTS. <strong>MAKEUP</strong> GEORGINA GRAHAM AT MANAGEMENT + ARTISTS.</span>\n<h1></h1>\n<br>\n<img class=\"hidden-xl hidden-md visible-sm visible-xs\" src=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/1-lg.jpg\" /></div></div>\n<div class=\"tmpl_intro-1\"><div class=\"title\"><h1 class=\"bold center\"><span id=\"tmpl-head\">City-boy tailoring</span></h1></div><div class=\"text book center\"><span id=\"tmpl-sell\">Wall Street cover up: Demna raids Gordon Gekko’s wardrobe, trades brogues for pink mules.</span>\n</div>"
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중

### 9. 컨텐츠 뷰어 [/ada/boutique/contentsView] / POST <a id="/ada/boutique/contentsView" href="#/ada/boutique/contentsView">¶</a>

*info*

    기능 : 컨텐츠 내용이 노출
    수정 예정 사항 : account 작업이 완료 된 후 return 형식이 변경 될 수 있음.

*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        content_id : int               // 컨텐츠 ID

*return value*

	// 성공
	{
        "res": 0,
        "data": {
            "contents": "<meta http-equiv=\"content-type\" content=\"text/html;charset=utf-8\" />\n<div class=\"header_container-7\">\n<div class=\"header\">\n<img class=\"hidden-xl hidden-md visible-sm visible-xs\" src=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/00-xsm.jpg\" /></div></div>\n<div class=\"baseline col12 alpha omega\">\n<div class=\"article-content \">\n<div class=\"baseline col12 alpha omega\">\n<div class=\"container\">\n<div class=\"baseline col2 col-md-1 col-sm-1 col-xs-12\">&nbsp;</div>\n<div class=\"baseline col8 col-md-10 col-sm-10 col-xs-12 alpha omega\">\n<article> <div class=\"baseline col12 alpha omega\">\n<div class=\"article-container cards\">\n<link href=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/css/listing2.min.css\" rel=\"stylesheet\" type=\"text/css\" />\n<link href=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/css/templates-2.1.min.css\" rel=\"stylesheet\" type=\"text/css\" />\n<link href=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/css/dynamicProducts-3.0.1.min.css\" rel=\"stylesheet\" type=\"text/css\" />\n<script src=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/css/dynamicProducts-3.0.1.min.js\"></script>\n<div id=\"item-template\"><article class=\"baseline col3 col-md-4 col-sm-4 col-xs-6 alpha omega listing-item mouse-over-element ab-available-sizes-product-card\"><div class=\"listing-item-image\"><a class=\"listing-item-link no-underline\" data-ffref=\"{{ffref}}\" href=\"%7b%7bProductUrl%7d%7d.html\" target=\"_self\"><img alt=\"{{Description2}}\" class=\"imageRollover lazy-img lazy-loaded js-rollover absolute crossfade\" src=\"http://:0/\" /><img alt=\"{{Description3}}\" class=\"absolute rolloverActive\" src=\"http://:1/\" /><noscript>&lt;img src=\"//:2\"&gt;</noscript></a></div><a class=\"listing-item-content listing-item-link no-underline\" data-ffref=\"{{ffref2}}\" href=\"%7b%7bProductUrl2%7d%7d.html\"><h5 class=\"listing-item-content-brand\">{{DesignerName}}</h5><p class=\"listing-item-content-description\">{{Description}}</p><span class=\"listing-item-content-price\">{{PriceDisplay}}</span></a></article></div>\n<div class=\"tmpl_intro-1\"><div class=\"title\"><h1 class=\"bold center\"><span id=\"tmpl-head\">Cover story!<br />What’s modest now?</span></h1></div><div class=\"text book center\"><span id=\"tmpl-sell\">SS18 reveals new ways to conceal. From kaftan silhouettes to the ultra-slick trench, here are 6 ways to cover up this season…</span></div><div class=\"credit center\"><span id=\"tmpl-byline\"><strong>PHOTOGRAPHY</strong> SEAN AND SENG AT MANAGEMENT + ARTISTS.<strong> STYLING</strong> MARK VASSALLO. <strong>MODEL</strong> NORA ATTAL AT VIVA LONDON. <strong>HAIR</strong>PAOLO SOFFIATTI AT BRYANT ARTISTS. <strong>MAKEUP</strong> GEORGINA GRAHAM AT MANAGEMENT + ARTISTS.</span>\n<h1></h1>\n<br>\n<img class=\"hidden-xl hidden-md visible-sm visible-xs\" src=\"https://s3.ap-northeast-2.amazonaws.com/library.dramabible/ADA_WEB/1-lg.jpg\" /></div></div>\n<div class=\"tmpl_intro-1\"><div class=\"title\"><h1 class=\"bold center\"><span id=\"tmpl-head\">City-boy tailoring</span></h1></div><div class=\"text book center\"><span id=\"tmpl-sell\">Wall Street cover up: Demna raids Gordon Gekko’s wardrobe, trades brogues for pink mules.</span>\n</div>"
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중



### 10. 태그 리스트 [/ada/boutique/tagList] / POST <a id="/ada/boutique/tagList" href="#/ada/boutique/tagList">¶</a>

*info*

    기능 : 컨텐츠 내용이 노출
    수정 예정 사항 : account 작업이 완료 된 후 return 형식이 변경 될 수 있음.
	태그타입 : category 1,subcategory 2,collection 3,brand 4,style 5,color 6,featured 7,mat 8,patten 9,parts 10

*parameter*

            client_uid : string				// 미적용 - 디바이스 UID
    		os : string						// 미적용 - OS : enum(ios/android/web)
    		access_token : string			// 미적용 - access token
        - 이하 contents var -
        search_type : int               // all 0, sale 1, new 2, category 3, brand 4, featured 5
        cate_val : int                  // tagType 1, category ID
        subcate_val : int               // tagType 2, sub category ID
        collection_val : int            // tagType 3, collection ID
        brand_val : int                 // tagType 4, brand ID
        style_val : int                 // tagType 5, style ID
        color_val : int                 // tagType 6, color ID
        featured_val : int              // tagType 7, featured ID
            

*return value*

	// 성공
	{
        "res": 0,
        "msg": "search success",
        "data": {
            "tagList": [
                {
                    "tagType": 0,				// 태그타입 
                    "tagValue": 1,				// 태그값
                    "tagName": "SALES",			// 태그문자
                    "check": 1					// 태그 선택여부 checked 1, no check 0
                },
                {
                    "tagType": 1,
                    "tagValue": 1,
                    "tagName": "Tops",
                    "check": 1
                },
                {
                    "tagType": 2,
                    "tagValue": 13,
                    "tagName": "T-shirt",
                    "check": 1
                },
                ... 태그 리스트 반복
            ]
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중



## Wish <a id="Wish" href="#Wish">¶</a>

### 1. 위시 등록/삭제 [/ada/wish/edit] / POST <a id="/ada/wish/edit" href="#/ada/wish/edit">¶</a>

*info*

    기능 : 상품의 wish를  등록 또는 삭제한다.
    
*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 선택 - 토큰이 없을경우 null, access token
        - 이하 contents var -
        sale_id : int                   // product id
        wish_type : int               // wish 등록 상태 (0 : 삭제 , 1 : 등록)

*return value*

    // 성공
     {
        "res": 0,
        "msg": "wish is edit",
        "data": {
            "wishInfo": {
                "isWish": 0			// wish 처리상태 : 1 wish등록, 0 wish해제
                "saleID": "27"                                     // wish 등록 처리 한 상품 ID 값 
            }
        }
    }

    //실패
    {
        "res": int,					// 0 이 아닌 값,
        "msg": string.              // 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중


### 2. 위시 리스트 보기 [/ada/wish/list] / POST <a id="/ada/wish/list" href="#/ada/wish/list">¶</a>

*info*

    기능 : wish 리스트 보기 - 상품 리스트, 상세 api에 있고우선 리스트기능은 보류한다. 
    수정 예정 사항 :  account 작업이 완료 된 후 return 형식이 변경 될 수 있음.


*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 필수, access token
        - 이하 contents var -

*return value*

    // 성공
    {
    "res": 0,
    "msg": "search success",
    "data": {
        "wishList": [
            {
                "saleID": 26,
                "productImage": "http://35.194.222.5/fit/P239BR-1RCO-F0G22-S-181_SLF_UC526059_g.png",
                "productImageWidth": 495,
                "productImageHeight": 660,
                "productName": "Embroidered Shorts",
                "brandName": "Prada",
                "price": 1420,
                "payMehtod": 1,
                "regDateTime": "2018-08-20 15:54:52",
                "saleType": 2
            },
            {
                "saleID": 27,
                "productImage": "http://35.194.222.5/fit/P241B-P33-F0002-S-181_SLF_UC508049_g.png",
                "productImageWidth": 495,
                "productImageHeight": 660,
                "productName": "Pocket Detail Shorts",
                "brandName": "Prada",
                "price": 1460,
                "payMehtod": 1,
                "regDateTime": "2018-08-21 11:51:10",
                "saleType": 2
            }
        ]
    }
}

    // 위시리스트에 아무 상품도 등록 되어 있지 않을 때
    {
        "res": 0,
        "msg": "list is empty",
        "data": {}
    }

    // 실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중


### 3. like 등록/삭제 [/ada/like/edit] / POST <a id="/ada/like/edit" href="#/ada/like/edit">¶</a>

*info*

    기능 : 상품의 like를  등록 또는 삭제하고 카운트를 받는다..
    
*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 필수, access token
        - 이하 contents var -
        sale_id : int                   // product id
        like_type : int                // 1 체크, 0 해제, 그밖에는 중복체크


*return value*

    // 성공
     {
        "res": 0,
        "msg": "sucess",            // 성공일때 sucess, 실패일때 fail
        "data": {
            "likeInfo": {
                "isLike": 0,		// like 처리상태 : 1 like등록, 2 like해제
                "saleID": "62",     // like 처리된 상품ID
                "count": 4          // like 총개수
            }
        }
    }

    //실패
    {
        "res": int,					// 0 이 아닌 값,
        "msg": string.              // 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중


## Cart <a id="Cart" href="#Cart">¶</a>

### 1. 장바구니 등록/구매처리 [/ada/cart/edit] / POST <a id="/ada/cart/edit" href="#/ada/cart/edit">¶</a>

*info*

    기능 : 상품을 장바구니에 저장하거나 삭제하고, 장바구니의 최종 구매처리를 한다.
    확인 사항 : 구매, 저장, 삭제 처리가 완료 된 상품을 동일한 actionType 으로 호출 시 에러가 발생(중복처리 안됨)
    
*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 필수  access token
        - 이하 contents var -
        sale_id : int                     // product id
        subitem_id : int                // 아이템 옵션 값 (productDetail API 에서 확인가능)
        action_type : int              // 1 장바구니 담기, 2 바로구매

*return value*

    // 성공
    {
        "res": 0,
        "msg": "정상적으로 처리되었습니다. : 0",
        "data": {
            "userStat": {
                "cartCount": 2
            },
            "itemInfo": {
                "productCount": 1,                     // 구매처리한 상품의 개수
                "productList": [
                    {
                        "saleID": "46",
                        "subItemID": "46"
                    }
                ]
            }
        }
    }

    // 이미 구매 처리 된 상태
    {
        "res": -1,
        "msg": "이미 구매한 상품입니다.:-210301",
        "data": {
            "userStat": {
                "cartCount": 0
            },
            "itemInfo": {
                "productList": [
                    {
                        "saleID": "46",
                        "subItemID": "46"
                    }
                ]
            }
        }
    }
    
    // 상품 파라미터 입력이 잘못 되었을 때
    {
        "res": -1,
        "msg": "상품정보가 없습니다.:-210001",
        "data": {
            "userStat": {
                "cartCount": 0
            },
            "itemInfo": {
                "productList": [
                    {
                        "saleID": null,
                        "subItemID": null
                    }
                ]
            }
        }
    }

    //실패
    {
        "res": -1,
        "msg": "fail:-210302",
        "data": {}
    }

*res*

    0 : 성공　
    ? : 정리중

### 2. 장바구니/구매 리스트 [/ada/cart/list] / POST <a id="/ada/cart/list" href="#/ada/cart/list">¶</a>

*info*

    기능 : 장바구니에 등록 되어 있는 상품 리스트를 출력
    
*parameter*

    client_uid : string				// 필수, 디바이스 UID
    os : string				// 필수, OS : enum(ios/android/web)
    access_token : string			// 필수  access token
    - 이하 contents var -
    
    search_type : Number                       // 검색 타입 (장바구니 목록 : 1, 구매 목록 : 2)
    page_val : Number                           // 페이지 번호 (default: 0)
    append_val : Number                        // 페이지 내 아이템 출력 갯수 (default : 10)

*return value*

    // 성공
     {
    "res": 0,
    "msg": "search success",
    "data": {
        "cartList": [
            {
                "cartID": 15,
                "saleID": 4,
                "itemID": 4,
                "saleName": "Overlay Dress",
                "priceType": 1,
                "price": 1220,
                "brandName": "Miu Miu",
                "itemImage": "MF2887-1RA8-F0HZR_SLF_UC535871_g.png",
                "colorID": 4,
                "colorName": "Blue",
                "codeValue": "#214377"
            },
            {
                "cartID": 16,
                "saleID": 8,
                "itemID": 8,
                "saleName": "Brocade Dress",
                "priceType": 1,
                "price": 1220,
                "brandName": "Prada",
                "itemImage": "P36N4-1QK3-F0136-S-181_SLF_UC569017_g.png",
                "colorID": 4,
                "colorName": "Blue",
                "codeValue": "#214377"
            },
            {
                "cartID": 20,
                "saleID": 33,
                "itemID": 33,
                "saleName": "Overprinted Skirt",
                "priceType": 1,
                "price": 690,
                "brandName": "Prada",
                "itemImage": "GFD108-1RGD-F0970-S-181_SLF_UC519273_g.png",
                "colorID": 12,
                "colorName": "Red",
                "codeValue": "#a40000"
            }
        ]
    }
}

    //실패
    {
		"res": -1,
		"msg": "fail:-210302",
		"data": {}
	}

*res*

    0 : 성공　
    ? : 정리중

### 3. 장바구니 삭제처리 [/ada/cart/delete] / POST <a id="/ada/cart/delete" href="#/ada/cart/delete">¶</a>

*info*

    기능 : 상품을 장바구니에서 삭제.
    
    
*parameter*

        client_uid : string                     // 필수, 디바이스 UID
        os : string                             // 필수, OS : enum(ios/android/web)
        access_token : string                   // 필수  access token
        - 이하 contents var -
        cart_id : int                           // cart ID, 장바구니 ID

*return value*

    // 성공
    {
        "res": 0,
        "msg": "정상적으로 처리되었습니다. : 0",
        "data": {
            "userStat": {
                "cartCount": 24                     //  잔여 장바구니 담긴 상품 갯수
            },
            "itemInfo": {
                "productCount": 1,                  // 삭제 처리 개수
                "productList": [
                    {
                        "dropID": 31,               // 삭제된 장바구니 ID
                        "saleID": 1,                // 삭제된 상품 ID
                        "subItemID": 1              // 삭제된 sub Item ID
                    }
                ]
            }
        }
    }


    // 삭제 할 장바구니 아이템이 없을 경우
    {
        "res": -1,
        "msg": "장바구니에 해당 상품이 존재하디 않습니다.:-210302",
        "data": {
            "userStat": {
                "cartCount": 24                     // 잔여 장바구니 담긴 상품 갯수
            },
            "itemInfo": {
                "productCount": 0,                  // 삭제 처리 개수 = 0
                "productList": [
                    {}                              // 삭제된 상품정보
                ]
            }
        }
    }


    //실패
    {
        "res": -1,
        "msg": "fail:-210302",
        "data": {}
    }

*res*

    0 : 성공　
    ? : 정리중

### 4. 장바구니 전체구매 [/ada/cart/buyAll] / POST <a id="/ada/cart/buyAll" href="#/ada/cart/buyAll">¶</a>

*info*

    기능 : 장바구니 내의 모든 상품을 구매.
    
*parameter*

        client_uid : string				// 필수, 디바이스 UID
        os : string						// 필수, OS : enum(ios/android/web)
        access_token : string			// 필수  access token
        - 이하 contents var -
        cart_ids : string                                   // cart ID

*return value*

    // 성공
    {
        "res": 0,
        "msg": "정상적으로 처리되었습니다. : 0",
        "data": {
            "userStat": {
                "cartCount": 0                 // 장바구니의 남아 있는 상품의 갯
            },
            "itemInfo": {
                "productCount": 2,                     // 구매처리한 상품의 개수
                "productList": [
                    {
                        "cartID": 105,          
                        "saleID": 45,           
                        "subItemID": 45
                    },
                    {
                        "cartID": 106,
                        "saleID": 46,
                        "subItemID": 46
                    }
                ]
            }
        }
    }

    // 장바구니에 상품이 없을 경우
    {
        "res": -1,
        "msg": "장바구니에 상품이 없습니다.:-210305",
        "data": {
            "userStat": {
                "cartCount": 0
            },
            "itemInfo": {
                "productCount": 0,
                "productList": []
            }
        }
    }

    //실패
    {
        "res": -1,
        "msg": "fail:-210302",
        "data": {}
    }

*res*

    0 : 성공　
    ? : 정리중

## Card <a id="Card" href="#Card">¶</a>

### 1. 프레임 리스트 [/ada/card/frameList] / POST <a id="/ada/card/frameList" href="#/ada/card/frameList">¶</a>

*info*

    기능 : ADA 컨텐츠 카드 시스템의 프레임 리스트를 노출
    수정예정 : 입력 파라미터가 추가 될 수 있음. 

*parameter*

    client_uid : string                     // 필수, 디바이스 UID
    os : string                             // 필수, OS : enum(ios/android/web)
    access_token : string                   // 필수  access token
    
*return value*

    // 성공
    {
    "res": 0,
    "msg": "search success",
    "data": {
        "frameList": [
            {
                "frameID": int,             // 카드프레임 ID
                "frameName": string,        // 카드프레임 이름
                "component1": {             // 컴포넌트 1 
                    "type": int,            // 컴포넌트 1의 타입 값 - 미사용 : 0  / 텍스트메세지 : 1 / 프로필타입-1 : 2
                    "align": int,           // 정렬 - 미사용 : 0 / 좌측정렬 : 1 / 우측정렬 : 2 / 가운데정렬 : 3
                    "color": string,        // 컬러 (RGB code)
                    "size": int             // 크기(pt)
                },
                "component2": {             // 컴포넌트 2
                    "type": int             // 컴포넌트 2의 타입 값 - 미사용 : 0 / 배지출력 (미션시간 : 1 / ARKE 코인 정보 : 2 / HOT : 4 / NEW : 5 / SALE : 6)
                },
                "component3": {             // 컴포넌트 3
                    "type": int,            // 컴포넌트 3의 타입 값 - 미사용 : 0 / 이미지 : 1 / 동영상 : 2 / 상품 : 3 / 갤러리 : 4 / 유저 : 5
                    "w": int,               // 이미지나 동영상일때, 프레임 이미지 넚이 비율값
                    "h": int,               // 이미지나 동영상일때, 프레임 이미지 높이 비율값
                    "s": int                // 이미지일때, 미사용 0/높이에 맞추기 1/넓이에 맞추기 2/프레임에 맞추기 3
                },
                "component4": {             // 컴포넌트 4
                    "type": int             // 컴포넌트 4의 타입 값 - 미사용 : 0/ 프로필타입-2 : 1
                },
                "component5": {             // 컴포넌트 5
                    "type": int,            // 컴포넌트 5의 타입 값 - 미사용 : 0 /1라인텍스트 : 1/ 2라인텍스트 : 2
                    "align1": int,          // 정렬-미사용 : 0 / 좌측정렬 : 1/우측정렬 : 2 / 가운데정렬 : 3
                    "color1": string,       // 컬러 (RGB code)
                    "size1": int,           // 크기(pt)
                    "align2": int,          // 정렬-미사용 : 0 / 좌측정렬 : 1/우측정렬 : 2 / 가운데정렬 : 3
                    "color2": string,       // 컬러 (RGB code)
                    "size2": int            // 크기(pt)
                },
                "component6": {             // 컴포넌트 6
                    "type": int,            // 컴포넌트 6의 타입 값 - 미사용 : 0 / one 커스텀 : 1 / multi 커스텀 : 2
                    "align": int            // 정렬-미사용 : 0 / 좌측정렬 : 1/우측정렬 : 2 / 가운데정렬 : 3
                },
                "description": string       // 프레임 설명
            }
            ... (반복) 
        ]
    }
}

    //실패
    {
        "res": int,                                                                 // 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중



### 2. 카드 리스트 [/ada/card/cardList] / POST <a id="/ada/card/cardList" href="#/ada/card/cardList">¶</a>

*info*

    기능 : ADA 컨텐츠 카드 시스템의 카드 리스트를 노출
    수정예정 : 입력 파라미터가 추가 될 수 있음. 

*parameter*

    client_uid : string                     // 필수, 디바이스 UID
    os : string                             // 필수, OS : enum(ios/android/web)
    access_token : string                   // 필수  access token
    - 이하 content var -
    type1 : int             // 컴포넌트 1 의 타입 값 - 미 사용 : 0 / 텍스트 메세지 : 1 / 프로필타입1 : 2
    type2 : int             // 컴포넌트 2 의 타입 값 - 미 사용 : 0 / 코인 정보 : 1 / 뱃지 출력 (hot : 3 / new : 4 / sale : 5)
    type3 : int             // 컴포넌트 3 의 타입 값 - 미 사용 : 0 / 이미지 : 1 / 동영상 : 2 / 상품 : 3 / 갤러리 : 4 / 유저 : 5 
    type4 : int             // 컴포넌트 4의 타입 값 - 미 사용 : 0 / 사용 : 1
    type5 : int             // 컴포넌트 5의 타입 값 - 미 사용 : 0 / 1라인텍스트 : 1 / 2라인텍스트 : 2
    type6 : int             // 컴포넌트 6의 타입 값 - 미 사용 : 0 / 싱글커스텀 : 1 / 멀티커스텀 : 2
    c_type : int            // 컴포넌트 6의 커스텀 타입 값 (미사용 0/인게임 이동 1/인웹페이지 이동 2/외부 페이지 이동 3/별점 4/커스텀버튼 5)


*return value*

    // 성공
    "res": 0,
    "msg": "search success",
    "data": {
        "cardList": [
                "card": {
                "id": int,
                "name": string,
                "frameID": int
            },
            "category": {
                "id": string,
                "name": int
            },
            "component1": [
                    ("type" = 1)
                {
                    "type": int,                    // 컴포넌트 1 의 타입 값 - 미 사용 : 0 / 텍스트 메세지 : 1 / 프로필타입1 : 2
                    "txtOBJ": {
                        "txt": string,              // 텍스트
                        "align": int,               // 정렬 - 미사용 : 0 / 좌측정렬 : 1 / 우측정렬 : 2 / 가운데정렬 : 3
                        "color": string,            // RGB코드
                        "size": int                 // 크기(pt)
                    }
                }
                    ("type" = 2)
                {
                    "profileOBJ": {
                        "profileImage": string,     // 이미지 URL
                        "nickName": string,         // 닉네임
                        "accountID": string         // 아이디
                    }
                }
            ],                              
            "component2": [
                    ("type" = 1)
                {
                    "type": int,                    // 컴포넌트 2 의 타입 값 - 미 사용 : 0 / 코인 정보 : 1 / 뱃지 출력 (hot : 3 / new : 4 / sale : 5)
                    "timeOBJ": {                    // 시간 정보
                        "time": string              // 시간 
                    }
                }
                    ("type" = 2)
                {
                    "coinOBJ": {                    // 코인정보
                        "coinImg": string,          // 코인 이미지 URL
                        "coinCnt": string           // 코인 값 string
                    }
                }
                    ("type" = 3)
                {
                    "badgeOBJ": {                   // 뱃지 정보
                        "hot": int                  // 미사용 : 0 사용 : 1
                    }
                }
                    ("type" = 4)
                {
                    "badgeOBJ": {
                        "new": int
                    }
                }
                    ("type" = 5)
                {
                    "badgeOBJ": {
                        "sale": int
                    }
                }
            ],                                
            "component3": {
                    ("type" = 1)
                "type": int,                         // 컴포넌트 3 의 타입 값 - 미 사용 : 0 / 이미지 : 1 / 동영상 : 2 / 상품 : 3 / 갤러리 : 4 / 유저 : 5 
                "imageOBJ": {                        // 이미지 정보
                    "imageURL": string,              // 이미지 경로
                    "imageLink": string,             // 이미지 링크
                    "width": int,                    // 이미지 넓이 비율 값
                    "height": int,                   // 이미지 높이 비율 값
                    "set": int                       // 이미지 일 때, 미사용 : 0/ 높이에 맞추기 : 1 / 넓이에 맞추기 : 2 / 프레임에 맞추기 : 3
                }
                    ("type" = 2)
                "videoOBJ": {                        // 비디오 정보
                    "videoURL": string,              // 비디오 경로
                    "width": int,                    // 동영상 넓이 비율 값
                    "height": int,                   // 동영상 높이 비율 값
                    "set": int                       // 미사용 : 0 / 높이에 맞추기 : 1 / 넓이에 맞추기 : 2 / 프레임에 맞추기 : 3
                }
                    ("type" = 3)
                "productList": [                    
                    {
                        "itemURL": string,              // 아이템 이미지 경로
                        "itemLink": string,             // 아이템 링크
                        "itemName": string,             // 상품 명
                        "itemID": string                // 상품 가격
                    },
                    ... 반복
                ]
                    ("type" = 4)
                "contentList": [
                    {
                        "imageURL": string,             // 컨텐츠 이미지 경로
                        "imageLink": string,            // 컨텐츠 링크
                        "title": string,                // 제목
                        "nickName": string              // 닉네임
                    },
                    ... 반복
                ]
                    ("type" = 5)
                "recommendList": [
                    {
                        "profileURL": string         // 컨텐츠 이미지 경로
                        "userID": int,               // 유저 아이디
                        "nickName": string,          // 유저 닉네임
                        "follow": int                // 팔로워 상태 (follow : 1 / etc : 0)
                    },
                    ... 반복
                ]

            },        
        "component4": {
                "type": int,                         // 컴포넌트 4의 타입 값 - 미 사용 : 0 / 사용 : 1
                "profileOBJ": {
                    "profileImage": string,          // 이미지 URL
                    "nickName": string,              // 닉네임
                    "accountID": int                 // 아이디
                }
            },                               
        "component5": {                               // 컴포넌트 5의 타입 값 - 미 사용 : 0 / 1라인텍스트 : 1 / 2라인텍스트 : 2
                    ("type" =1)
                "type": int,
                "txtOBJ1": {
                    "txt": string,                       // 텍스트
                    "size": int,                         // 폰트크기(pt)
                    "color": string                      // RGB코드
                }
                    ("type" = 2)
                "txtOBJ1": {
                    "txt": string,
                    "size": int,
                    "color": string
                },
                "txtOBJ2": {
                    "txt": string,                     // 텍스트
                    "size": int,                       // 폰트크기(pt)
                    "color": string                    // RGB코드
                }
            }
        "component6": {
                    ("type" = 1)
                "type": int,                        // 컴포넌트 6의 타입 값 - 미 사용 : 0 / 싱글커스텀 : 1 / 멀티커스텀 : 2
                "align": int,                        // 정렬-미사용 : 0 / 좌측정렬 : 1 / 우측정렬 : 2 / 가운데정렬 : 3
                "customType": int,                  // 미사용 : 0 / 인게임 이동 : 1 / 인웹페이지 이동 : 2 / 외부 페이지 이동 : 3 / 별점 : 4 / 커스텀버튼 : 5
                ("customType" = 1,2,3)
                "moreOBJ": {
                    "moreLink": string                  // more Detail 링크
                }
            }
            "component6": {
                    ("customType" = 4)
                "starOBJ": {
                    "starSelect": int                // 선택한 별점 (0 ~ 5)
                }
            }
            "component6": {
                    ("customType" = 5)
                "buttonOBJ": {
                    "buttonList": [
                        {
                            "Label": string,         // 버튼 라벨
                            "Count": int,            // 버튼 갯수
                            "Selected": int          // 선택한 버튼 (선택 : 1 / else : 0)
                        }
                    ]
                    ... 반복
                }
            }
            "component6": {
                    ("type" = 2)
                "type": int,
                "align": int,
                    ("customType" == 1,2,3)
                "customType": int,
                "moreOBJ": {
                    "moreLink": string
                },
                    ("customType" == 4)
                "starOBJ": {
                    "starSelect": int
                },
                    ("customType" == 5)
                "buttonOBJ": {
                    "buttonList": [
                        {
                            "label": string,
                            "count": int,
                            "selected": int
                        }
                    ]
                    ... 반복
                },
                "commonOBJ": {                      // 아래 항목중 노출 표시가 안된것은 노출하지 않는다.
                    "liked": int,                    // 좋아요 선택 : 1, 미 선택 : 0 
                    "likedCount": int,               // 좋아요 갯수 
                    "threadCount": int,              // thread 갯수 
                    "commentCount": int,             // 댓글 갯수 
                    "shared": int                    // 공유버튼 노출 1, 미 노출 : 0
                }
            }
        ]
        ... 반복
    }

    //실패
    {
        "res": int,                                                                 // 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중


### 3. 좋아요 처리 [/ada/card/like] / POST <a id="/ada/card/like" href="#/ada/card/like">¶</a>

*info*

    기능 : ADA 컨텐츠 카드 시스템의 프레임 리스트를 노출
    수정예정 : 입력 파라미터가 추가 될 수 있음. 
    
*parameter*

    client_uid : string                     // 필수, 디바이스 UID
    os : string                             // 필수, OS : enum(ios/android/web)
    access_token : string                   // 필수  access token
    - 이하 contents var - 
    card_id : int                           // CARD ID
    liked : int                             // 좋아요 선택 : 1 / 미 선택 : 0

    
*return value*

    // 성공
    {
        "res": 0,
        "msg": "search success",
        "data": {
            "cardID": int,                  // CARD ID
            "liked": int,                   // 좋아요 선택 여부
            "likeCount": int                // 좋아요 카운트
        }
    }

    //실패
    {
        "res": int,                                                                 // 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중


### 4. 별점 처리 [/ada/card/star] / POST <a id="/ada/card/star" href="#/ada/card/star">¶</a>

*info*

    기능 : ADA 컨텐츠 카드 시스템의 프레임 리스트를 노출
    수정예정 : 입력 파라미터가 추가 될 수 있음. 
    
*parameter*

    client_uid : string                     // 필수, 디바이스 UID
    os : string                             // 필수, OS : enum(ios/android/web)
    access_token : string                   // 필수  access token
    - 이하 contents var - 
    card_id : int                           // CARD ID
    star_val : int                          // STAR n (0 ~ n)

    
*return value*

    // 성공
    {
        "res": 0,
        "msg": "search success",
        "data": {
            "cardID": int,                  // CARD ID
            "starVal": int                  // 입력한 별점
        }
    }

    //실패
    {
        "res": int,                                                                 // 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중

### 5. 버튼 처리 [/ada/card/button] / POST <a id="/ada/card/button" href="#/ada/card/button">¶</a>

*info*

    기능 : ADA 컨텐츠 카드 시스템의 프레임 리스트를 노출
    수정예정 : 입력 파라미터가 추가 될 수 있음. 
    
*parameter*

    client_uid : string                     // 필수, 디바이스 UID
    os : string                             // 필수, OS : enum(ios/android/web)
    access_token : string                   // 필수  access token
    - 이하 contents var - 
    card_id : int                           // CARD ID 
    button_val : int                        // 선택한 버튼 값 (0 ~ n)

    
*return value*

    // 성공
    {
    "res": 0,
    "msg": "search success",
    "data": {
        "cardID": int,                      // CARD ID
        "buttonVal": int,                   // 선택한 버튼 값
        "buttonList": [
            {
                "label": string,              // 버튼 라벨
                "count": int,                 // 버튼 갯수
                "selected": int               // 선택한 버튼 (선택 : 1 / else : 0)
            },
            ... 반복
        ]
    }
}

    //실패
    {
        "res": int,                                                                 // 0 이 아닌 값,
        "msg": string.					// 오류 메세지
        "data": null
    }

*res*

    0 : 성공　
    ? : 정리중

## Etc <a id="etc" href="#etc">¶</a>


### 1. 통신테스트 [/ada/etc/test] / POST <a id="/ada/etc/test" href="#/ada/etc/test">¶</a>

*info*

    기능 : api 서버의 통신상태를 확인한다.
	설명 : os, clientUID 값만 수신하고, 접속된 ip정보를 리턴한다.

*parameter*

        client_uid: string			// 디바이스 UID
        os : string				// OS : enum(ios/android/web)


*return value*

    // 성공
    {
        "res": 0,
        "msg": "search success",
        "data": {
            "IMG_SERVER_URL": "http://35.194.222.5",
            "CLIENT_IP": "106.250.172.162",
            "API_PATH": "/ada/etc/test"
        }
    }

    //실패
    {
        "res": int,						// 0 이 아닌 값,
		"msg": string.					// 오류 메세지
		"data": null
    }

*res*

    0 : 성공　
    ? : 정리중
