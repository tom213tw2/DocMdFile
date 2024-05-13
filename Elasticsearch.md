# Elasticsearch

## Info

| Account         | Password        |
|-----------------|-----------------|
| elastic |FLaO29FLV8|


## ToCheck IS Service is noraml

```
curl -H "Authorization: ApiKey $(cat /var/pwd/Elasticsearchkey.txt)" -X GET "http://localhost:9200"
```




## To Create apikey to file(monthly)

### Need to Install jq
```
sudo apt-get update
sudo apt-get install jq
ㄋ
```



```
curl -X POST "http://localhost:9200/_security/api_key" -H "Content-Type: application/json" -d'
{
  "name": "api-Key",
  "expiration": "30d"
}' -u "elastic:FLaO29FLV8" | jq -r '.encoded' > /var/pwd/Elasticsearchkey.txt
```