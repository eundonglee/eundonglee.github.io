---
title:  "JPQL Cross Join의 늪"
tags:
  - 개발
  - JAVA
  - JPQL
categories:
  - JAVA


---

프로젝트 진행 중 JPQL을 활용하는 부분에서 의도하지 않은 Cross Join이 발생하는 것을 발견하였다. 사실 쿼리 효율성에 있어서 치명적인 문제였고,반드시 제거되어야만 하는 부분이었다. 그 해결의 과정을 기록해 두고자 한다.

## 크로스 조인이 뭐죠...?

아마도 ~~나 빼고는~~ 다들 잘 알고 있을 테지만... 크로스 조인이 이루어질 경우 조인되는 두 테이블 내에 존재하는 모든 값 쌍(Cartesian Product)이 산출된다. 즉 A 테이블과 B 테이블에 각각 M, N개의 데이터가 존재한다고 했을 때, 두 테이블 간의 크로스 조인이 이루어지게 되면 ```M * N * ({A 테이블 한 row의 DataSize} + {B 테이블 한 row의 DataSize})``` 만큼의 데이터가 DB의 메모리에 올라간다. 두 테이블의 데이터가 같은 속도로 축적된다는 가정 하에 크로스 조인에 의해 산출되는 데이터의 양은 지수적으로 증가하고, 이에 따라 DB의 부하 또한 그에 비례하여 증가하게 된다.
~~즉 있어서는 안 되는 것이 나의 코드에 있었다.~~

### 그런데 JPQL 쿼리에서 암묵적인 조인은 크로스 조인으로 처리된다.

## 이게 내 쿼리에서 발생한 크로스 조인 문제와 무슨 상관이 있었냐 하면

```
SELECT b
FROM Booking b
ORDER BY b.showtime.startDatetime;
```

이런 식으로 Booking 테이블을 조회할 때, Booking.showtime.startDatetime 기준(예매된 영화의 시작시간 기준)으로 정렬하겠다고 JPQL 쿼리 내에 선언하는 경우에는 Booking 테이블과 Showtime 테이블이 암묵적으로 조인된다. 왜냐하면 startDatetime은 Showtime 테이블 안에 있으니까.

**그리고 JPA는 관계가 명시되지 않은 Booking 테이블과 Showtime 테이블을 크로스 조인으로 처리한다.**

설령 앞에서 ```LEFT JOIN Showtime``` 을 했어도, JPA는 앞서의 조인을 무시하고 다시 한번 크로스 조인을 해 준다. 왜냐하면 Showtime이랑 Booking.showtime은 다르기 때문에.

## 그래서 어떻게 해야 하냐면

```
SELECT b 
FROM Booking b 
LEFT JOIN Showtime st 
ORDER BY st.startDatetime;
```

이렇게 조인 처리를 하고 앨리어스(```st```)까지 명시해 주면 크로스 조인을 막을 수 있었다.

비슷한 맥락에서 JPQL을 사용한 메소드에서 Sort("showtime.startDatetime") 와 같이 다른 테이블의 칼럼을 기준으로 한 정렬 조건을 파라미터로 받는 경우에도 동일한 크로스 조인 문제가 발생했다.
그래서 외부 테이블과 조인이 필요한 조건의 정렬은 외부 인자를 받지 않고 쿼리 자체에서 처리하는 방식으로 바꿨다.
(그런데 pageable에 포함돼서 들어오는 Sort의 경우에는 동일한 상황에서 크로스 조인 문제가 발생하지 않더라...;; 왜 그럴까...)

## 결과적으로...

```
SELECT b 
FROM Booking b 
LEFT JOIN FETCH Showtime st 
ORDER BY st.startDatetime;
```

엔티티그래프를 다 **조인 페치**로 바꾸고, 앨리어스를 명시해서 크로스 조인 문제를 해결했다. 단순히 크로스 조인만 막으면 되는 게 아니라 쿼리로 가져온 엔티티 객체 + 해당 엔티티가 참조하고 있는 엔티티 객체 데이터까지 사용할 필요가 있었기 때문이다.

## 그런데 문제가 뭐였냐면...

그래서 조인 페치로 다 바꾸고 앨리어스도 잘 명시해 주고 **페이지네이션**을 붙여주면

**안 돌아간다 ^^**

**왜냐하면...**

1) 페이지네이션을 하면 상황에 따라서 JPA가 카운트 쿼리를 추가로 돌린다. (어떨 땐 돌리고 어떨 땐 안 돌리는데, 이 부분은 추가적으로 학습이 필요할 듯하다.)
여기서는 다른 조건 다 동일하고 select 부분에서 조회하는 변수만 count 함수 형태로 바뀌게 된다.(원래 쿼리에서 booking이었으면, 카운트 쿼리에서는 count(booking)을 조회하는 식이다.)

```
# 메인 쿼리
SELECT b 
FROM Booking b 
LEFT JOIN FETCH b.showtime st 
ORDER BY st.startDatetime;

# 카운트 쿼리
SELECT count(b) 
FROM Booking b 
LEFT JOIN FETCH b.showtime st 
ORDER BY st.startDatetime;
```
즉 페이지네이션 시 이런 식으로 추가 쿼리가 발생한다.

2) 그런데 조인 페치의 부모에 해당하는 엔티티 객체(위의 경우에는 Booking b)는 **반드시** 셀렉트 절 안에 들어가야 한다. 안 그러면 에러가 발생한다.
(왜냐하면 굳이 조인 페치로 불러서 메모리에 로딩해 놓고 가져다 쓰지 않는 건 논리적으로 말이 안 되니까...?) 

그런데 위에서 봤듯이 조인 페치 후에 페이지네이션 하면, 아래와 같은 카운트 쿼리가 발생한다.
```
SELECT count(b) 
FROM Booking b 
LEFT JOIN FETCH b.showtime st 
ORDER BY st.startDatetime;
```
결과적으로 조인 페치의 부모에 해당하는 b 가 SELECT 절에 없어서 에러가 난다..

**그래서**

조인 페치와 함께 페이지네이션을 쓰려면, **메인 쿼리**와 더불어 페이지네이션에 필요한 **카운트 쿼리**를 별도로 나타내 줘야 한다. 당연히 카운트 쿼리에는 **조인 페치** 대신 **일반 조인**을 사용해야 한다.
```
@Query(value = "SELECT b 
                FROM Booking b  
                LEFT JOIN FETCH b.showtime st 
                ORDER BY st.startDatetime",
       countQuery = "SELECT count(b)
                FROM Booking b
                LEFT JOIN b.showtime st 
                ORDER BY st.startDatetime"
    )
```

참고 :  <https://stackoverflow.com/questions/21549480/spring-data-fetch-join-with-paging-is-not-working>

- **진짜 끝**

아래는 예매내역 조회를 위해 사용되는 쿼리 중 일부이다.
``` 
@Query(value = "select b from Booking b "
            + "left join fetch b.showtime st "
            + "left join fetch st.movie m "
            + "where st.startDatetime >= :fromStartDatetime "
            + "and b.memberId = :memberId "
            + "and b.bookingStatusCode in :bookingStatusSet",

            countQuery = "select count(b) from Booking b "
            + "left join b.showtime st "
            + "left join st.movie m "
            + "where st.startDatetime >= :fromStartDatetime "
            + "and b.memberId = :memberId "
            + "and b.bookingStatusCode in :bookingStatusSet"
    )
```