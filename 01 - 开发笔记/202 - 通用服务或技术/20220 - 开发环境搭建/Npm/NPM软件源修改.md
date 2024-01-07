# 临时使用特定的NPM源
〈br/>

```bash
npm --registry https://registry.npmmirror.com install express 

# 推荐使用淘宝NPM源
https://registry.npm.taobao.org
```

# 永久修改本地的NPM源
<br/>

```bash
npm config set registry https://registry.npmmirror.com

# 推荐使用淘宝NPM源
https://registry.npm.taobao.org
```
<br/>

# 查看NPM源地址是否修改成功
```bash
npm config get registry
```
<br/>

# 重制NPM源为官方源
```bash
npm config get registry
```