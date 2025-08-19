# DM 验证码 Web SDK

🔒 DM web 端 SDK

- [x] 20250728：滑块插件功能已实现
- [x] 20250819：旋转、拼图插件功能已实现

## 🚀 快速开始

### 安装

```bash
# NPM 安装
npm install fm-captcha-web-sdk

# Yarn 安装
yarn add fm-captcha-web-sdk

# pnpm 安装
pnpm add fm-captcha-web-sdk
```

### 方式一：script 标签直接引入

```html
<!DOCTYPE html>
<html>
  <head>
    <title>验证码演示</title>
  </head>
  <body>
    <!-- 装载验证码的父元素，需要是相对定位position:relative -->
    <div class="login-form">
      <!-- 执行操作的按钮 -->
      <button class="login-btn">登录</button>
      <!-- 装载验证码的div 不用单独设置样式，打开时会铺满整个父元素 -->
      <div id="captcha-box"></div>
    </div>
  </body>
</html>
<!-- 只需引入一个JS文件 -->
<script src="node_modules/fm-captcha-web-sdk/dist/fm-captcha-web-sdk.min.js"></script>
<script>
  // 参数配置
  const config = {
    requestCaptchaDataUrl: 'https://xxxx.xxx.xxx/getCaptcha?type=SLIDER', // 验证码数据请求地址
    validCaptchaUrl: 'https://xxxx.xxx.xxx/check', // 验证接口地址
    bindEl: '#captcha-box', // 验证码挂载的元素，需要是id元素
    validSuccess: (res) => console.log('验证成功', res, 当前插件, sdk实例), // 验证成功回调
    validFail: (res) => console.log('验证失败', res, 当前插件, sdk实例) // 验证失败回调
  }

  // 实例化验证码对象
  const captcha = new FmCaptchaWebSdk(config)
  // 初始化验证码插件
  captcha.init()
</script>
```

### 方式二：ES6 模块引入

```javascript
import FmCaptchaWebSdk from 'fm-captcha-web-sdk'

const config = {
  requestCaptchaDataUrl: 'https://xxxx.xxx.xxx/getCaptcha?type=SLIDER',
  validCaptchaUrl: 'https://xxxx.xxx.xxx/check',
  bindEl: '#captcha-box'
}

const captcha = new FmCaptchaWebSdk(config)
captcha.init()
```

### 方式三：CommonJS 引入

```javascript
const FmCaptchaWebSdk = require('fm-captcha-web-sdk')

const config = {
  requestCaptchaDataUrl: 'https://xxxx.xxx.xxx/getCaptcha?type=SLIDER',
  validCaptchaUrl: 'https://xxxx.xxx.xxx/check',
  bindEl: '#captcha-box'
}

const captcha = new FmCaptchaWebSdk(config)
captcha.init()
```

## 📋 配置参数

### config 主要配置

| 参数                    | 类型     | 必填 | 说明                                    |
| ----------------------- | -------- | ---- | --------------------------------------- |
| `bindEl`                | string   | ✅   | 验证码绑定的 DOM 元素选择器             |
| `requestCaptchaDataUrl` | string   | ✅   | 获取验证码数据的接口地址                |
| `validCaptchaUrl`       | string   | ✅   | 验证码校验接口地址                      |
| `validSuccess`          | function | ❌   | 验证成功回调函数(res,当前插件,sdk 实例) |
| `validFail`             | function | ❌   | 验证失败回调函数(res,当前插件,sdk 实例) |
| `btnCloseFun`           | function | ❌   | 关闭按钮点击回调                        |
| `btnRefreshFun`         | function | ❌   | 刷新按钮点击回调                        |
| `requestHeaders`        | object   | ❌   | 请求头配置                              |

### styleConfig 样式配置

| 参数                       | 类型   | 必填 | 说明                                |
| -------------------------- | ------ | ---- | ----------------------------------- |
| `moveTrackMaskBgColor`     | string | ❌   | 移动边框背景色，默认#89d2ff         |
| `moveTrackMaskBorderColor` | string | ❌   | 移动边框颜色，默认#0298f8           |
| `i18n`                     | string | ❌   | tips_success: '验证成功,耗时%s 秒', |

## 👥 常见集成场景

### Vue.js 集成

```javascript
// Vue 3 Composition API
import { ref, onMounted } from 'vue'
import FmCaptchaWebSdk from 'fm-captcha-web-sdk'

export default {
  setup() {
    const captchaRef = ref(null)

    const initCaptcha = () => {
      const captcha = new FmCaptchaWebSdk({
        bindEl: captchaRef.value,
        requestCaptchaDataUrl: 'https://your-api.com/captcha',
        validCaptchaUrl: 'https://your-api.com/verify',
        validSuccess: (res) => {
          console.log('验证成功', res, 当前插件, sdk实例)
        }
      })
      captcha.init()
    }

    return { captchaRef, initCaptcha }
  }
}
```

### React 集成

```javascript
import React, { useRef, useEffect } from 'react'
import FmCaptchaWebSdk from 'fm-captcha-web-sdk'

function CaptchaComponent() {
  const captchaRef = useRef(null)

  useEffect(() => {
    const captcha = new FmCaptchaWebSdk({
      bindEl: captchaRef.current,
      requestCaptchaDataUrl: 'https://your-api.com/captcha',
      validCaptchaUrl: 'https://your-api.com/verify'
    })
    captcha.init()

    return () => captcha.destroyWindow()
  }, [])

  return <div ref={captchaRef}></div>
}
```

### 生产环境配置建议

```javascript
const config = {
  // 建议使用 HTTPS 接口
  requestCaptchaDataUrl: 'https://your-api.com/captcha',
  validCaptchaUrl: 'https://your-api.com/verify',

  // 错误处理
  validFail: (res) => {
    console.error('验证失败:', res, 当前插件, sdk实例)
    // 发送错误统计
  }
}
```

## 🎯 技术特性

- **无外部依赖** - 纯原生 JavaScript 实现
- **体积小巧**
- **跨浏览器兼容** - 支持现代浏览器和 IE11+
- **UMD 模块格式** - 支持 AMD、CommonJS、全局变量
- **响应式设计** - 自适应不同屏幕尺寸
- **内置资源** - CSS 样式和图片已内联，无需额外文件

## 📋 API 文档

### 主要方法

```javascript
const captcha = new FmCaptchaWebSdk(config, styleConfig)

// 初始化验证码
captcha.init()

// 销毁组件窗口，移除模板元素
captcha.destroyWindow()

// 重新加载验证码
captcha.reloadCaptcha()

// 打开验证码
captcha.openCaptcha()

// 创建验证码
captcha.createCaptcha()
// 销毁验证码
captcha.destroyCaptcha()
```

## 📧 联系我们

757276716@qq.com
