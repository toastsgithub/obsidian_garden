---
{"publish":true,"created":"2025-12-17T10:27:40.000+08:00","modified":"2025-12-21T22:10:28.240+08:00","cssclasses":""}
---


> TIL:
> 1. CORS 只限制 js 读请求结果，但请求本身是发送出去的
> 2. csrf 本质防的是浏览器跨站自动携带凭证

#技术漫游 #服务安全


> [!NOTE] 经典的 CSRF 场景：
> hijack 网站在他自己的站内嵌入了被攻击的网站：
>
> `hijack.com 里内嵌 bank.com/api/send_money/`
>
> 用户就在 hijack 的站内非预期地触发了 send_money 的请求，此时因为用户浏览器里有 bank.com 的相关 cookie，就有可能携带上并攻击成功


## 解法① Csrf Token

1. 页面加载的时候问服务端拿一个随机生成的 csrf token
2. 每次请求的时候携带 csrf token
3. 服务端校验 token

上述场景里即便 hijack.com 诱导用户携带正确的 cookie 请求了 send_money，但这个请求过程没有携带 csrf token 所以会被拦截，而获取和携带 csrf token 的逻辑只有被攻击的原站点才有

> 当然如果极端点, csrf token 在一个周期里是复用的，而且 hijack.com 也能拿到这个 token，那是有可能被攻破的

## 解法② Cookie Samesite 属性

cookie 的 sameSite 属性设置为 strict 或者 lax

- strict: 任何情况不携带 cookie
- lax: 仅在 `顶级导航的 GET 请求` 中携带

上述场景就不会携带 cookie，自然就没有请求相关的凭证 [^1]

> 但在实际业务场景中，大型业务系统之间需要跨站请求的情况非常多，所以一般只会把 sameSite 设置为 None（仅在 Secure 下携带 cookie），通过别的手段来防止 csrf

[^1]: 很多攻击都是因为浏览器的机制自动或者非预期地携带了凭证，所以防止凭证被携带和增加凭证的复杂度都是防止攻击的手段
