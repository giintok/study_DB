[마크다운 작성 사이트](https://dillinger.io/)

RDBMS_MYSQL 정리
=============

##### 1. 접속

* CMD에서 쿼리 입력 
    * `chcp 949, utfkr` 
    cmd에서 한글 문자 포함 표시, utf-8 인코딩
    * `chcp 65001, utf-8`

* 접속
    * `mysql -u root -p`  
      유저 관리자 -비밀번호


##### 2. 미정
* select 문 [데이터 가져오기]
    * `select 무엇(컬럼등),무엇 from 위치(테이블명);`

|DB보기|Table 보기|Table 설정보기||
|:-------:|:-------:|:------------:|:---------------------:|
| show databases; | show tables; | describe 테이블명; |   |
| **데이터 가져오기**                     | **별명을 붙여서 가져오기**           |   |
| Select 컬럼명 From 테이블명;            | Select 컬럼명 As 별명 From 테이블명; |   |
| **조건 데이터**                         |                                      |   |
| Select 컬럼명 From 테이블명 Where 조건; |                                      |   |

* 조건 (표로 할 것)
    * `Select * From 테이블명 Where 컬럼명 = 3;`
    테이블에서 3이상의 데이터 전체 가져오기 _ 비교 연산자 사용
    * `Select * From 테이블명 Where 컬럼명 = '문자열';`
    문자와 일치하는 데이터 가져오기
    주의 : 대소문자 구분 없음, 끝의 공백은 무시
    * `Select * From 테이블명 Where Binary 컬럼명 = '문자열';`
    완벽하게 일치하는 데이터만 가져오기
    * `Select * From 테이블명 Where 컬럼명 like '문자열%';`
    일부분만 일치 like, not lkie 
    [% : 로 시작하는 ex 김% 김으로 시작하는 이름 전체], [_ : 임의 문자 1문자]
    대소문자는 구분 안함, 끝에 공백은 생각함
    * 이스케이프 처리 \% [주로 % , _ , \ , ' , " , n , t , b , r ']
    * 논리 연산자 

* 데이터 형

    | 종류     | 데이터형                   | 특징                                   | 사용         |
    |:--------:|:--------------------------:|:--------------------------------------:|:-------------:|
    | 문자열   | CHAR<br>VARCHAR<br>TEXT    | 고정길이<br>가변길이<br>가변길이 대용량 |  char(10) 항상 10자리사용<br>varchar(255) 255자까지<br>문서같은 대용량 |
    | 정수     | TINYINT<br>INT<br>BIGINT   | 1byte<br>4bytes<br>8bytes              | 컬럼명 Int;<br>지정해도 지정안해도 됨<br>보통 3자리, 10자리, 19자리 |
    | 실수     | FLOAT<br>DOUBLE<br>DECIMAL | 4byte<br>8bytes<br>가변적              | 정밀도 차이<br>Float와 Double는 고정된 크기<br>float(전체 자릿수,소수점 이하) |
    | 날짜시간 | DATE<br>DATETIME           | 3bytes<br>8bytes                       | 'YYYY-MM-DD'형식<br>'YYYY-MM-DD HH:MM'형식 |
    | 부울     | BOOLEAN                    | mysql에서 제공                         | Tinyint(1)을 주로 사용 |
    | 위도경도 | GEOMETRY                   | 공간 데이터 저장                       | POINT, LINESTRING, POLYGON등<br>지리적 데이터 저장 |
    
