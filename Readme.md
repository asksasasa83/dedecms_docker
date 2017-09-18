## 构建 Docker 镜像

以 UTF8 FULL 5.7 为例

```bash
docker build -t chengxulvtu/dedecms:utf8_full_5.7
```

## 上传到 Docker Hub

### 登录

```bash
docker login
```

### Push

```bash
docker push chengxulvtu/dedecms:utf8_full_5.7
```