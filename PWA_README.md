# MiSub PWA 功能说明

## 🚀 PWA (Progressive Web App) 特性

MiSub 现已支持 PWA 功能，为用户提供类原生应用的体验！

### ✨ 主要功能

#### 📱 可安装性
- **桌面安装**: 可以将 MiSub 安装到电脑桌面，像本地应用一样运行
- **移动端添加**: 在手机浏览器中可以"添加到主屏幕"
- **独立窗口**: 安装后以独立窗口运行，没有浏览器地址栏干扰

#### 🔄 离线支持
- **智能缓存**: 自动缓存应用资源和已访问的内容
- **离线访问**: 网络断开时仍可查看已缓存的订阅信息
- **优雅降级**: 离线时显示专门的离线页面，提供最佳用户体验

#### 🎯 自动更新
- **后台更新**: Service Worker 在后台自动检查并下载新版本
- **更新提示**: 有新版本时会显示底部通知条，用户可选择立即更新
- **无缝升级**: 点击更新后自动重载，获取最新功能

#### ⚡ 性能优化
- **快速启动**: 预缓存关键资源，启动速度大幅提升
- **智能缓存策略**: API 请求采用网络优先策略，静态资源使用缓存优先
- **渐进式加载**: 支持分块加载，减少初始加载时间

### 🛠️ 技术实现

#### 核心技术栈
- **Vite PWA Plugin**: 自动生成 Service Worker 和 Manifest
- **Workbox**: 提供强大的缓存策略和离线支持
- **Vue 3**: 响应式更新提示组件

#### 缓存策略
```javascript
// API 请求 - 网络优先
{
  urlPattern: /^https:\/\/api\..*/i,
  handler: 'NetworkFirst',
  options: {
    cacheName: 'api-cache',
    networkTimeoutSeconds: 10
  }
}

// 静态资源 - 缓存优先
{
  urlPattern: /.*\.(js|css|html)$/,
  handler: 'StaleWhileRevalidate',
  options: {
    cacheName: 'static-cache'
  }
}
```

### 📲 如何触发PWA安装

#### 🎯 自动安装提示
MiSub会在满足以下条件时自动显示安装提示：
- 网站通过HTTPS访问（或localhost）
- 网站有有效的Web App Manifest
- 网站注册了Service Worker
- 用户在网站上有一定的互动（通常是几秒钟）
- 浏览器支持PWA安装

#### 🔧 手动触发安装

**方式1：使用应用内安装按钮**
- 登录后在头部导航栏会显示"安装应用"按钮
- 点击该按钮即可触发安装提示
- 如果浏览器不支持，会显示"安装说明"按钮

**方式2：浏览器地址栏**
- Chrome/Edge：地址栏右侧的安装图标（⊕ 或 💻）
- 点击图标选择"安装MiSub"或"安装此站点为应用"

**方式3：浏览器菜单**
- Chrome：菜单 → "安装MiSub..." 或 "更多工具" → "创建快捷方式" → 勾选"在窗口中打开"
- Edge：菜单 → "应用" → "安装此站点为应用"
- Safari移动端：分享按钮 → "添加到主屏幕"

**方式4：开发者工具（仅开发环境）**
- 按 `Ctrl+Shift+P` 打开开发者工具面板
- 点击"触发安装提示"按钮测试安装流程

#### 📱 各平台安装方法

##### 桌面端（Windows/Mac/Linux）
1. **Chrome浏览器**
   - 访问MiSub网站
   - 等待几秒后会自动显示安装横幅
   - 或点击地址栏右侧的安装图标
   - 选择"安装"确认
   - 应用将添加到桌面和开始菜单

2. **Edge浏览器**
   - 访问MiSub网站
   - 点击地址栏右侧的应用图标
   - 选择"安装此站点为应用"
   - 应用将添加到桌面和开始菜单

3. **Safari浏览器**
   - Safari暂不支持桌面PWA安装
   - 可以通过"文件" → "添加到程序坞"创建快捷方式

##### 移动端（iOS/Android）
1. **Android Chrome**
   - 访问MiSub网站
   - 点击右上角菜单（⋮）
   - 选择"安装应用"或"添加到主屏幕"
   - 确认添加，图标将出现在主屏幕

2. **iOS Safari**
   - 访问MiSub网站
   - 点击底部分享按钮（□↑）
   - 滑动找到"添加到主屏幕"
   - 确认添加，图标将出现在主屏幕

3. **Android其他浏览器**
   - 大多数基于Chromium的浏览器都支持PWA安装
   - 查看浏览器菜单中的"添加到主屏幕"选项

#### ⚠️ 安装要求检查
如果无法触发安装，请检查以下条件：

1. **HTTPS要求**
   ```bash
   # 检查网址是否为HTTPS
   https://your-domain.com  ✅
   http://your-domain.com   ❌（不支持，除了localhost）
   http://localhost:5173    ✅（开发环境例外）
   ```

2. **Service Worker状态**
   - 打开浏览器开发者工具
   - 切换到"Application"标签页
   - 检查"Service Workers"部分
   - 状态应该是"activated and running"

3. **Manifest文件**
   - 在开发者工具的"Application"标签页
   - 检查"Manifest"部分
   - 确保所有字段都正确加载

4. **浏览器支持**
   | 浏览器 | 桌面安装 | 移动端安装 | 版本要求 |
   |--------|----------|------------|----------|
   | Chrome | ✅ | ✅ | 67+ |
   | Edge | ✅ | ✅ | 79+ |
   | Safari | ❌ | ✅ | 11.3+ |
   | Firefox | ❌ | ❌ | 不支持 |

#### 🛠️ 故障排除

**问题1：没有显示安装提示**
- 确保网站通过HTTPS访问
- 检查Service Worker是否正确注册
- 尝试在无痕模式下访问
- 清除浏览器缓存后重试

**问题2：安装按钮不可用**
- 检查浏览器是否支持PWA
- 确保manifest.json文件可访问
- 验证所有PWA要求是否满足

**问题3：安装后无法正常使用**
- 检查所有资源是否正确缓存
- 验证Service Worker缓存策略
- 确保API接口在离线时有合适的降级处理

#### 🎨 自定义安装体验

**延迟显示安装提示**
```javascript
// 在PWAInstallPrompt.vue中可以调整
// 延迟3秒后显示提示
setTimeout(() => {
  showToast('发现您可以安装MiSub到桌面，获得更好的使用体验！', 'info', 6000);
}, 3000);
```

**自定义安装条件**
```javascript
// 可以根据用户行为决定是否显示安装提示
const shouldShowInstall = () => {
  const visitCount = localStorage.getItem('visitCount') || 0;
  const isLoggedIn = sessionStore.sessionState === 'loggedIn';
  return visitCount > 2 && isLoggedIn;
};
```

### 🔧 开发和部署

#### 本地开发
```bash
# 启动开发服务器（包含 PWA 功能）
npm run dev

# 检查 PWA 配置
npm run check-pwa

# 构建 PWA 版本
npm run build-pwa
```

#### PWA 功能验证
1. 打开浏览器开发者工具
2. 切换到 "Application" 标签页
3. 检查左侧 "Service Workers" 和 "Manifest" 部分
4. 确保 Service Worker 状态为 "activated"
5. 验证 Manifest 信息正确显示

#### 部署注意事项
- 确保 HTTPS 部署（PWA 要求）
- 所有图标文件应为真实的 PNG 格式
- 检查 manifest.json 路径正确
- 验证 Service Worker 能够正常注册

### 🎨 自定义配置

#### 修改应用信息
编辑 `vite.config.js` 中的 manifest 配置：
```javascript
manifest: {
  name: '你的应用名称',
  short_name: '简短名称',
  description: '应用描述',
  theme_color: '#你的主题色',
  background_color: '#背景色'
}
```

#### 调整缓存策略
在 `vite.config.js` 的 workbox 配置中修改：
```javascript
workbox: {
  globPatterns: ['**/*.{js,css,html,ico,png,svg}'],
  runtimeCaching: [
    // 添加自定义缓存规则
  ]
}
```

### 📊 PWA 评分

使用 Chrome DevTools 的 Lighthouse 可以检查 PWA 质量评分：
- 性能（Performance）
- 可访问性（Accessibility）  
- 最佳实践（Best Practices）
- SEO
- PWA

### 🤝 浏览器支持

| 浏览器 | 桌面安装 | 移动端安装 | Service Worker |
|--------|----------|------------|----------------|
| Chrome | ✅ | ✅ | ✅ |
| Edge | ✅ | ✅ | ✅ |
| Safari | ❌ | ✅ | ✅ |
| Firefox | ❌ | ❌ | ✅ |

### 🔗 相关资源

- [PWA 官方文档](https://web.dev/progressive-web-apps/)
- [Vite PWA Plugin](https://vite-plugin-pwa.netlify.app/)
- [Workbox 指南](https://workbox.js.org/)
- [Web App Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest)

---

享受 MiSub 的 PWA 体验吧！🎉