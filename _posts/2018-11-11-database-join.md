---
layout: post
title: DB Join
tags: [조인, Join]
---
> DB Join의 개념과 종류에 대해 이해한다.
<!--more-->

### 조인(Join)이란
* **한 데이터베이스 내의 여러 테이블의 레코드를 조합하여 하나의 열로 표현한 것**이다.
* 조인은 테이블로서 저장되거나, 그 자체로 이용할 수 있는 결과 셋을 만들어 낸다.

### 조인의 필요성
* 관계형 데이터베이스의 구조적 특징으로 정규화를 수행하면 의미 있는 데이터의 집합으로 테이블이 구성되고, 각 테이블끼리는 관계(Relationship)를 갖게 된다.
* 이와 같은 특징으로 관계형 데이터베이스는 저장 공간의 효율성과 확장성이 향상되게 된다.
* 다른 한편으로는 서로 관계있는 데이터가 여러 테이블로 나뉘어 저장되므로, 각 테이블에 저장된 데이터를 효과적으로 검색하기 위해 조인이 필요하다.

### 조인의 종류
<img src="{{ site.baseurl }}/assets/img/post/join-table.png" width="100%" height="100%">

#### 1. 내부 조인(INNER JOIN)
* 여러 애플리케이션에서 사용되는 가장 흔한 결합 방식이며, 기본 조인 형식으로 간주된다.
* 내부 조인은 조인 구문에 기반한 2개의 테이블(A, B)의 컬럼 값을 결합함으로써 새로운 결과 테이블을 생성한다.
* **명시적 조인 표현(explicit)**과 **암시적 조인 표현(implicit)** 2개의 다른 조인식 구문이 있다.
* **명시적 조인 표현**
    * 테이블에 조인을 하라는 것을 지정하기 위해 JOIN 키워드를 사용하며, 그리고 나서 다음의 예제와 같이 ON 키워드를 조인에 대한 구문을 지정하는데 사용한다.
    ```sql
    SELECT *
    FROM employee INNER JOIN department
    ON employee.DepartmentID = department.DepartmentID;
    ```
* **암시적 조인 표현**
    * SELECT 구문의 FROM 절에서 그것들을 분리하는 컴마를 사용해서 단순히 조인을 위한 여러 테이블을 나열하기만 한다.
    ```sql
    SELECT *
    FROM employee, department
    WHERE employee.DepartmentID = department.DepartmentID;
    ```
* 결과

<img src="{{ site.baseurl }}/assets/img/post/inner-join.png" width="90%" height="90%">

#### 1-1. 동등 조인(EQUI JOIN)
* 비교자 기반의 조인이며, 조인 구문에서 **동등비교만을 사용**한다.
* 다른 비교 연산자(<와 같은)를 사용하는 것은 동등 조인으로서의 조인의 자격을 박탈한다.

#### 1-2. 자연 조인(NATURAL JOIN)
* 동등 조인의 한 유형으로 조인 구문이 조인된 테이블에서 **동일한 컬럼명을 가진 2개의 테이블**에서 모든 컬럼들을 비교함으로써, 암시적으로 일어나는 구문이다.
* 결과적으로 나온 조인된 테이블은 동일한 이름을 가진 컬럼의 각 쌍에 대한 단 하나의 컬럼만 포함하고 있다.
```sql
SELECT * FROM employee NATURAL JOIN department;
```
* 결과

<img src="{{ site.baseurl }}/assets/img/post/natural-join.png" width="70%" height="70%">

#### 1-3. 교차 조인(CROSS JOIN)
* 조인되는 두 테이블에서 **곱집합을 반환**한다.
* 즉, 두 번째 테이블로부터 각 행과 첫 번째 테이블에서 각 행이 한번씩 결합된 열을 만든다.
* 예를 들어 m행을 가진 테이블과 n행을 가진 테이블이 교차 조인되면 m*n 개의 행을 생성한다
```sql
SELECT * FROM employee CROSS JOIN department;
```
```sql
SELECT * FROM employee, department;
```
* 결과

<img src="{{ site.baseurl }}/assets/img/post/cross-join.png" width="90%" height="90%">

#### 2. 외부 조인(OUTER JOIN)
* 조인 대상 테이블에서 **특정 테이블의 데이터가 모두 필요한 상황**에서 외부 조인을 활용하여 효과적으로 결과 집합을 생성할 수 있다.

#### 2-1. 왼쪽 외부 조인(LEFT OUTER JOIN)
* 우측 테이블에 조인할 컬럼의 값이 없는 경우 사용한다.
* 즉, **좌측 테이블의 모든 데이터를 포함**하는 결과 집합을 생성한다.
```sql
SELECT *
FROM employee LEFT OUTER JOIN department
ON employee.DepartmentID = department.DepartmentID;
```
* 결과

<img src="{{ site.baseurl }}/assets/img/post/left-outer-join.png" width="90%" height="90%">

#### 2-2. 오른쪽 외부 조인(RIGHT OUTER JOIN)
* 좌측 테이블에 조인할 컬럼의 값이 없는 경우 사용한다.
* 즉, **우측 테이블의 모든 데이터를 포함**하는 결과 집합을 생성한다.
```sql
SELECT *
FROM employee RIGHT OUTER JOIN department
ON employee.DepartmentID = department.DepartmentID;
```
* 결과

<img src="{{ site.baseurl }}/assets/img/post/right-outer-join.png" width="90%" height="90%">

#### 2-3. 완전 외부 조인(FULL OUTER JOIN)
* 양쪽 테이블 모두 OUTER JOIN이 필요할 때 사용한다.
```sql
SELECT *
FROM employee FULL OUTER JOIN department
ON employee.DepartmentID = department.DepartmentID;
```
* 결과

<img src="{{ site.baseurl }}/assets/img/post/full-outer-join.png" width="90%" height="90%">

#### 3. 셀프 조인(SELF JOIN)
* **한 테이블에서 자기 자신에 조인**을 시키는 것이다.

### 조인을 사용할 때 주의사항
* SQL 문장의 의미를 제대로 파악
    * SQL을 어떻게 작성하느냐에 따라 성능이 크게 좌우된다.
    * 어떤 질의를 수행할 것인지를 명확하게 정의한 후, 비효율을 제거하여 최적의 SQL을 작성해야 한다.
* 명확한 조인 조건 제공
    * 조인 조건을 명확하게 제공하지 않을 경우, 의도치 않게 CROSS JOIN(Cartesian Product)이 수행될 수 있다.

### 조인을 사용할 때 고려사항
* 조인할 대상의 집합을 최소화
    * 집합을 최소화할 방법이 있으면, 조건을 먼저 적용하여 관계를 맺을 집합을 최소화한 후, 조인을 맺는 것이 효율적이다.
* 효과적인 인덱스 활용
    * 인덱스를 활용하면, 조인 연산의 비용을 낮출 수 있다.

### Reference
- [https://ko.wikipedia.org/wiki/Join_(SQL)](https://ko.wikipedia.org/wiki/Join_(SQL))
- [https://blog.ngelmaum.org/entry/lab-note-sql-join-method](https://blog.ngelmaum.org/entry/lab-note-sql-join-method)