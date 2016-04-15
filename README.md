# CS5124701
巨量資料分析 Big Data Analytics

---------------------------------------
## Data Coolection and Persistence

## 準備事項

1. 需要的工具：Java 8、ElasticSearch、Logstash
2. 使用的系統為：Ubuntu Linux 14.04
3. Twitter apps 申請開發：
  * － https://apps.twitter.com/
  * a. Consumer Key (API Key)
  * b. Consumer Secret (API Secret)
  * c. Access Token
  * d. Access Token Secret

## 利用 ElasticSearch 以及需要的 .conf 
<p>1. Twitter.conf 程式碼並定義關鍵字為 "is"：</p>

<pre><code>input{
	twitter {
	    consumer_key => "bIUQr43ndaA4owFL8An7qY0If"
	    consumer_secret => "ET2Z8umkoPSdmfpPgSNYhbLGUNPnze0fXBFdw2VD3QaKmJZKgZ"
	    oauth_token => "712241008679858176-alHKsuJEzvDNmzntg4rb0epuZlzPnV7"
	    oauth_token_secret => "CBwMqPITQgcOO65bP9CTMPDzd69nUeEwoVRgsoAepUN91"
	    keywords => ["is"] 
	    languages => ["en"]
	}
}
output{
	elasticsearch{
	    index => "twitter_is"
      }
}
</code></pre>

<p>2. 開啟 ElasticSearch：</p>

<pre><code>$ ./elasticsearch-2.2.1/bin/elasticsearch
</code></pre>

<p>3. Logstash 執行並抓取 1G 資料量：</p>

<pre><code>$ ./logstash -f twitter.conf
</code></pre>

## 轉換 ElasticSearch 的資料成 json 以及需要的 .conf 

<pre><code>input{
	elasticsearch{
	    index => "twitter_is" //和轉出的 index 相同
      	}	

}
output{

	
	file {
	    path => "./test-%{+YYYY-MM-dd}.json" //轉換成以日期為主的 json檔
	}
}
</code></pre>

## ElasticSearch 蒐集到的資料

<pre><code>{
    "_index": "twitter_is",
    "_type": "logs",
    "_id": "AVQKhqvVXq36qS9GE0mP",
    "_version": 1,
    "_score": 1,
    "_source": {
        "@timestamp": "2016-04-12T12:51:31.000Z",
        "message": "Reading in Italian is a pain 😂",
        "user": "Nerelyn18",
        "client": "<a href="http://twitter.com/download/iphone" rel="nofollow">Twitter for iPhone</a>",
        "retweeted": false,
        "source": "http://twitter.com/Nerelyn18/status/719870561875337217",
        "hashtags": [ ],
        "symbols": [ ],
        "user_mentions": [ ],
        "@version": "1"
    }

}
</code></pre>
