Admin Page API
==============

API data movement in admin - gacha page
---------------------------------------
***
###### TOC
* [update](#ALL-Character-Table-Update)
* [add](#Gacha-Pool-Add)
* [edit](#Gacha-pool-edit)
* [delete](#Gacha-Pool-Delete)



***

### ALL Character Table Update

- ##### Page : /gacha/admin
- ##### Method : post
- ##### Payload :
> ###### order : order types [ <span style="color:red"> string </span> ] ( "update" )
> ###### characters : characters json file [ <span style="color:red">json object </span> ]
- ##### etc :
서버 저장소의 ```character_table.json``` 을 업데이트 하는 명령
#### Request Example
```json
gacha/admin, {"order": "update", "characters": characters}
```
#### Response
- ##### Status : 200(OK) || 400(ERROR)
- ##### Payload : none

***

### Gacha Pool Add

- ##### Page : /gacha/admin
- ##### Method : post
- ##### Payload :
> ###### order : order types [ <span style="color:red"> string </span> ] ( "add" )
> ###### meta.type : new gacha pool type [ <span style="color:red"> string </span> ] ( "featured" || "limited" )
> ##### meta.name : new gacha pool name [ <span style="color:red"> string </span> ] (ex: "DEEP DROWN")
> ##### meta.code : new gacha pool code [ <span style="color:red"> string </span> ] (ex: "gacha_pool_1001")
- ##### etc: 
서버 저장소에 저장되어 있는 character json data를 기반으로 새로운 gacha pool 데이터를 생성함. 기본 character json data 에는 ```limited``` 와 ```unobtainable``` 항목이 존재함.
만약 ```type: "limited"``` 일 경우 전체 캐릭중 ```unobtainable```항목의 오퍼레이터를 제외한 모든 캐릭터를 가챠 풀의 형태로 정제하여 새로운 가챠 풀을 생성함.

만약 ```type: "featured"``` 일 경우 전체 캐릭 중 ```unobtainable```과 ```limited``` 항목에 포함되는 오퍼레이터를 제외한 모든 캐릭터를 가챠 풀의 형태로 정제하여 새로운 가챠 풀을 생성함.
가챠 풀 내부의 ```meta``` 항목에는 post로 전해준 type, name, code를 포함시킴

어드민이 나중에 픽업 대상을 ```edit``` 할 수 있도록 ```featured```항목에는 아무것도 없는 빈 배열을 담는다.

이후, 가챠 풀을 json형태로 payload에 담아 반환.

#### Request Example
```json
/gacha/admin, {"order": "add", "meta":{"type":type, "name":name, "code": code}}
```
#### Response
- ##### status : 200(OK) || 400(ERROR)
- ##### payload :
> ###### pool : generated gacha pool json [ <span style="color:red"> json object </span> ]

#### Response Example
```json
///success
200, {"pool": {
"meta": {"type":..."name":..."code":...},
"5":[...6star characters...],
"4": [...5star characters...],
...,
"featured":[]}}


/// error
400, {"message": "meta information collision"} // 메타 정보 중 unique 한 항목(code, name) 이 기존 가챠 풀에 존재할 경우
```

***

### Gacha Pool Edit

- ##### Page : /gacha/admin
- ##### Method : post
- ##### Payload :
> ###### order : order types [ <span style="color:red"> string </span> ] ( "edit" )
> ###### code : gacha pool edit target [ <span style="color:red"> string </span> ]
> ###### pool : gacha pool json [ <span style="color:red"> json object </span> ]
- ##### etc:
```add``` 명령을 통해 받은 데이터를 토대로 admin이 수정을 가함. 수정된 gacha pool을 json형식으로 payload에 담아 보냄
#### Request Example
```json
/gacha/admin, {"order": "edit", "code":code, "pool": {
"meta": {"type":..."name":..."code":...},
"5":[...6star characters...],
"4": [...5star characters...],
...,
"featured":["char_123_abc", "char_234_bcd", "char_345_cde"]} }
```
#### Response
- ##### Status : 200(OK) || 400(ERROR)
- ##### Payload : none

***

### Gacha Pool Delete
- ##### Page : /gacha/admin
- ##### Method : post
- ##### Payload :
> ###### order : order types [ <span style="color:red"> string </span> ] ( "delete" )
> ###### code : gacha pool delete target [ <span style="color:red"> string </span> ] (ex: "gacha_pool_1001")
- ##### etc :
혹시 ```add```나 ```edit```명령으로 잘못된 pool이 업데이트 되었을 경우를 대비한 명령
#### Request Example
```json
/gacha/admin, {"order":"delete", "code": code}
```
#### Response
- ##### Status : 200(OK) || 400(ERROR)
- ##### Payload : none

