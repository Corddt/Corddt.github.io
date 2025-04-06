部署细节：

![Image](https://github.com/user-attachments/assets/26e77a6c-f395-4c82-9d89-8e9410a61f99)

打开命令行，利用docker远程部署milvus:
```
docker compose up -d
```
部署完成后，修改.env的相关配置。
```
cat .env
nano .env
```
Ctrl+O是保存，Ctrl+X是退出。
后面开始进行向量化。
```
npm run generate-embeddings
```

![Image](https://github.com/user-attachments/assets/13fe650f-458d-41c6-bf20-041813427d36)

![Image](https://github.com/user-attachments/assets/8846c973-0bad-4294-8817-7bd8a526c7e7)

向量化完成后，启动。

![Image](https://github.com/user-attachments/assets/41b705a2-db6f-419b-87c3-de62efc3ebf2)

暴露端口：

![Image](https://github.com/user-attachments/assets/81f35d31-1870-4666-8f6f-93097363c377)

在本地测试：

![Image](https://github.com/user-attachments/assets/1c2961a9-7147-47b0-8a61-0e5dcee25076)
可以看到，成功收到响应。
服务器端也可以看到类似响应。

![Image](https://github.com/user-attachments/assets/2e18debd-cf55-4430-8e51-125b7315a688)