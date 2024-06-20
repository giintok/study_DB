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


##### 2. 예악어
* SELECT 문 [데이터 가져오기]
    * `select 무엇(컬럼등),무엇 from 위치(테이블명);`

|DB보기|Table 보기|Table 설정보기||
|:-------:|:-------:|:------------:|:---------------------:|
| show databases; | show tables; | describe 테이블명; |   |
| **데이터 가져오기**                     | **별명을 붙여서 가져오기**           |   |
| Select 컬럼명 From 테이블명;            | Select 컬럼명 As 별명 From 테이블명; |   |
| **조건 데이터**                         |                                      |   |
| Select 컬럼명 From 테이블명 Where 조건; |                                      |   |

* WHERE 구 조건
    * LIKE - NOT LIKE
    * IS NULL - IS NOT NULL
    * 논리 연산자 사용 가능
    * 연산자 우선 순위 [괄호 - BINARY - ! - 곱*, 나눗셈/ - 단항 연산자(- ,+) - 비교 연산자(<=>등) - NOT - AND - XOR - OR ]
    같은 순위의 경우 앞에서부터 차례대로 시행

| SQL 구문 | 설명 | 비고 |
|-------|-------|----|
| `SELECT * FROM 테이블명 WHERE 컬럼명 >= 3;` | `컬럼명`의 값이 3 이상인 모든 데이터를 선택합니다. | 비교 연산자 사용 |
| `SELECT * FROM 테이블명 WHERE 컬럼명 = '문자열';` | `컬럼명`이 '문자열'과 **일치**하는 모든 데이터를 선택합니다. | 대소문자 구분 없음, 끝의 공백 무시 |
| `SELECT * FROM 테이블명 WHERE BINARY 컬럼명 = '문자열';` | `컬럼명`이 '문자열'과 **완벽하게 일치**하는 데이터를 선택합니다. | 대소문자 구분 및 정확한 문자열 일치 요구 |
| `SELECT * FROM 테이블명 WHERE 컬럼명 LIKE '문자열%';` | `컬럼명`이 '문자열'로 시작하는 모든 데이터를 선택합니다. | 대소문자 구분 없음, 끝의 공백 고려 |
| `SELECT * FROM 테이블명 WHERE 컬럼명 LIKE '_문자열';` | `컬럼명`이 임의의 한 문자로 시작하고 그 뒤에 '문자열'이 이어지는 모든 데이터를 선택합니다. | `_`는 임의의 한 문자를 나타냄 <br> 이스케이프 처리 \%  <br> [주로 % , _ , \ , ' , " , n , t , b , r '] |
| `SELECT * FROM 테이블명 WHERE 컬럼명 = '';` | `컬럼명`이 빈 문자열인 모든 데이터를 선택합니다. | 길이가 0인 문자열 선택 |
| `SELECT * FROM 테이블명 WHERE 컬럼명 IS NULL;`  | `컬럼명`이 NULL인 모든 데이터를 선택합니다. | NULL 값 필터링 |
| (NOT) BETWEEN a AND b <br> (NOT) IN (a,b,c) | a 이상 b 이하인 경우 1 반환 <br> a,b,c중 하나라도 일치하면 1 반환 | 범위 지정이라 날짜 지정에도 사용 가능|

* 데이터 형

    | 종류     | 데이터형                   | 특징                                   | 사용         |
    |:--------:|:--------------------------:|:--------------------------------------:|:-------------:|
    | 문자열   | CHAR<br>VARCHAR<br>TEXT    | 고정길이<br>가변길이<br>가변길이 대용량 |  char(10) 항상 10자리사용<br>varchar(255) 255자까지<br>문서같은 대용량 |
    | 정수     | TINYINT<br>INT<br>BIGINT   | 1byte<br>4bytes<br>8bytes              | 컬럼명 Int;<br>지정해도 지정안해도 됨<br>보통 3자리, 10자리, 19자리 |
    | 실수     | FLOAT<br>DOUBLE<br>DECIMAL | 4byte<br>8bytes<br>가변적              | 정밀도 차이<br>Float와 Double는 고정된 크기<br>float(전체 자릿수,소수점 이하) |
    | 날짜시간 | DATE<br>DATETIME           | 3bytes<br>8bytes                       | 'YYYY-MM-DD'형식<br>'YYYY-MM-DD HH:MM'형식 |
    | 부울     | BOOLEAN                    | mysql에서 제공                         | Tinyint(1)을 주로 사용 |
    | 위도경도 | GEOMETRY                   | 공간 데이터 저장                       | POINT, LINESTRING, POLYGON등<br>지리적 데이터 저장 |

##### 3. 데이터

* 집계 함수 위치 : SELECT 구, HAVING 구, ORDER BY 구 
* 나중에 NULL 0으로 계산하는 법 추가 
* 
| 키워드 | 설명 | 예시 및 비고 |
|--------|------|--------------|
| DISTINCT| 각 행 전체에서 중복된 것을 1개로 통합하여 처리 | `SELECT DISTINCT 컬럼명, 컬럼명 FROM 테이블명;`|
| COUNT | 집계 함수, 특정 조건을 만족하는 행의 수를 계산 | `SELECT COUNt(DISTINCT 컬럼명) FROM  테이블명;` |
| SUM, MAX, MIN, AVG| 집계 함수, 컬럼의 합계값, 최댓값, 최솟값, 평균값 | 문자열은 사전순, 날짜는 최신 or 오래된 순|

* GROUP BY 구 = 집약 키
    SELECT 컬럼명 AS 벌명 FROM 테이블명  GROUP BY 컬럼명;
    순서에 의해 SELECT 컬럼명 AS 벌명 FROM 테이블명  GROUP BY 별명; 은 에러가 나온다.
    주의 : 1개의 구룹에 대해 결과가 여러 개가 되는 데이터를 가져올 수 없다. 즉 SELECT에는 상수, 집계함수, 집약키의 컬러명만 올 수 있다.
    SELECT '상수', 집약키, COUNT(*) FROM 테이블명 GROUP BY 집약키;

* HAVING 구 : 그룹에 조건을 주기 위해서
    SELECT 컬럼명 FROM 테이블명  GROUP BY 컬럼명 HAVING 조건;

* SQL 실행 순서
    처리시간이 걸리니 미리 레코드 수를 줄이자
    |구|설명|
    |--|--|
    | FROM | 테이블을 지정하고 전체 레코드 추출 |
    | WHERE | 조건에 따라 레코드 축소 |
    | GROUP BY | 그룹을 통합 |
    | HAVING | 그룹에 조건을 줘서 그룹 축소 |
    | SELECT | 가저올 데이터 선택 |
    | DISTINCT | 중복 데이터 통합 |

