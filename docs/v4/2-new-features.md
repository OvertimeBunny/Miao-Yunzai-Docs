---
sidebar_position: 2
---

# 模块

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';


:::tip 提示

所有行为都应该是从推荐的模块中导出，这是统一的接口规范。

:::


## 核心

### 导入

```ts
import * as Core from 'yunzai/core'
```

### 开发

<Tabs>
  <TabItem value="apple" label="回调" default>

这是新增的函数式编程，其简洁的写法更容易阅读和理解

```ts title="./message.ts"
import { Messages } from 'yunzai/core'
const message = new Messages();
message.response(/^(#|\/)?你好/, async e => {
    e.reply('你好')
})
export default message
```

机器人接收消息时进行正则匹配，若成功将执行对应的回调函数

函数从当前事件`e`(Event)中，执行回复函数，并发送你好。

```ts title="./index.ts"
import { Events } from 'yunzai/core'
import App$1 from './message.js'
const event = new Events()
event.use(App$1.ok)
export const apps = event.ok
```
通过Events收集回调事件，并导出名为`apps`的变量，此变量机器人将识别后加载。

  </TabItem>
  <TabItem value="orange" label="继承">

配置更丰富，且功能复杂的继承机制

```ts title="./message.ts"
import { Plugin } from 'yunzai/core'
export default class App extends Plugin {
  constructor () {
    super()
    this.priority = 700
    this.rule = [
        {
          reg:/^(#|\/)?你好/,
          fnc: this.hello.name
        },
      ]
  }
  async hello () {
    this.e.reply('你好')
  }
}
```

机器人接收消息时，将执行正则，若命中则根据函数名执行对应函数

函数从当前事件`this.e`(Event)中，执行回复函数，并发送你好。

```ts title="./index.ts"
import { Events } from 'yunzai/core'
import App$1 from './message.js'
const event = new Events()
event.use(App$1)
export const apps = event.ok
```

通过Events收集回调事件，并导出名为`apps`的变量，此变量机器人将识别后加载。

  </TabItem>
</Tabs>

## 配置

```ts 
import * as Config from 'yunzai/config'
```

配置模块主要分为`系统性常量`和`系统配置器`

- 系统性常量

```ts
import { BOT_NAME } from 'yunzai/config'
```

这是无法修改的,存在于内容,且运行后不变的

- 系统配置器

```ts
import { ConfigController } from 'yunzai/config'
```

配置器包含了配置文件内的所有参数.

## 工具

```ts 
import * as Utils from 'yunzai/utils'
```

该模块是与机器人运行无关的,但与config系统有关的.

主要辅助开发者更快的实现业务,而不再需要关注方法的实现


## 数据

```ts 
import * as DB from 'yunzai/db'
```

当前数据分为`Redis`和`Sqlite`数据库

## 原神


```ts 
import * as MYS from 'yunzai/mys'
```

米游接口 -- 非原神插件禁止使用,未来版本将废弃
