
### Q1. RDB와 NoSQL의 차이에 대해 설명해 주세요.
A1. RDB는 관계형 데이터베이스로 정해진 데이터 스키마를 기반으로 row와 column으로 구성된 tabular format으로 data를 저장합니다. 구조화된 쿼리 언어 (SQL)로 데이터베이스를 관리합니다. ACID 특성을 보존하는 트랜젝션 수행을 지향하며 이를 위한 Isolation Level이 있습니다. 

NoSQL(Not only SQL)은 스키마를 사전에 정의하지 않아도 데이터를 저장할 수 있습니다. 상대적으로 유연한 저장 형식을 보장하며 그래프기반, column-based, document-oriented, key-value 등 다양한 방식으로 구현되고 이에 맞는 적절한 쿼리 언어가 제공됩니다. ACID를 굳이 완벽하게 보장할 필요가 없다는 관점에서 (데이터 분석에 초점) 시작되었으므로 대부분의 경우 CAP 이론을 따라 구성됩니다. 일관성/가용성/파티션 세가지 중 어떤 두가지 속성을 보장하냐(단순 선택이 아님. 절충안)에 따른 다양한 데이터베이스가 구성됩니다. 

### Q2. NoSQL의 강점과, 약점이 무엇인가요? 
A2. NoSQL은 상대적으로 유연한 스키마로 데이터를 저장할 수 있어 비정형데이터를 다루기에 좋습니다. 또한 horigintal sharding을 통해 확장성을 보장할 수 있습니다.
대부분의 NoSQL은 일관성 기준을 완화하는 대신 가용성을 더 확보하므로, 강력한 데이터 무결성을 보장해야할 경우에는 단점이 됩니다. (그렇다고 모든 NoSQL이 Transaction ACID를 안지키는 것은 아닙니다) 특히 다중 서버 확장을 고려할 때 transaction을 보장하는 것은 어렵습니다. 또한 유연한 스키마가 장점이 될 수 있지만, 저장 스키마가 고정되어있지 않으므로 쿼리할 때 어려움이 있을 수 있습니다.

### Q3. RDB의 어떠한 특징 때문에 NoSQL에 비해 부하가 많이 걸릴 "수" 있을까요? (주의: 무조건 NoSQL이 RDB 보다 빠르다라고 생각하면 큰일 납니다!)
새로운 기능들이 추가되고 변경되어 table schema가 바뀌거나 추가될 경우 로직이 복잡해질 수 있으며 index를 새로 구축하게 되는 등의 작업이 요구될 때 부하가 많이 걸릴 수 있다. 
정말 많은 데이터를 처리하려고 할 때 RDB는 수평확장이 어렵고, 멀티 노드로 구축했을 때 동시성을 신경쓰기 어렵습니다. 

### Q4. NoSQL을 활용한 경험이 있나요? 있다면, 왜 RDB를 선택하지 않고 해당 DB를 선택했는지 설명해 주세요.


### References
https://stackoverflow.com/questions/8729779/why-nosql-is-better-at-scaling-out-than-rdbms
https://dataonair.or.kr/db-tech-reference/d-lounge/technical-data/?mod=document&uid=236124
https://www.mongodb.com/developer/products/mongodb/everything-you-know-is-wrong/