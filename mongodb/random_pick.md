# Mongodb에서 임의로 선택하기
## 왜 필요한가?
Random하게 doc를 선택하여 사용자에게 contents를 제공하려 할 때, 단순한 방법을 사용하게 되면 시간이 오래걸린다. db를 사용하는 이점을 살려보자.

## 방법론 소개
1. random 수를 골라 skip하기
  ```javascript
  var query = { state: 'OK' };
  var n = db.myCollection.count(query);
  var r = Math.floor(Math.random() * n);
  var randomElement = db.myCollection.find(query).limit(1).skip(r);
  ```
  간편하지만 전문성이 떨어지고 속도가 느리다. 그리고 count와 find 사이에 delete가 진행될 경우 동시성 문제가 발생한다.([링크](http://stackoverflow.com/questions/2824157/random-record-from-mongodb#comment2895058_2824166)) 이런 coding은 지양해야한다. 그리고 비슷한 시간대에 random함수를 돌려서 그런지 중복이되는 느낌을 지우기 힘들다.
2. random을 위한 index를 하나 더 만들기
  - query 방법 1
  ```javascript
  var query = {
    state: 'OK',
    rnd: {
        $gte: Math.random()
    }
  };
  var randomElement = db.myCollection.findOne({ $query: query, $orderby: { rnd: 1 } });
  ```
  - query 방법 2
  ```javascript
  var query = {
    state: 'OK',
    rnd: {
        $gte: Math.random()
    }
  };
  var randomElement = db.myCollection.findOne(query);
  ```

  속도는 첫번째 방법보다는 빠른 편, random때문에 index를 만들어야 할까? 속도는 query 방법2가 셋중에 그나마 빠르다고 한다. [링크](http://bdadam.com/blog/finding-a-random-document-in-mongodb.html)
3. aggregate를 활용한 방법 3.2 mongodb
```javascript
db.mycoll.aggregate(
   { $sample: { size: 1 } }
)
```
aggregation pipeline인 sample을 사용하는 방법으로 간편하고 효율적이다. db에서 제공하는 최적화된 operator를 사용하는 것이기 때문에 안정적이다.
잘 설명된 블로그 포스트를 참고하자. [링크](https://www.mongodb.com/blog/post/how-to-perform-random-queries-on-mongodb)

## 참고 link
http://bdadam.com/blog/finding-a-random-document-in-mongodb.html
http://stackoverflow.com/questions/2824157/random-record-from-mongodb
https://www.mongodb.com/blog/post/how-to-perform-random-queries-on-mongodb
