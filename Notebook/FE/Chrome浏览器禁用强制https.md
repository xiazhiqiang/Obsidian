在 Chrome 浏览器中解除站点强制 HTTPS 访问（HSTS 强制跳转），可通过以下方法操作：

---

### **方法 1：清除特定域名的 HSTS 策略**

1. 在地址栏输入 `chrome://net-internals/#hsts` 并回车。
2. 在 **Delete domain security policies** 下输入目标域名（如 `example.com`），点击 **Delete**。
    - 若域名是子域名（如 `sub.example.com`），还需删除主域名（`example.com`）的 HSTS 策略。
3. 在 **Query HSTS/PKP domain** 下输入域名并点击 **Query**，若返回 **Not found** 则表示删除成功

### **方法 2：调整站点权限**

1. 访问目标站点时，点击地址栏左侧的 🔒（锁形图标），选择 **网站设置**。
2. 在 **不安全内容** 选项下，启用 **允许**。
