---
title: MongoDB Tutorial-1
author: kiwing
date: 2024-02-02 22:48:51 +0800
categories: [MLOps, database]
tags: [DB]
pin: false
---

# Mongodb CRUD
- 오픈소스 문서 지향(document-oriented) NoSQL 데이터베이스 관리 시스템
- JSON과 유사한 형식인 BSON(Binary JSON)을 사용하여 데이터를 저장

## NoSQL (Not Only SQL)

- RDBMS의 전통적인 테이블 기반 구조 대신 비관계형 데이터를 저장하고 검색하기 위해 설계된 DB 범주
    - RDBMS : MySQL, Oracle, PostgreSQL ...
    - NoSQL : MongoDB와 Redis, HBase  ...
- 대량의 분산된 데이터를 저장, 검색, 관리하기 위해 설계된 비관계형 DB 시스템
- 빅 데이터와 실시간 웹 애플리케이션과 같은 분야에서 확장성과 성능 최적화
- Key-Value Stores, Document-Oriented Databases, Column-Family Stores, Graph Databases

<br>

| 특징                | RDBMS                                       | NoSQL                                       |
|---------------------|---------------------------------------------|---------------------------------------------|
| 데이터 모델         | 테이블 기반                                 | 문서, 키-값, 그래프, 컬럼 패밀리 등 다양함  |
| 스키마              | 엄격한 스키마 구조                         | 스키마리스 또는 유연한 스키마              |
| 확장성              | 수직 확장 (서버 업그레이드)                  | 수평 확장 (서버 추가)                       |
| 트랜잭션            | ACID(원자성, 일관성, 고립성, 지속성) 속성 지원 | 일부 지원, ACID 보다는 BASE(기본적으로 사용 가능, 부드러운 상태, 최종 일관성) 모델을 따름 |
| 쿼리 언어           | SQL (구조화된 쿼리 언어)                     | 대부분의 자체 쿼리 언어 사용         |
| 데이터 처리         | 복잡한 트랜잭션과 조인 연산에 유리           | 대규모 분산 데이터 처리에 유리              |
| 사용 사례           | 복잡한 조회가 필요한 애플리케이션            | 빅데이터, 실시간 웹 애플리케이션, 유연성이 필요한 경우 |
| 일관성 유형         | 강력한 일관성                               | 최종 일관성, 일관성 레벨 선택 가능           |
| 데이터 중복         | 데이터 중복 최소화를 위한 정규화            | 성능 향상을 위해 데이터 중복 허용           |

<br>

## [주요 특징]

- 문서 지향 스토리지
    - 데이터를 JSON 유사 형태의 문서(document)로 저장 
    - 이 문서는 스키마가 없기 때문에 서로 다른 구조의 데이터를 같은 컬렉션 내에 저장 O
- 스키마리스:
    - MongoDB는 스키마가 없는 데이터베이스
    - DB 저장되는 문서들이 동일한 구조를 가질 필요 없음
- 확장성: 
    - 데이터가 증가함에 따라 더 많은 서버를 DB 시스템에 추가해 처리 능력을 쉽게 확장 O
- 복제 및 고가용성:
    - MongoDB는 데이터 replication를 지원해 데이터의 안전성과 고가용성을 보장
    - 복제를 통해 데이터의 사본을 여러 서버에 분산시킬 수 있음
- 집계(aggregation) 프레임워크:
    - MongoDB는 집계연산을 위한 강력한 프레임워크를 제공
    - 데이터를 처리하고 분석하기 위한 복잡한 질의(query)를 수행할 수 있습니다.

<br>

### Mongo 구조

<img src="https://github.com/Ki-Wing/Ki-Wing.github.io/assets/92567807/7238b7fd-4232-4974-8dcd-703af65623f6" alt="1" width="90%"/>

<span style="color:#808080"> https://kciter.so/posts/about-mongodb </span>

<br>

- Database
    - 파일의 물리적 컨테이너
    - 하나의 MongoDB 서버는 여러 DB를 호스팅할 수 있음 (각 DB는 분리된 파일 세트로 디스크에 저장)
- Collections
    - MongoDB의 문서가 저장되는 곳 (문서들 그룹화하는 메커니즘)
    - 스키마 고정 X. 같은 컬렉션 내의 문서는 서로 다른 구조를 가질 수 있음
- Documents
    - MongoDB 데이터를 저장하는 기본 단위
    - BSON 형식으로 저장
    - key-value 쌍의 집합으로 구성되며 각 문서는 컬렉션 내에서 고유한 _id 필드를 통해 식별
- Fields
    - 문서 내의 각 항목
    - key-value 쌍으로 구성되며 데이터의 실제 값을 포함
    - 다른 문서를 참조하는 필드 또는 내장 문서를 포함 O

<br>

``` JSON
{
  "_id": ObjectId("507f191e810c19729de860ea"),
  "title": "MongoDB Guide",
  "author": "Ki WING",
  "pages": 100,
  "tags": ["MongoDB", "Database", "NoSQL"],
  "details": {
    "isbn": "123-456-789",
    "language": "English"
  }
}

```
문서 구성 예시

- title, author, pages, tags, details와 같은 여러 필드를 포함
- details 필드는 내장 문서 가짐
- 이 문서는 어떤 컬렉션의 일부일 수 있으며, 해당 컬렉션은 더 큰 데이터베이스의 구성 요소임

ObjectID

- 12바이트(24자리의 16진수 문자열) 로 구성
1. timestamp 
    - 4바이트
    - 문서가 생성된 시간의 UNIX 타임스탬프 (10진수로 변환하면 특정한 시간값을 얻을 수 있음)
    - 507f191e : 1350937982초
2. random value
    - 5바이트
    - 서버별로 고유한 값을 생성하기 위해 사용
        - 2-1. Machine Identifier : 3바이트 ObjectId를 생성한 시스템의 고유 식별자. 시스템의 MAC 주소에서 생성  
        - 2-2. Process ID : 2바이트 ObjectId를 생성한 프로세스의 ID. 동일한 시스템 내에서 실행되는 서로 다른 프로세스들이 서로 다른 ObjectId를 생성하도록 보장 
3. (incrementing) Counter
    - 3바이트
    - 순차적으로 증가하는 카운터 값
    - 카운터는 일반적으로 랜덤하게 시작되며 각각의 ObjectId가 생성될 때마다 1씩 증가

<br>

## [Monfon 분산 sys]
- 데이터를 여러 서버에 걸쳐 저장하고 관리하는 구조
- MongoDB의 분산 시스템은 주로 레플리카 세트(Replica Sets)와 샤딩(Sharding)을 통해 구현

### Replica Sets
- 데이터의 복사본을 여러 서버에 분산시켜 가용성과 데이터 보호를 제공

<br>

<img src="https://github.com/Ki-Wing/Ki-Wing.github.io/assets/92567807/52a59776-a604-4dd5-aa14-9dca7f3493eb" alt="2" width="80%"/>

<span style="color:#808080">https://www.ionos.co.uk/digitalguide/websites/web-development/mongodb-replica-set/</span>

<br>

- [이건 참고만..]
- P-S-S (Primary-Secondary-Secondary)
    - Primary Node: 
        - 클라이언트의 모든 write operations 처리
            - 변경 사항은 Secondary로 복제
        - Primary가 실패하면 Secondary 노드 중 하나가 자동으로 Primary 역할을 수행
    - Secondary Node: 
        - Primary 노드의 데이터 복사본을 유지
        - 세컨더리 노드는 Primary로부터 데이터를 복제하여 일관된 데이터 상태를 유지하며, Primary에 장애가 발생했을 때 고가용성을 보장합니다
        - read operations을 처리할 수도 있음

<br>

- P-S-A (Primary-Secondary-Arbiter)
    - Arbiter (A):
        - 데이터를 저장하거나 복제하지 않는 특수한 노드 
        - Primary 노드에 장애가 발생했을 때 새로운 Primary 노드의 선출 도움
        - 레플리카 세트의 쿼럼(과반수)을 형성하는 데 필요한 투표권을 가지고 있는데 데이터의 실제 복제에는 참여 X (스토리지 리소스가 제한적인 환경에서 유용)

<details>

### Replica Sets 특징

- 자동 장애 복구: Primary가 노드에 장애가 발생하면, Secondary 노드 중 하나가 자동으로 Primary가 역할을 인수 (이 과정은 일반적으로 몇 초 내에 완료)
- 데이터 보호: 데이터의 여러 복사본을 유지함으로써 하드웨어 실패, 네트워크 분할, 서비스 중단 등으로부터 데이터를 보호할 수 있음
- 읽기 확장성: 읽기 쿼리를 Secondary 노드로 분산시킴으로써, 읽기 처리량을 증가시킬 수 있음. Primary 노드 부하를 줄이고 전반적인 시스템 성능을 향상시킬 수 있음

<br>

### Sharding

- 데이터를 여러 서버에 분할해서 저장함으로써 DB 수평적 확장 가능
- 큰 규모의 데이터 세트를 다루거나 높은 처리량이 요구될 때 사용

<br>

- Shard: 
    - 데이터의 부분 집합을 저장하는 서버/서버 클러스터
    - 독립적으로 데이터를 저장하고 처리하여 전체 시스템 부하 분산
- Shard Key: 
    - 문서를 샤딩할 때 사용하는 필드(또는 필드의 조합)
    - 문서를 샤드에 분배하는 데 사용하고 쿼리 성능 최적화에 중요한 역할을 함
- Mongos: 
    - 쿼리 라우터 역할을 수행하는 프로세스
    - 클라이언트의 요청을 적절한 샤드로 라우팅하고 여러 샤드에서의 쿼리 결과를 수집하여 클라이언트에게 반환
- Config Servers: 
    - 샤딩 환경의 메타데이터를 저장하는 서버 
    - 샤드의 구성, 샤드 키의 분포, 샤딩된 데이터베이스 및 컬렉션의 구조 등을 관리

[nodejs]: https://nodejs.org/
[starter]: https://github.com/cotes2020/chirpy-starter
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow
[latest-tag]: https://github.com/cotes2020/jekyll-theme-chirpy/tags
