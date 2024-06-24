[마크다운 작성 사이트](https://dillinger.io/)
1. 예약어랑 함수 나눠서 표로 정리하기...
2. 예문 추가하기
3. DB 생성 및 삭제 데이터 추가하는거 CMD 사진 추가..
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
    SELECT 컬럼명 FROM 테이블명  GROUP BY 컬럼명(=집약키);
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
    |ORDER BY | 정렬 |

* ORDER BY 레코드 정렬
    SELECT * FROM 테이블명 ORDER BY 컬럼명(=정렬키) ASC;
    ASC : 오름차순(생략시 ASC), DESC : 내림차순
    같은 순위인 경우 순서가 정해지 않으므로, 컬럼1 ASC, 컬럼2 ASC 로 순서를 정해준다.
    SELECT 컬럼1, 컬럼2 FROM 테이블명 ORDER BT 2 DESC;
    컬럼1 = 1, 컬럼2 = 2가 되어서 컬럼2 가 내림차순이 된다. (*의 경우 테이블의 컬럼 순)

* 인덱스 기본키로하는것 만 배움
    ALTER TABLE 테이블명 ADD PRIMARY KEY (컬럼명);

MYSQL에서만 사용 가능
* LIMIT : 특정 행 가지고오기 단 일반적으로는 첫 행을 기준으로 가지고 온다
* OFFSET : 시작 위치를 정해서 LIMIT 를 이용 지정한 행 수를 가지고 온다. 주의점은 첫행이 1이 아닌 0부터 시작
    LIMIT 3 OFFSET 5 :시작위치 5=6번째줄 에서 시작하여 3행을 가지고 온다.
    LIMT 2,3; = LIMIT 3 OFFSET 2; 로 순서가 바뀌어서 들어간다 햇갈린다.

* 조건 분류
    CASE : 컬럼에 따른 조건을 각각 주고싶을 떄, IF문과 비슷 
    SELECT 컬럼1, 컬럼2
        CASE
          WHEN 조건1 THEN 처리1
          WHEN 조건2 THEN 처리2
          ELSE 처리0
        END
    FROM 테이블명;

* 컬럼 값이 딱 일치하는지 아닌지 판단할 떄, 문자열이나 날짜등
    SELECT 컬럼1
        CASE 컬럼2
          WHEN 값1 THEN 처리1
          WHEN 값2 THEN 처리2
          ELSE 처리0
        END
    FROM 테이블명;

* IF 함수 로도 조건 분류 가능 알고있는거랑 사용법 비슷 SELECT, WHERE, ORDER BY 구등에서 사용
    IF (조건, TRUE일 때 반환 값, FALSE일 떄 반환 값)
    IF (조건, TRUE일 떄 반환 값, 
        IF(조건2, 조건2 TRUE일 때 반환 값, 조건2 FALSE일 떄 반환 값))

* NULL 대응
    NULL을 0등의 값으로 변환
    1. COALESCE(인수1, 인수2, --- , 인수N)
    인수1이 NULL이 아니면 인수1을 반환
    인수1이 NULL인 경우 인수2를 체크해서 다시 NULL이 아니면 인수2반환 NULL이면 인수3 이런식으로
    COALESCE(컬럼명, 0) : 컬럼값이 NULL이면 0을 반환, NULL이 아니면 컬럼 값을 반환
    2. IFNULL(인수1, 인수2)
    NULL을 반환하는 함수 특정 값을 NULL로 변환할때나 비교를 통해 상태 처리할 때, 나눗셈 오류 방지
    1. NULLIF(인수1, 인수2)
    인수1 = 인수2 면 NULL을 반환, 같지않으면 인수1을 반환

* CAST : 데이터형을 변환
    CAST(변환하는 데이터 AS 변환하는 데이터형)
    SELECT CAST(12345 AS CHAR(3));
    이 경우 '123'인 결과로 왜냐하면 숫자 12345를 길이 3으로 잘랐기 때문 
