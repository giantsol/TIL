# Get Largest Value

**person**이라는 테이블에서 age가 가장 높은 한 사람을 쿼리하고 싶을 때

```sqlite
SELECT * FROM person WHERE age = (SELECT MAX(age) FROM person);
```

위 처럼 할 수도 있지만 이렇게 하면 `SELECT MAX(age) FROM person`을 계산하면서 소팅을 하기 때문에 느리다고 함.

그래서 아래와 같이 하는게 더 간결하기도 하고 더 빠름.

```sqlite
SELECT * FROM person ORDER BY age DESC LIMIT 1;
```
