### some sample queries
<pre>
GET mytest_index/_search 
{
  "query": {
     "wildcard" : { "search_term" : "rob*n" }
  }
}

GET mytest_index/_search 
{ "size":100,
  "query": {
     "fuzzy" : { "search_term" : "hsppy" }
  }
}

GET mytest_index/_search
{ "query": {
    "regexp" : { "search_term" : "ro.*?y" }
  }
}

GET mytest_index/_search
{ "query": {
    "regexp" : { "search_term" : "o{3}" }
  }
}

GET mytest_index/_search
{ "query": {
    "prefix" : { "search_term" : "rob" }
  }
}

### unique count
GET /mytest_index/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "agg_date": {
              "gte": "2017-06-01",
              "lte": "2017-07-02"
            }
          }
        },
        {
          "bool": {
            "should": [
              {
                "term": {
                  "industry": 80
                }
              },
              {
                "query_string": {
                  "query": "(conversion:ABCD) OR (conversion:XYZ)"
                }
              },
              {
                "query_string": {
                  "query": "(channel:www.amazon.com) OR (channel:www.google.com)"
                }
              },
              {
                "query_string": {
                  "query": """(search_term:"batman and robin" or search_term:"why were my amazon orders cancelled") OR (search_term:"pizza hut of tullahoma")"""
                }
              }
            ]
          }
        }
      ]
    }
  },
  "aggs": {
    "unique_cnt": {
      "cardinality": {
        "field": "my-id",
        "precision_threshold": 4000
      }
    }
  }
}

### REINDEX

POST _reindex?wait_for_completion=false
{
  "source": {
    "index": "mytest_index",
    
    "type": "behavior",
    "query": {
      "bool": {
        "must": [
          {
            "range": {
              "agg_date": {
                "gte": "2017-06-01",
                "lte": "2017-07-02"
              }
            }
          },
          {
            "bool": {
              "should": [
                {
                  "term": {
                    "industry": 1000
                  }
                },
                {
                  "query_string": {
                    "query": "(conversion:ABCD) OR (conversion:XYZ)"
                  }
                },
                {
                  "query_string": {
                    "query": "(channel:www.amazon.com) OR (channel:www.google.com)"
                  }
                },
                {
                  "query_string": {
                    "query": """(search_term:"batman and robin" or search_term:"why were my amazon orders cancelled") OR (search_term:"pizza hut of tullahoma")"""
                  }
                }
              ]
            }
          }
        ]
      }
    }
  },
  "dest": {
    "index": "copy_segmentation"
  }
}

GET mytest_index/_count

PUT /mytest_index/_settings
{
    "index" : {
        "refresh_interval" : "-1"
    }
}

GET mytest_index/_search?size=1000

GET mytest_index/_search
{
  "query":{
    "query_string": {
      "query": "(conversion:ABC)"
    }
  }
}

PUT /mytest_index/_settings
{
    "index" : {
        "max_result_window" : 200000
    }
}

POST _tasks/node_id:task_id/_cancel


GET _tasks?

GET _cat/pending_tasks

### 1. total document count
GET mytest_index/_count

### 2. agg example
GET /mytest_index/_search
{
   "size":0,
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "agg_date": {
              "gte": "2017-06-30",
              "lte": "2017-07-09"
            }
          }
        },
        {
          "bool": {
            "should": [
              {
                "match_all": {
                }
              }
            ]
          }
        }
      ]
    }
  },
  "aggs": {
    "id": {
      "terms": {
        "size":1000,
        "field": "industry"
      }
    }
  }
}

### 3 unique count id example
GET /mytest_index/_search?request_cache=false
{
  "size":0,
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "agg_date": {
              "gte": "2017-06-01",
              "lte": "2017-07-09"
            }
          }
        },
        {
          "bool": {
            "should": [
              {
                "term": {
                  "industry": 80
                }
              },
              {
                "query_string": {
                  "query": "(conversion:ABCD) OR (conversion:XYZ)"
                }
              },
              {
                "query_string": {
                  "query": "(channel:www.amazon.com) OR (channel:www.google.com)"
                }
              },
              {
                "query_string": {
                  "query": """(search_term:"batman and robin" or search_term:"why were my amazon orders cancelled") OR (search_term:"pizza hut of tullahoma")"""
                }
              }
            ]
          }
        }
      ]
    }
  },
  "aggs": {
    "unique_cnt": {
      "cardinality": {
        "field": "my-id",
        "precision_threshold": 4000
      }
    }
  }
}

### 4. some query examples



GET copy_segmentation/_count


POST _refresh

### 5. copy query results to another index
POST _reindex?wait_for_completion=false
{
  "source": {
    "index": "mytest_index",
    
    "type": "behavior",
    "query": {
      "bool": {
        "must": [
          {
            "range": {
              "agg_date": {
                "gte": "2017-06-30",
                "lte": "2017-07-02"
              }
            }
          },
          {
            "bool": {
              "should": [
                {
                  "term": {
                    "industry": 1000
                  }
                },
                {
                  "query_string": {
                    "query": "(conversion:ABCD) OR (conversion:XYZ)"
                  }
                },
                {
                  "query_string": {
                    "query": "(channel:www.amazon.com) OR (channel:www.google.com)"
                  }
                },
                {
                  "query_string": {
                    "query": """(search_term:"batman and robin" or search_term:"why were my amazon orders cancelled") OR (search_term:"pizza hut of tullahoma")"""
                  }
                }
              ]
            }
          }
        ]
      }
    }
  },
  "dest": {
    "index": "copy_segmentation"
  }
}

GET mytest_index/_search
{
  "query": {
    "query_string": {
      "query": "(conversion:ABC)"
    }
  }
}


GET /mytest_index/_search
{
   "size":0,
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "agg_date": {
              "gte": "2017-06-01",
              "lte": "2017-07-02"
            }
          }
        },
        {
          "bool": {
            "should": [
              {
                "query_string": {
                  "query": """(industry:120)"""
                }
              }
            ]
          }
        }
      ]
    }
  },
  "aggs": {
    "id": {
      "terms": {
        "size":1000,
        "field": "my-id"
      }
    }
  }
}






GET new_test/_search 
{
  "query":{
    
  "query_string": {
            "query": "(channel:\"www.linkedin.com\") OR (channel:\"www.petfinder.com\")"
        }
  }
}


GET new_test/_search


PUT other_test
{
  "settings": {
    "max_result_window": 2000,
    "refresh_interval":"2s",
    "index" : {
            "number_of_shards" : 5, 
            "number_of_replicas" : 0
    }
  },
  "mappings": {
    "behavior": { 
      "properties": { 
        "my-id":{"type": "keyword"},
        "platform":    { "type": "keyword"},
        "channel":    { "type": "text","analyzer": "standard"}, 
        "conversion":     { "type": "text","analyzer": "standard"}, 
        "industry":      { "type": "integer"}, 
        "search_term":    { "type": "text", "analyzer": "standard"}, 
        "agg_date":  {
          "type":   "date", 
          "format": "strict_date_optional_time||epoch_millis"
        }
      }
    }
  }
}


#########
END



PUT /mytest_index/_settings
{
    "index" : {
        "max_result_window" : 2000
    }
}


GET /mytest_index/_search
{
   "size":0,
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "agg_date": {
              "gte": "2017-06-01",
              "lte": "2017-07-02"
            }
          }
        },
        {
          "bool": {
            "should": [
              {
                "match_all": {
                }
              }
            ]
          }
        }
      ]
    }
  },
  "aggs": {
    "id": {
      "terms": {
        "size":1000,
        "field": "industry"
      }
    }
  }
}



</pre>
