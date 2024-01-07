# Dockerfile的构建和运行指南
将项目拷贝到需要执行的机器上，打开终端，并导航到包含 Dockerfile 的目录。
使用以下命令来构建 Docker 镜像。

`docker build -t selfie-verification .`
在上述命令中，-t 参数用于指定镜像的标签（名称和版本）。. 表示 Dockerfile 所在的当前目录。确保在命令的末尾有一个 .，以指定上下文路径。

Docker 将开始根据 Dockerfile 中的指令构建镜像。它将执行每个指令，并根据需要下载和安装所需的依赖项。构建过程可能需要一些时间，具体取决于您的网络速度和镜像的复杂性。

构建完成后，可以使用以下命令来查看已构建的镜像列表：

docker images
确保您可以看到刚刚构建的镜像及其相应的标签。

使用以下命令来运行镜像。
`docker run --rm -p 8080:8080 selfie-verification`

Docker 将创建一个容器并在其中运行镜像。根据示例 Dockerfile，容器将执行 CMD 指令中定义的命令。
如果您的应用程序需要与主机或其他容器进行通信，您可能需要使用适当的标志来配置容器的网络和端口映射。


# 高级设计概述

### 目录结构
```text
|-- common #系统所需的公共类
|-- config #系统配置文件夹
|----|--SecurityConfig.java
|-- controller #对外暴露的端点
|-- domian #各种类型的实体类
|-- exception #项目自定义的异常
|-- filter
|----|--JwtRequestFilter.java #JWT请求认证过滤器
|-- handler #一些特殊情况的处理
|-- mapper #用于数据库实体对象的映射
|-- service #提供各类服务
|----|--impl
|----|----|--MediaVerificationsServiceImpl
|----|----|--TenantsServiceImpl
|----|----|--UserDetailServiceImpl
|----|----|--UsersServiceImpl
|----|--MediaVerificationsService
|----|--TenantsService
|----|--UsersService
|-- util #项目中用到的工具类
|----|-- AuthExceptionUtil.java
|----|-- JwtTokenUtil.java
|-- SelfieVerificationApplication.java #项目的启动类
```

# API规范文档
## Selfie verification server.

#### Login

## Selfie verification server. -> Login

#### 接口URL

> /login

#### 请求方式

> POST
#### Content-Type
> json
#### 请求Body参数

```javascript
{
    "username": "admin",
    "password": "1234567",
    "tenantId": 1
}
```

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | ---- | ---- | ---- |
| username | admin | String | 是 | Username. |
| password | 1234567 | String | 是 | Password. |
| tenantId | 1 | Integer | 是 | tenantId. |


#### 成功响应示例

```javascript

{

  "code": 0
  "msg": "请求成功",
  "traceId": "27ff48aa-ef06-4ad2-be14-cedf95af407b",
  "data": {
  "token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImMiOiJhZG1pbiIsInRlbmFudElkIjoxLCJleHAiOjE3MDE1OTc0MDksImlhdCI6MTcwMTU3OTQwOX0.nz9kzObi0rjkefeOqO_J-BLbuYuOb3kU-2IiOGW4JjnXGKEKByAKDocXT5NeZkgolPcxNK3-O6sAciAao18JfQ"

  }

}

```

  

| 参数名 | 示例值 | 参数类型 | 参数描述 |
| --- | --- | ---- | ---- |
| code | 0 | Integer | Returning 0 indicates a normal request, while any other situation is considered abnormal. |
| msg | 请求成功 | String | Returning  error message. |
| traceId | 27ff48aa-ef06-4ad2-be14-cedf95af407b | String |  |
| data | - | Object | Matching structure: true indicates a successful match. |
| data.token | eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImMiOiJhZG1pbiIsInRlbmFudElkIjoxLCJleHAiOjE3MDE1OTc0MDksImlhdCI6MTcwMTU3OTQwOX0.nz9kzObi0rjkefeOqO_J-BLbuYuOb3kU-2IiOGW4JjnXGKEKByAKDocXT5NeZkgolPcxNK3-O6sAciAao18JfQ | String | Authentication token. |

  

#### 错误响应示例

```javascript
{
  "code": 40100,
  "msg": "Incorrect username or password.",
  "traceId": "33b25b1d-a6cb-44a4-8b9b-828c5fb24dc9",
  "data": null
}
```

  

| 参数名 | 示例值 | 参数类型 | 参数描述 |
| --- | --- | ---- | ---- |
| code | 40100 | Integer | Returning 0 indicates a normal request, while any other situation is considered abnormal. |
| msg | Incorrect username or password. | String | Returning  error message. |
| traceId | 33b25b1d-a6cb-44a4-8b9b-828c5fb24dc9 | String |  |
| data | null | Null |  |

  

## Selfie verification server. -> Challenge

```text

The api should return the result from the “verification server”

```

#### 接口URL

> /api/v1/challenge

  

#### 请求方式

> POST

#### Content-Type
> json

#### 请求Header参数

  

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | ---- | ---- | ---- |
| Authorization | Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImMiOiJhZG1pbiIsInRlbmFudElkIjoxLCJleHAiOjE3MDE1OTczNjAsImlhdCI6MTcwMTU3OTM2MH0.xCnCelm1mXqodbwIUXuo7eAFOlFDSy7Lo5s0GOMJwldxUgGipGx1WVFc5B6sIy1aqDYbKa0sMy10j9NFZ0KuHQ | String | 是 | - |

  

#### 请求Body参数

```javascript
{
    "videoUrl": "https://img.jbzj.com/file_images/article/202106/2021062116032963.png",
    "imageUrl": "https://img.jbzj.com/file_images/article/202106/2021062116032963.png"
}

```

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | ---- | ---- | ---- |
| videoUrl | https://img.jbzj.com/file_images/article/202106/2021062116032963.png | String | 是 | Video url. |
| imageUrl | https://img.jbzj.com/file_images/article/202106/2021062116032963.png | String | 是 | Image url. |
  

#### 成功响应示例

  

```javascript
{
  "code": 0,
  "msg": "successful",
  "traceId": "f1b6e824-1f57-4964-a022-27c9ecd2e672",
  "data": true
}

```

  

| 参数名 | 示例值 | 参数类型 | 参数描述 |
| --- | --- | ---- | ---- |
| code | 0 | Integer | Returning 0 indicates a normal request, while any other situation is considered abnormal. |
| msg | 请求成功 | String | Returning  error message. |
| traceId | f1b6e824-1f57-4964-a022-27c9ecd2e672 | String |  |
| data | true | Boolean | Matching structure: true indicates a successful match. |


#### 错误响应示例

```javascript
{
  "code": 50400,
  "msg": "video url cannot be null,image url cannot be null",
  "traceId": "284e88df-6087-4e7b-bb01-6a18e03fea9b",
  "data": null
}

```

| 参数名 | 示例值 | 参数类型 | 参数描述 |
| --- | --- | ---- | ---- |
| code | 50400 | Integer | Returning 0 indicates a normal request, while any other situation is considered abnormal. |
| msg | video url cannot be null,image url cannot be null | String | Returning  error message. |
| traceId | 284e88df-6087-4e7b-bb01-6a18e03fea9b | String |  |
| data | null | Null |  |

  

## Selfie verification server. -> Query challenge history

```text
Returns the result(s) of the recent challenge APIs for the user.
```

#### 接口状态

#### 接口URL

> /api/v1/challenge/history

#### 请求方式
> POST

#### Content-Type
> json

#### 请求Header参数

| 参数名 | 示例值 | 参数类型 | 是否必填 | 参数描述 |
| --- | --- | ---- | ---- | ---- |
| Authorization | Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImMiOiJhZG1pbiIsInRlbmFudElkIjoxLCJleHAiOjE3MDE1OTczNjAsImlhdCI6MTcwMTU3OTM2MH0.xCnCelm1mXqodbwIUXuo7eAFOlFDSy7Lo5s0GOMJwldxUgGipGx1WVFc5B6sIy1aqDYbKa0sMy10j9NFZ0KuHQ | String | 是 | - |


#### 请求Body参数

```javascript
{}
```

#### 成功响应示例

```javascript

{
  "code": 0,
  "msg": "请求成功",
  "traceId": "6d0e9bff-c936-4a2e-98df-e80627c41bf8",
  "data": {
    "videoUrl": "https://img.jbzj.com/file_images/article/202106/2021062116032963.png",
    "imageUrl": "https://img.jbzj.com/file_images/article/202106/2021062116032963.png",
    "videoSize": 105196,
    "imageSize": 105196,
    "verificationStatus": "verified",
    "verificationMessage": null,
    "timeSpent": 5147,
    "createdAt": "2023-12-03 01:16:14",
    "updatedAt": "2023-12-03 01:16:14"
  }

}

```


| 参数名 | 示例值 | 参数类型 | 参数描述 |
| --- | --- | ---- | ---- |
| code | 0 | Integer | Returning 0 indicates a normal request, while any other situation is considered abnormal. |
| msg | 请求成功 | String | Returning  error message. |
| traceId | 6d0e9bff-c936-4a2e-98df-e80627c41bf8 | String |  |
| data | - | Object | Match hsitory. |
| data.videoUrl | https://img.jbzj.com/file_images/article/202106/2021062116032963.png | String | Video url. |
| data.imageUrl | https://img.jbzj.com/file_images/article/202106/2021062116032963.png | String | Image url. |
| data.videoSize | 105196 | Integer | Video Size |
| data.imageSize | 105196 | Integer | Image Size |
| data.verificationStatus | verified | String | Verification result. verified or failed |
| data.verificationMessage | null | Null | Verification error message. |
| data.timeSpent | 5147 | Integer | The amount of time taken. |
| data.createdAt | 2023-12-03 01:16:14 | String |  |
| data.updatedAt | 2023-12-03 01:16:14 | String |  |

#### 错误响应示例

```javascript
{
  "code": 50400,
  "msg": "Request is unauthorized.",
  "traceId": "284e88df-6087-4e7b-bb01-6a18e03fea9b"
}

```

| 参数名 | 示例值 | 参数类型 | 参数描述 |
| --- | --- | ---- | ---- |
| code | 50400 | Integer | Returning 0 indicates a normal request, while any other situation is considered abnormal. |
| msg | Request is unauthorized. | String | Returning  error message. |
| traceId | 284e88df-6087-4e7b-bb01-6a18e03fea9b | String |  |