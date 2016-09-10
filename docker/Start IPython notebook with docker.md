# Start IPython notebook with docker

### 下載 docker image

```
docker pull ipython/scipyserver
```

### 啟動

```
docker run -d -p 80:8080 -e "PASSWORD=MakeAPassword" -e "USE_HTTP=1" -v /home/user/analysis_data:/tmp/data ipython/scipyserver
```

開啟瀏覽器，網址輸入`http://{docker_host_IP}`，開始使用 IPython notebook (已安裝 [SciPy](https://www.scipy.org/) 相關套件)。

### 參考資料

- [ipython/scipyserver](https://hub.docker.com/r/ipython/scipyserver/)
