# 服务目的
开发一个自拍验证服务器。例如，在Tinder上，如果用户成功录制一段与其个人资料照片匹配的实时视频，用户可以获得一个验证过的个人资料徽章。

# 服务端点
### 服务请求端口：8080
#### 身份验证API
**请求路径：** `/login`  
**请求方法：** POST  
**API作用：** 这个端点用于不同品牌的系统进行身份验证

#### 图片与视频匹配校验
**请求路径：** `/api/v1/challenge`  
**请求方法：** POST  
**API作用：** 这个端点用于校验给定的视频与图片是否匹配

#### 校验历史记录查询
**请求路径：** `/api/v1/challenge/history`  
**请求方法：** POST  
**API作用：** 这个端点用于查询最后一次匹配校验的结果

# 认证指南
本服务使用基于账户密码的认证方式，并支持多租户。以下是详细的认证步骤：
## 获取认证令牌
首先，你需要使用你的账户名和密码获取一个认证令牌。这可以通过发送一个POST请求到`/login`端点来完成。请求体应该包含你的账户名和密码，以及租户ID。例如：
```json
{
  "username": "your_username",
  "password": "your_password",
  "tenantId": "your_tenant_id"
}
```
如果你的账户名和密码是正确的，服务器会返回一个JSON响应，其中包含你的认证令牌：
```json
{
    "code": 0,
    "msg": "successful",
    "traceId": "27ff48aa-ef06-4ad2-be14-cedf95af407b",
    "data": {
        "token": "your_token"
    }
}
```

## 使用令牌认证
一旦你获取到了认证令牌，你就可以使用它来访问服务的其他端点。你应该在你的请求头中包含这个令牌，例如：
```bash
curl -XPOST /api/v1/challenge \ 
-H "Authorization: Bearer your_token" \ 
-H "Content-Type: application/json" \ 
-d '{"imageUrl": "", "videoUrl": ""}'
```
请注意，你需要将`your_token`替换为你实际的认证令牌。  
如果你的令牌是有效的，服务器会处理你的请求并返回一个响应。如果你的令牌是无效的，服务器会返回一个错误。

## 令牌过期
请注意，你的认证令牌在一定时间后会过期。如果你的令牌过期了，你需要重新获取一个新的令牌。

# 监控

### 系统健康相关指标
**指标名称：**`system.health`  
**标签：**`status`, `database`, `diskSpace`, `customService`  
**值：**  
	- `status`:  `UP`表示正常，`DOWN`表示有问题。  
	- `database`:  `UP`表示数据库连接正常，`DOWN`表示数据库连接有问题。  
	- `diskSpace`:  `UP`表示磁盘空间充足，`DOWN`表示磁盘空间不足。  
	- `customService`:  `UP`表示服务运行正常，`DOWN`表示服务运行异常。  

### 业务相关指标
**指标名称：**`match.info`  
**标签：**`total`, `successful`, `failed`  
**值：**  
	- `total`: 总的匹配次数  
	- `successful`: 成功的次数  
	- `failed`: 失败的次数  