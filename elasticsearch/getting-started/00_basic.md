# You Know, for Search
Elasticsearch is a **distributed**, scalable, **real-time** search and analytics engine.    
Apache Lucene을 기반으로 함

- A distributed real-time document store where **_every field_** is indexed and searchable
- A distributed search engine with real-time analytics
- Capable of scaling to hundreds of servers and petabytes of structured and unstructured data

## Talking to Elasticsearch
### Java API
9300 port 사용    
#### Node client
non data node롤 local cluster에 참여. 특정 데이터가 클러스터 상 어떤 노드에 있는 지를 악고 직접 해당 노드로 request를 보냄    
#### Transport client
remote cluster로 request를 보냄. cluster에 참여하지 않음.

### RESTful API with JSON over HTTP
9200 port 사용    
```
curl -X<VERB> '<PROTOCOL>://<HOST>:<PORT>/<PATH>?<QUERY_STRING>' -d '<BODY>'
```
- `VERB`: GET, POST, PUT, HEAD, DELETE
- `QUERY_STRING`: `?pretty` JOSN response를 보기 좋게 프린트

## Document Oriented
Elasticsearch is **document oriented**.    
관계형 데이터베이스의 행과 열에 객체를 저장하는 것이 아니라, 객체 전체를 documents로 저장하고 검색을 위해 document의 내용을 indexing.    

Documents의 serialization format으로 JSON을 사용

## Indexing
### RDB vs. Elasticsearch
Relational DB		-> Databases		-> Tables		-> Rows			-> Columns    
Elasticsearch		-> Indices				-> Types		-> Documents	-> Fields

> #### The meanings of index
> _Index(noun)_    
> 관계형 데이터베이스의 데이터베이스에 해당. documents의 저장소
>
> _index(verb)_
> Index에 document를 저장하는 것
>
> _Inverted index_
> 관계형 DB에서 데이터 검색을 위해 특정 컬럼에 index를 추가하는 것에 해당. 기본적으로 documents의 모든 fields는 inverted index를 가지며 검색대상이 될 수 있음. Inverted index를 갖지 않은 fields는 검색대상이 되지 못함.    

## Search with Query DSL
```
GET /megacorp/employee/_search
{
	"query": {
		"match" : {
			"last_name": "Smith"
		}
	}
}
```

## More-Complicated Searches
```
GET /megacorp/employee/_search
{
	"query": {
		"filtered": {
			"filter": {
				"range": {
					"age": { "gt": 30 }
				}
			},
			"query": {
				"match": {
					"last_name": "Smith"
				}
			}
		}
	}
}
```

## Score concept
Elasticsearch의 검색 결과는 `"_score"`필드를 가지며, 이는 query와의 relevance 점수

## Aggregations
Data 분석에 사용
```
GET /megacorp/employee/_search
{
  "query": {
    "match": {
      "last_name": "smith"
    }
  },
  "aggs": {
    "all_interests": {
      "terms": {
        "field": "interests"
      }
    }
  }
}
```
