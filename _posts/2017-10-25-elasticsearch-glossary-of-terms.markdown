---
layout: post
title:  "Elasticsearch - Glossary of terms"
date:   2017-10-25 21:13:0 +0900
tags: elasticsearch glossary terms
---
# Glossary of terms
* * *

### Full text
Text 또는 full text는 paragraph 와 같이 구조화 되지 않은 일반적인 텍스트이다. 기본적으로, text는 index에 실제로 저장되는 terms 로 분석된다.  - Text field 는 full text로 검색되기 위해 index time (색인 시간) 에 분석되어야 하며, full text 쿼리의 키워드는 색인 시간에 생성된 terms를 제공하기 위해 검색 시간안에 분석되어야 한다.

### Term
Term 은 elasticsearch에서 색인된 value (값) 이다. 예를 들면, foo, FOO, foo 는 같은 값이 아니다. 이는 term queries로 검색될 수 있다.

### Analysis
Analysis 는 full text 를 terms 로 변환하는 과정이다. 사용하는 analyzer (분석기) 에 따라서, {FOO BAR, Foo-Bar, foo,bar} 와 같은 구를 foo와 bar가 term인 모든 결과를 얻는다. 

### Cluster
Cluster 는 하나 이상의 노드 (서버)가 모인 것이며, 이를 통해 전체 데이터를 저장하고 모든 노드를 포괄하는 통합 색인화 및 검색 기능을 제공한다.

### Document
Document는 elasticsearch 에 저장되는 JSON document를 의미한다 (관계형 데이터베이스에서 table의 row 와 같다.). 각 document 는 type 과 id 를 갖고 index에 저장된다.

### Id
Document 의 id 는 document 유일하게 식별하는 식별자이다. 만약 id 없이 document 를 생성했다면, 자동으로 생성된다.

### Type (deprecated 로 버전업하면서 제거되고 있다.)
Type 은 document 의 형식을 표현하기 위해 사용된다. 예를 들면, email, user 또는 트윗가 같다. 

### Field
Document 는 field들 또는 key-value 쌍의 리스트 을 가진다.  값을 우리가 알고있는 상수 값이 될 수 있고 (string, integer, date), 또는 array와 object 도 될 수 있다. Field 는 관계형 데이터베이스에서 table 의 column 과 같다.  각 field 에 대한 mapping 에는 해당 field에 저장할 수 있는 데이터 type (string, integer, object) 을 나타내는 field type을 가진다. 

### Mapping
Mapping 은 관계형 데이터베이스에서 schema definition 과 같다.  각 index 는 type을 정의하는 mapping을 가진다 (plus a number of index-wide settings). Mapping 은 명시적으로 정의할 수 있고, document 가 색인 (indexed) 될 때 자동적으로 생성될 수도 있다.

### Index
Index 는 관계형 데이터베이스의 테이블과 같다. It has a mapping which contains a type, which contains the fields in the index. Index 는 하나 이상의 primary shards 에 매핑 되고 0개 이상의 replica shards 를 가지는 logical namespace (논리적인 공간) 이다.

### Node	
Node 는 cluster에 속한 elasticsearch 의 실행중인 인스턴스이다. 여러 노드들은 테스트 목적으로 하나의 서버에서 실행될 수 있지만, 보통 한 서버당 하나의 노드를 실행 시킨다. Node 시작 시에 (at startup), node 는 동일한 이름을 가진 cluster 를 찾기 위해 unicast 를 사용하고, 그 cluster 에 접속을 시도한다.

### Shard
Shard 는 하나의 lucene instance 이다. Shard 는 elasticsearch 에서 알아서 관리해 주는 low-level "worker" 단위 이다. 

### Primary shard
각 document 는 하나의 primary shard 를 저장한다. Document 를 색인할 때, primary shard 가 먼전 색인되고, 나머지 모든 replicas  가 색인된다. 기본적으로, 색인은 5개의 primary shards 를 가진다. 색인을 처리 할 수 있는 document 수를 줄이기 위해 primary shard 를 더 적게 또는 더 많이 specify (지정) 할 수 있다. 한번 index 가 생성되면 index 의 primary shards 수를 변경할 수 없다.

### Replica shard
Replica 는 primary shard 의 복사본이고, 아래 두가지 목적을 가진다:
- Increase failover: a replica shard can be promoted to a primary shard if the primary fails
- Increase performance: get and search requests can be handled by primary or replica shards. By default, each primary shard has one replica, but the number of replicas can be changed dynamically on an existing index. A replica shard will never be started on the same node as its primary shard.

### Routing
Document 를 색인할 때, 이는 primary shard 에 저장된다. 이 shard 에 routing value 를 hashing 함으로써 선택된다. 기본적으로 routing value 는 document id로부터 얻는다 (만약 document 가 특정 부모 document 가 있다면, 부모의 id로 부터). This value can overridden by specifying a routing value at index time, or a routing field in the mapping.

### Source field
기본적으로 JSON 색인한 document 는 _source field 에 저장될 수 있고, all get 과 search requests 에 의해 반환될 수 있다. 이는 검색 결과로부터 original object 를 직접적으로 접근할 수 있게 해준다.

## Reference
- [Elastic docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/glossary.html#glossary-text)
