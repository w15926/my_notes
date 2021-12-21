# js-cooke

### install

```shell
npm i js-cookie
```

### use

```js
import Cookies from 'js-cookie'

const TokenKey = 'location_user_token'

export function getToken() {
  return Cookies.get(TokenKey)
}

export function setToken(token) {
  return Cookies.set(TokenKey, token)
}

export function removeToken() {
  return Cookies.remove(TokenKey)
}
```

```js
// 创建一个在整个站点有效的 cookie:
Cookies.set('name', 'value')
// 创建一个7天后过期的 cookie，在整个站点有效:
Cookies.set('name', 'value', { expires: 7 })
// 创建一个到期的 cookie，对当前页面的路径有效:
Cookies.set('name', 'value', { expires: 7, path: '' })

// 删除指定 cookie
Cookies.remove('name')
// 删除对当前页面路径有效的 cookie:
Cookies.set('name', 'value', { path: '' });
Cookies.remove('name'); // fail!
Cookies.remove('name', { path: '' }) // removed!

// 获取指定 cookie 值:
Cookies.get('name') // => 'value'
Cookies.get('nothing') // => undefined
// 获取所存在的 cookie
Cookies.get() // => { name: 'value' }
```

