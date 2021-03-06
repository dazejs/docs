# 控制器

控制器负责处理传入的请求，并返回对客户端的响应。

---

## 定义

控制器必须继承基础控制器 `Controller`

```ts {3}
import { Controller } from '@dazejs/framework'

export default class User extends Controller {
  // ...
}

```

---

## 定义路由

控制器使用 `@route([prefix])` 装饰器来表示该控制器使用了路由功能

```ts
import { Controller, route } from '@dazejs/framework'

// 当前控制器的端点访问以 '/users' 开头 (可省略开头 '/')
@route('/users')
export default class User extends Controller {
  // ...
}
```

框架提供了 `get`, `post`, `put`,`patch`,`del`,`head`,`option`, `all` 装饰器（位于 `http` 命名空间下），用于创建访问路由：

```ts
import { Controller, route, http } from '@dazejs/framework'

// 当前控制器的端点访问以 '/users' 开头 (可省略开头 '/')
@route('users')
export default class User extends Controller {
    // get /users
    @http.het()
    index() {
        // ...
    }

    // put /users/:id
    @http.put(':id')
    update(id) {
        // ...
    }
}
```

上面代码会自动创建`get`方法访问的`/users`的端点与`put`方法访问的`/users/:id`的端点

> `@http.all` 装饰器会使用所有 `http` 模块支持的方法，详细信息见 `http` 模块的 `http.METHODS` 属性

---

## 路由参数

路由的参数框架会自动注入到控制器方法中：

```ts
import { Controller, route, http } from '@daze/framework';

@route()
export default class User extends Controller {
    @http.get('/:name/:age')
    index(name, age) {
        // ...
    }
}
```

> 路由参数根据定义顺序注入

---

## REST 风格

使用 `@rest` 装饰器代替 `@route` 装饰器，并且默认 `Rest` 方法无需添加装饰器:

```ts
import { Controller, rest } from '@daze/framework';

@rest('posts')
export default class Post extends Controller {
   /**
   * Display a listing of the resource.
   */
  index() {
    //
  }

  /**
   * Show the form for creating a new resource.
   */
  create() {
    //
  }

  /**
   * Display the specified resource.
   * @param {number} id
   */
  show(id) {
    //
  }

  /**
   * Show the form for editing the specified resource.
   * @param {number} id
   */
  edit(id) {
    //
  }

  /**
   * Store a newly created resource in storage.
   */
  store() {
    //
  }

  /**
   * Update the specified resource in storage.
   * @param {number} id
   */
  update(id) {
    //
  }

  /**
   * Remove the specified resource from storage.
   * @param {number} id
   */
  destroy(id) {
    //
  }
}
```

内置 `Rest` 风格方法：

| 请求类型 | 资源地址        | 控制器方法 | 说明             |
| -------- | --------------- | ---------- | ---------------- |
| get      | /posts          | index()      | 索引/列表        |
| get      | /posts/create   | create()     | 创建（显示表单)  |
| post     | /posts          | store()      | 保存你创建的数据 |
| get      | /posts/:id      | show(id)       | 获取对应id的内容 |
| get      | /posts/:id/edit | edit(id)       | 编辑（显示表单） |
| put      | /posts/:id      | save(id)       | 保存你编辑的数据 |
| delete   | /posts/:id      | destroy(id)    | 删除对应id的内容 |
