# 建置網站

## 網站 設置
### 添加反向代理
*proxy_pass改為你要的Port號*
```
#PROXY-START/

location ^~ /
{
    proxy_pass http://localhost:5001;
    proxy_set_header Host localhost;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
    proxy_http_version 1.1;
    # proxy_hide_header Upgrade;

    add_header X-Cache $upstream_cache_status;

    #Set Nginx Cache

    
    
    if ( $uri ~* "\.(gif|png|jpg|css|js|woff|woff2)$" )
    {
    	expires 1m;
    }
    proxy_ignore_headers Set-Cookie Cache-Control expires;
    proxy_cache cache_one;
    proxy_cache_key $host$uri$is_args$args;
    proxy_cache_valid 200 304 301 302 1m;
}

#PROXY-END/
```

### 在進程守護管理器 
- 添加進層
进程数量: 1
启动优先级:999
dotnet ReactlyMVC.dll –urls http://localhost:5000
port 要跟上面port 一致


### 重啟進程
pkill -f "dotnet WebApplication7.dll --urls http://localhost:5001"

