#Search
###_all Field
기본적으로 document를 index하면 _all 필드를 추가하고 document의 모든 내용을 넣는다. 필드를 지정하지 않고 쿼리를 날리면 _all 필드를 탐색하여 결과를 리턴한다.
####_all 사용하지 않기
```
PUT /my_index/_mapping/my_type
{
    "my_type": {
        "_all": { "enabled": false }
    }
}
```
####_all이 특정 필드만 담도록 하기
```
PUT /my_index/my_type/_mapping
{
    "my_type": {
        "include_in_all": false,    // 모든 필드를 포함하지 않도록
        "properties": {
            "title": {
                "type":           "string",
                "include_in_all": true    // _all에 포함되도록
            },
            ...
        }
    }
}

```
#Mapping and Analysis
###Exact Values vs. Full Text
**엘라스틱 서치의 data type은 exact values와 full text 두 가지로 크게 나누어 진다.**    

>####Exact values
>Binary 연산만 가능하다. 즉 같거나 다르거나(크거나 작거나 등)의 연산만 가능
>####Full text
>얼마나 연관이 있는지의 연산이 가능
>Analysing을 통해

_all 필드는 full text에 해당

##Analysis and Analyzers
Analyzer가 analysis를 수행

**Text를 단어로 분리하고 일반화 하여 index를 작성하고 쿼리 입력값을 이 index와 비교하여 relevant를 결정**

Elasticsearch는 다양한 analyzer를 제공하며 여럿을 조합하는 것도 가능

###Full text fields and analysis
Full text fields는 indexing 될 때 analyzed 되어 inverted index를 구성하는데 사용 된다.

**Query string이 full text와 동일한 analysing 과정을 거쳐야 제대로 된 검색 결과를 얻을 수 있다.**

###String field and Full text field
**기본적으로 모든 string field는 full text field로 처리 또한 standard analyzer를 사용**

##Mapping
**Database의 schema에 해당**

Field의 type, analyzer 등이 지정되어 있고, **PUT**을 통해 설정이 가능

###index 속성과 String의 data type
string type field는 index 속성이 존재

- analyzed: full text
- not-analyzed: exact value
- no: don't index, 검색 대상이 될 수 없음

**analyzer도 지정 가능**
```
{
    "tag": {
        "type":     "string",
        "index":    "not_analyzed",
        "analyzer": "english"
    }
}
```

#Modeling
##nested vs. object
field type이 object인 field는 indexing한 object의 각 프로퍼티(field)를 array로 저장 한다.
>name과 age의 field를 갖는 object를 indexing하면 name array, age array 형식으로 indexing 된다.    
>따라서 name과 age의 **관계가 사라지게** 된다.
>즉, 28살의 Amy를 검색할 수 없다.

**관계를 유지하기 위해서는 field type을 nested로 mapping해야 한다.**    
nested object는 독립된 document로 저장된다.

#Aggregation
##High-level concepts
####Buckets
_Collections of documents that meet a criterion_
####Metrics
_Statistics calculated on the documents in a bucket_
