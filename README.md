# 猫咪聊天应用 (Cat Talk)

这是一个基于Firebase实时数据库的匿名聊天应用，用户无需注册即可聊天。

## 功能特点

- 自动生成匿名用户ID
- 实时消息推送（延迟<200ms）
- 消息历史记录保存
- 响应式设计，适配移动设备
- 安全措施：HTML转义，防止XSS攻击

## 设置步骤

### 1. 创建Firebase项目

1. 访问 [Firebase控制台](https://console.firebase.google.com/)
2. 点击「添加项目」→ 输入项目名称（如AnonymousChat）
3. 禁用Google Analytics（可选）

### 2. 配置Realtime Database

1. 在Firebase控制台中，导航至「构建」→「Realtime Database」
2. 创建数据库（选择位置靠近用户的区域）
3. 选择「测试模式」启动（仅开发环境）

### 3. 配置数据库规则

进入Realtime Database → 规则页面，修改为以下规则：

```json
{
  "rules": {
    "messages": {
      ".read": true,
      ".write": true,
      "$message": {
        ".validate": "newData.hasChildren(['userId', 'text', 'timestamp']) 
                     && newData.child('text').isString() 
                     && newData.child('text').val().length <= 500"
      }
    }
  }
}
```

> 注意：这些规则适合测试环境。生产环境应该实施更严格的安全规则。

### 4. 获取Firebase配置

1. 进入项目设置 → 找到「您的应用」部分
2. 创建Web应用 → 注册应用（名称随意）
3. 将SDK配置信息复制到`index.html`文件中的相应位置：

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT.firebaseio.com",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

## 本地测试

可以使用简单的HTTP服务器运行项目：

```bash
# 使用Python
python -m http.server 3000

# 或使用Node.js
npx serve
```

## 部署选项

### GitHub Pages

1. 创建GitHub仓库
2. 上传代码
3. 启用GitHub Pages
4. 访问地址：https://用户名.github.io/仓库名

### Vercel/Netlify

直接从GitHub仓库部署，或拖拽项目文件夹至相应平台。

## 扩展功能

- 用户在线状态指示
- 消息已读/已送达状态
- 多房间聊天支持
- 图片/文件共享
- 集成Firebase Authentication实现更安全的用户系统

## 免费配额提醒

Firebase免费套餐包含：
- 存储空间：1GB
- 下载流量：10GB/月
- 同时连接数：10万
- 写入操作：2万/天

## 许可证

MIT 