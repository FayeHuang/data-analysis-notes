# Start Zeppelin with docker

### 下載 docker image

```
docker pull dylanmei/zeppelin
```

### 啟動

```
docker run -d -p 80:8080 -v /home/user/analysis_data:/tmp/data dylanmei/zeppelin
```

開啟瀏覽器，網址輸入`http://{docker_host_IP}`，開始使用 Zeppelin。

### 參考資料

- [dylanmei/zeppelin](https://hub.docker.com/r/dylanmei/zeppelin/)