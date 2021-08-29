---
title: 搭建elasticsearch中文搜索以及logstash自动同步mysql
date: 2021-08-07T12:43:23+08:00    # 创建时间
lastmod: 2021-08-07T12:43:23+08:00 # 最后修改时间
draft: false                       # 是否是草稿？
tags: ["elasticsearch", "mysql"]  # 标签
categories: ["技术"]              # 分类
author: "ddb"                  # 作者
---


### 搭建elk和head

``` shell
docker pull libac/elasticsearch-rtf
docker run -d --name es -p 9200:9200 -p 9300:9300 -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -e "discovery.type=single-node" -v /opt/es/es.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /opt/es/data/:/usr/share/elasticsearch/data libac/elasticsearch-rtf

docker pull mobz/elasticsearch-head:5
docker run -d -p 9100:9100 mobz/elasticsearch-head:5
```

/opt/es/es.yml参考
``` yaml
index:                                                                                                                                                                                        
  analysis:                                                                                                                                                                                   
    tokenizer:                                                                                                                                                                                
      my_pinyin:                                                                                                                                                                              
        type: pinyin                                                                                                                                                                          
        first_letter: prefix                                                                                                                                                                  
        padding_char: ''                                                                                                                                                                      
      pinyin_first_letter:                                                                                                                                                                    
        type: pinyin                                                                                                                                                                          
        first_letter: only                                                                                                                                                                    
      mmseg_maxword:                                                                                                                                                                          
        type: mmseg                                                                                                                                                                           
        seg_type: max_word                                                                                                                                                                    
      mmseg_complex:                                                                                                                                                                          
        type: mmseg                                                                                                                                                                           
        seg_type: complex                                                                                                                                                                     
      mmseg_simple:                                                                                                                                                                           
        type: mmseg                                                                                                                                                                           
        seg_type: simple                                                                                                                                                                      
      semicolon_spliter:                                                                                                                                                                      
        type: pattern                                                                                                                                                                         
        pattern: ";"                                                                                                                                                                          
      pct_spliter:                                                                                                                                                                            
        type: pattern                                                                                                                                                                         
        pattern: "[%]+"                                                                                                                                                                       
      ngram_1_to_2:                                                                                                                                                                           
        type: nGram                                                                                                                                                                           
        min_gram: 1                                                                                                                                                                           
        max_gram: 2                                                                                                                                                                           
      ngram_1_to_3:                                                                                                                                                                           
        type: nGram                                                                                                                                                                           
        min_gram: 1                                                                                                                                                                           
        max_gram: 3                                                                                                                                                                           
    filter:                                                                                                                                                                                   
      ngram_min_3:                                                                                                                                                                            
        max_gram: 10                                                                                                                                                                          
        min_gram: 3                                                                                                                                                                           
        type: nGram                                                                                                                                                                           
      ngram_min_2:                                                                                                                                                                            
        max_gram: 10                                                                                                                                                                          
        min_gram: 2                                                                                                                                                                           
        type: nGram                                                                                                                                                                           
      ngram_min_1:                                                                                                                                                                            
        max_gram: 10                                                                                                                                                                          
        min_gram: 1                                                                                                                                                                           
        type: nGram                                                                                                                                                                           
      min2_length:                                                                                                                                                                            
        min: 2                                                                                                                                                                                
        max: 4                                                                                                                                                                                
        type: length                                                                                                                                                                          
      min3_length:                                                                                                                                                                            
        min: 3                                                                                                                                                                                
        max: 4                                                                                                                                                                                
        type: length                                                                                                                                                                          
      pinyin_first_letter:                                                                                                                                                                    
        type: pinyin                                                                                                                                                                          
        first_letter: only                                                                                                                                                                    
    analyzer:                                                                                                                                                                                 
      lowercase_keyword:                                                                                                                                                                      
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        tokenizer: standard                                                                                                                                                                   
      lowercase_keyword_ngram_min_size1:                                                                                                                                                      
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        - stop                                                                                                                                                                                
        - trim                                                                                                                                                                                
        - unique                                                                                                                                                                              
        tokenizer: nGram                                                                                                                                                                      
      lowercase_keyword_ngram_min_size2:                                                                                                                                                      
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        - min2_length                                                                                                                                                                         
        - stop                                                                                                                                                                                
        - trim                                                                                                                                                                                
        - unique                                                                                                                                                                              
        tokenizer: nGram                                                                                                                                                                      
      lowercase_keyword_ngram_min_size3:                                                                                                                                                      
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        - min3_length                                                                                                                                                                         
        - stop                                                                                                                                                                                
        - trim                                                                                                                                                                                
        - unique                                                                                                                                                                              
        tokenizer: ngram_1_to_3                                                                                                                                                               
      lowercase_keyword_ngram:                                                                                                                                                                
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        - stop                                                                                                                                                                                
        - trim                                                                                                                                                                                
        - unique                                                                                                                                                                              
        tokenizer: ngram_1_to_3                                                                                                                                                               
      lowercase_keyword_without_standard:                                                                                                                                                     
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        tokenizer: keyword                                                                                                                                                                    
      lowercase_whitespace:                                                                                                                                                                   
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        tokenizer: whitespace                                                                                                                                                                 
      ik:                                                                                                                                                                                     
        alias:                                                                                                                                                                                
        - ik_analyzer                                                                                                                                                                         
        type: ik                                                                                                                                                                              
      ik_max_word:                                                                                                                                                                            
        type: ik                                                                                                                                                                              
        use_smart: false                                                                                                                                                                      
      ik_smart:                                                                                                                                                                               
        type: ik                                                                                                                                                                              
        use_smart: true                                                                                                                                                                       
      mmseg:                                                                                                                                                                                  
        alias:                                                                                                                                                                                
        - mmseg_analyzer                                                                                                                                                                      
        type: mmseg                                                                                                                                                                           
      mmseg_maxword:                                                                                                                                                                          
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        tokenizer: mmseg_maxword                                                                                                                                                              
      mmseg_complex:                                                                                                                                                                          
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        tokenizer: mmseg_complex                                                                                                                                                              
      mmseg_simple:                                                                                                                                                                           
        type: custom                                                                                                                                                                          
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        tokenizer: mmseg_simple                                                                                                                                                               
      comma_spliter:                                                                                                                                                                          
        type: pattern                                                                                                                                                                         
        pattern: "[,|\\s]+"                                                                                                                                                                   
      pct_spliter:                                                                                                                                                                            
        type: pattern                                                                                                                                                                         
        pattern: "[%]+"                                                                                                                                                                       
      custom_snowball_analyzer:                                                                                                                                                               
        type: snowball                                                                                                                                                                        
        language: English                                                                                                                                                                     
      simple_english_analyzer:                                                                                                                                                                
        type: custom                                                                                                                                                                          
        tokenizer: whitespace                                                                                                                                                                 
        filter:                                                                                                                                                                               
        - standard                                                                                                                                                                            
        - lowercase                                                                                                                                                                           
        - snowball                                                                                                                                                                            
      edge_ngram:                                                                                                                                                                             
        type: custom                                                                                                                                                                          
        tokenizer: edgeNGram                                                                                                                                                                  
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
      pinyin_ngram_analyzer:                                                                                                                                                                  
        type: custom                                                                                                                                                                          
        tokenizer: my_pinyin                                                                                                                                                                  
        filter:                                                                                                                                                                               
        - lowercase                                                                                                                                                                           
        - nGram                                                                                                                                                                               
        - trim                                                                                                                                                                                
        - unique                                                                                                                                                                              
      pinyin_first_letter_analyzer:                                                                                                                                                           
        type: custom                                                                                                                                                                          
        tokenizer: pinyin_first_letter                                                                                                                                                        
        filter:                                                                                                                                                                               
        - standard                                                                                                                                                                            
        - lowercase                                                                                                                                                                           
      pinyin_first_letter_keyword_analyzer:                                                                                                                                                   
        alias:                                                                                                                                                                                
        - pinyin_first_letter_analyzer_keyword                                                                                                                                                
        type: custom                                                                                                                                                                          
        tokenizer: keyword                                                                                                                                                                    
        filter:                                                                                                                                                                               
        - pinyin_first_letter                                                                                                                                                                 
        - lowercase                                                                                                                                                                           
      path_analyzer: #used for tokenize :/something/something/else                                                                                                                            
        type: custom                                                                                                                                                                          
        tokenizer: path_hierarchy                                                                                                                                                             
                                                                                                                                                                                              
index.analysis.analyzer.default.type: mmseg                                                                                                                                                   
#index.analysis.analyzer.default.type: keyword                                                                                                                                                
                                                                                                                                                                                              
# rtf.filter.redis.host: 127.0.0.1                                                                                                                                                            
# rtf.filter.redis.port: 6379
http.cors.enabled: true
http.cors.allow-origin: "*"
```

### logstash搭建

``` shell
nohup /usr/share/logstash/bin/logstash -f /usr/share/logstash/bin/conf-mysql/logstash-mysql-es.conf >/dev/null 2>&1 &
```

/usr/share/logstash/bin/conf-mysql/logstash-mysql-es.conf参考

``` yaml
input{
     jdbc {
         jdbc_driver_library => "/usr/share/logstash/mysql-connector-java-5.1.44-bin.jar"
         jdbc_driver_class => "com.mysql.jdbc.Driver"
         jdbc_connection_string => "jdbc:mysql://********:3306/pan"
         jdbc_user => "root"
         jdbc_password => "********"
         jdbc_paging_enabled => "true"
         jdbc_page_size => "1000"
         jdbc_default_timezone =>"Asia/Shanghai"
         schedule => "*/1 * * * *"
         statement => "select * from aliyun where update_time > :sql_last_value"
         use_column_value => true
         tracking_column => "update_time"
         last_run_metadata_path => "./logstash_jdbc_last_run"
       }
}

output{
      elasticsearch {
         hosts => "127.0.0.1:9200"
         index => "aliyun"
         document_id => "%{id}"
      }
      stdout {
         codec => json_lines
     }
 }
```

### elk创建索引及搜索

``` shell
curl -XPUT http://localhost:9200/_template/rtf -d '{
  "template":   "*", 
  "settings": { "number_of_shards": 1 }, 
  "mappings": {
    "_default_": {
      "_all": { 
        "enabled": true
      },
      "dynamic_templates": [
        {
          "strings": { 
            "match_mapping_type": "string",
            "mapping": {
              "type": "text",
              "analyzer":"ik_max_word",
              "ignore_above": 256,
              "fields": {
                "keyword": {
                  "type":  "keyword"
                }
              }
            }
          }
        }
      ]
    }
  }
}'
```

``` shell
curl -XPOST http://localhost:9200/aliyun/_default_/_mapping -H 'Content-Type:application/json' -d'
{
        "properties": {
            "title": {
                "type": "string",
                "analyzer": "ik_max_word"
            },
            "upload" : {"type" : "string","analyzer": "ik_max_word"},
            "url" : {"type" : "string","analyzer": "ik_max_word"},
            "pan_type" : {"type" : "string","analyzer": "ik_max_word"},
            "type" : {"type" : "string","analyzer": "ik_max_word"},
            "@version" : {"type" : "string","analyzer": "ik_max_word"}
        }
    
}'
```

``` shell
curl -XPOST http://localhost:9200/aliyun/_search -H 'Content-Type:application/json' -d'
{
  "query": {
    "filtered": {
      "query": {
        "match": {
          "title": "周杰伦在东京"
        }
      },
      "filter": {
        "bool": {
          "must": [
            {
              "term": {
                "type": "music"
              }
            }
          ],
          "must_not": [
            {
              "term": {
                "disable": 1
              }
            }
          ]
        }
      }
    }
  },
  "size": 10,
  "from": 1,
  "sort": [
    {
      "_timestamp": {
        "order": "desc"
      }
    }
  ]
}'
```
过滤查询：
https://blog.csdn.net/lebron3v/article/details/84030836

组合查询：
https://www.elastic.co/guide/cn/elasticsearch/guide/current/combining-queries-together.html