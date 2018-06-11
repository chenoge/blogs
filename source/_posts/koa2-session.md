---
title: koa2-session
date: 2018-04-12 12:40:00
tags: [koa2,session]
---

# 一、session应用

```javascript
const Koa = require('koa')
const session = require('koa-session-minimal')
// koa-mysql-session为koa-session-minimal中间件提供MySQL数据库的session数据读写操作
// koa-session-minimal默认使用内存保存session对象
const MysqlSession = require('koa-mysql-session')
const app = new Koa()

// 配置存储session信息的mysql
let store = new MysqlSession({
  user: 'root',
  password: 'abc123',
  database: 'koa_demo',
  host: '127.0.0.1',
})

// 存放sessionId的cookie配置
let cookie = {
  maxAge: '', // cookie有效时长
  expires: '',  // cookie失效时间
  path: '', // 写cookie所在的路径
  domain: '', // 写cookie所在的域名
  httpOnly: '', // 是否只用于http请求中获取
  overwrite: '',  // 是否允许重写
  secure: '',
  sameSite: '',
  signed: ''
}

// 使用session中间件
app.use(session({
  key: 'SESSION_ID',
  store: store,
  cookie: cookie
}))

app.use( async ( ctx ) => {
    // 设置session
    if ( ctx.url === '/set' ) {
    ctx.session = {
      user_id: Math.random().toString(36).substr(2),
      count: 0
    }
    ctx.body = ctx.session
  } else if ( ctx.url === '/' ) {
      // 读取session对象
      ctx.session.count = ctx.session.count + 1
      ctx.body = ctx.session
  }
})

app.listen(3000)
console.log('[demo] session is starting at port 3000')
```

<br/>

# 二、koa-session-minimal源码

#### 1、目录结构

- [session.js](https://github.com/longztian/koa-session-minimal/blob/master/src/session.js) 
- [memory_store.js](https://github.com/longztian/koa-session-minimal/blob/master/src/memory_store.js) 
- [store.js](https://github.com/longztian/koa-session-minimal/blob/master/src/store.js) 

<br/>

#### 2、session.js

**session.js做两件事**

1. **请求进来时，根据cookies中的sid来初始化ctx.session对象**
2. **请求返回时，根据ctx.session的变化，更新ctx.session对象**


```javascript
const uid = require('uid-safe')
const deepEqual = require('deep-equal')
const Store = require('./store')
const MemoryStore = require('./memory_store')

const ONE_DAY = 24 * 3600 * 1000 // one day in milliseconds

const cookieOpt = (cookie, ctx) => {
  const obj = cookie instanceof Function ? cookie(ctx) : cookie
  const options = Object.assign({
    maxAge: 0, // default to use session cookie
    path: '/',
    httpOnly: true,
  }, obj || {}, {
    overwrite: true, // overwrite previous session cookie changes
    signed: false, // disable signed option
  })
  if (!(options.maxAge >= 0)) options.maxAge = 0
  return options
}

const deleteSession = (ctx, key, cookie, store, sid) => {
  const tmpCookie = Object.assign({}, cookie)
  delete tmpCookie.maxAge
  ctx.cookies.set(key, null, tmpCookie)
  store.destroy(`${key}:${sid}`)
}

const saveSession = (ctx, key, cookie, store, sid) => {
  const ttl = cookie.maxAge > 0 ? cookie.maxAge : ONE_DAY
  ctx.cookies.set(key, sid, cookie)
  store.set(`${key}:${sid}`, ctx.session, ttl)
}

const cleanSession = (ctx) => {
  if (!ctx.session || typeof ctx.session !== 'object') ctx.session = {}
}

module.exports = (options) => {
  const opt = options || {}
  const key = opt.key || 'koa:sess'
  const store = new Store(opt.store || new MemoryStore())
  const getCookie = ctx => cookieOpt(opt.cookie, ctx)

  return async (ctx, next) => {
    // initialize session id and data
    // 根据cookies中的sid初始化ctx.session对象
    const oldSid = ctx.cookies.get(key)

    let sid = oldSid

    const regenerateId = () => {
      sid = uid.sync(24)
    }

    if (!sid) {
      regenerateId()
      ctx.session = {}
    } else {
      ctx.session = await store.get(`${key}:${sid}`)
      cleanSession(ctx)
    }

    const oldData = JSON.parse(JSON.stringify(ctx.session))

    // expose session handler to ctx
    ctx.sessionHandler = {
      regenerateId,
    }

    await next()
      
    // 保存更新后的ctx.session对象
    cleanSession(ctx)
    const hasData = Object.keys(ctx.session).length > 0

    if (sid === oldSid) { // session id not changed
      if (deepEqual(ctx.session, oldData)) return // session data not changed

      const cookie = getCookie(ctx)
      const action = hasData ? saveSession : deleteSession
      action(ctx, key, cookie, store, sid) // update or delete the existing session
    } else { // session id changed
      const cookie = getCookie(ctx)
      if (oldSid) deleteSession(ctx, key, cookie, store, oldSid) // delete old session
      if (hasData) saveSession(ctx, key, cookie, store, sid) // save new session
    }
  }
}
```

#### 3、memory_store.js

```javascript
module.exports = class MemoryStore {
  constructor() {
    this.sessions = {} // data
    this.timeouts = {} // expiration handler
  }

  get(sid) {
    return this.sessions[sid]
  }

  set(sid, val, ttl) {
    if (sid in this.timeouts) clearTimeout(this.timeouts[sid])

    this.sessions[sid] = val
    this.timeouts[sid] = setTimeout(() => {
      delete this.sessions[sid]
      delete this.timeouts[sid]
    }, ttl)
  }

  destroy(sid) {
    if (sid in this.timeouts) {
      clearTimeout(this.timeouts[sid])

      delete this.sessions[sid]
      delete this.timeouts[sid]
    }
  }
}
```

#### 4、store.js

```javascript
const co = require('co')

module.exports = class Store {
  constructor(store) {
    this.store = store
  }

  get(sid) {
    return co(this.store.get(sid))
  }

  set(sid, val, ttl) {
    return co(this.store.set(sid, val, ttl))
  }

  destroy(sid) {
    return co(this.store.destroy(sid))
  }
}
```

