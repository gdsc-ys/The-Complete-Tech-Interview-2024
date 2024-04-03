### Q1. Key (기본키, 후보키, 슈퍼키 등등...) 에 대해 설명해 주세요.

### Q2. 기본키는 수정이 가능한가요?
A2. 기존 기본키를 삭제 후 재등록하면 가능하다.

### Q3. 사실 MySQL의 경우, 기본키를 설정하지 않아도 테이블이 만들어집니다. 어떻게 이게 가능한 걸까요? 
A3. InnoDB 엔진은 pk기반 Cluster Index를 사용한다. 즉 PK는 테이블마다 정의가 되어있어야한다.
    만약 unique하고 non-null column이 없으면 auto-increment column을 추가하여 pk로 사용하여야 한다.
    pk를 정의하지 않았다면 InnoDB는 Not NULL로 정의된 첫번째 UNIQUE index를 cluster index로 사용한다.
    그런데 만약 pk의 역할을 할 column 조차 없다면 (no pk or suitable UNIQUE index)InnoDB는 row id값을 가진 GEN_CLUST_INDEX를 hidden clustered index로 생성한다. row ID는 6바이트 필드이며 새로운 row가 추가될 때마다 증가한다. 


### Q4. 외래키 값은 NULL이 들어올 수 있나요?
A4. 외래키는 NULL일 수도 있고 중복일 수도 있다. 외래키라함은 다른(상위) 테이블의 PK로 테이블간의 관계를 나타내기 위해 쓴다. 
    NULL은 데이터가 아예 없는 것이다.예를들어, 상위테이블이 Department고 하위테이블이 Employee일 때 아직 부서 배정이 완료되지 않은
    신입사원일 경우 Department FK는 NULL일 것이다.

### Q5. 어떤 칼럼의 정의에 UNIQUE 키워드가 붙는다고 가정해 봅시다. 이 칼럼을 활용한 쿼리의 성능은 그렇지 않은 것과 비교해서 어떻게 다를까요?
A5. 사실상 unique key는 unique index와 같다. (내부 구현은 동일하다) 사용목적에 따라 적절하게 쓰면 된다. 
    따라서 unique 키워드가 붙은 칼럼을 활용한 쿼리의 성능은 index를 타는 쿼리의 성능을 기대할 수 있으므로 그렇지 않은 것보다 향상될 것이다. 삽입을 할 땐 tree 안 요소들과 삽입할 요소가 다른지 (unique한지) 확인해야하므로 시간이 걸린다.

### References
https://dev.mysql.com/doc/refman/8.0/en/innodb-index-types.html
https://stackoverflow.com/questions/7573590/can-a-foreign-key-be-null-and-or-duplicate
https://stackoverflow.com/questions/10263760/unique-key-vs-unique-index-on-sql-server-2008
