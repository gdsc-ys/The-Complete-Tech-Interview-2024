### Q1. 트랜잭션이 무엇이고, ACID 원칙에 대해 설명해 주세요.
A1. 트랜잭션이란 데이터베이스에서 한번에 수행되어야 할 일련의 연산 모음 = 일련의 쿼리 집합 = 데이터베이스의 상태를 변화시키는 수행 작업의 단위이다.

- Atomicity(원자성): 트랜잭션은 db에 모두 반영되거나 모두 반영되지 않아야 한다.
    - e.g. 인출과 입금은 동시에 성공해야한다.
- Consistancy(일관성): 트랜잭션 수행 전과 후의 데이터베이스의 일관성이 유지되어야 한다.
    - e.g. 트랜잭션이 db의 제약조건을 위반하는 변화를 이끌어서는 안된다; 데이터가 정확한 값임을 보장하는 것은 아니다.
- Isolation(독립성): 동시에 여러 트랜젝션이 실행될 때 각 트랜젝션은 서로 독립적으로 실행되어야한다. 
- Durability(지속성): 트랜젝션이 완료되면 그 결과는 db 장애가 발생하더라도 영구적으로 반영되어야 한다.

### Q2. ACID 원칙 중, Durability를 DBMS는 어떻게 보장하나요?
A2. MySQL의 경우 snapshot을 undo 영역에 넣어 보장한다. 

### Q3. 트랜잭션을 사용해 본 경험이 있나요? 어떤 경우에 사용할 수 있나요?
A3. 

### Q4. 읽기에는 트랜잭션을 걸지 않아도 될까요?
A4. 모든 읽기 작업에 트랜잭션이 필요한 것은 아닙니다. 그러나 동시성과 데이터 일관성이 중요한 경우에는 트랜잭션을 사용하면 Repeatable read 이상의 격리 레벨에서 일관된 데이터 뷰를 유지할 수 있습니다. autocommit의 경우 단일 select 자체가 이미 transaction입니다.

### References
https://dev.mysql.com/doc/refman/8.0/en/innodb-performance-ro-txn.html
https://michaeljswart.com/2011/09/mythbusting-concurrent-updateinsert-solutions/
https://stackoverflow.com/questions/3098644/whats-the-point-to-enclose-select-statements-in-a-transaction