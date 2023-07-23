
# 生命周期

- `beforeCreate` -> `setup()`
- `created` -> `setup()`
- `beforeMount` -> `onBeforeMount`
- `mounted` -> `onMounted`
- `beforeUpdate` -> `onBeforeUpdate`
- `updated` -> `onUpdated`
- `beforeUnmount` -> `onBeforeUnmount`
- `unmounted` -> `onUnmounted`
- `errorCaptured` -> `onErrorCaptured`
- `renderTracked` -> `onRenderTracked`
- `renderTriggered` -> `onRenderTriggered`



# setup

> **setup在beforecreate钩子之前完成**
>
> *setup函数里没有this、$*
>
> *不用再写data与methods那些，都定义在一个方法里，通过return暴露出去才能调用*

### ref、reactive

#### 基础概念

> **reactive **
>
> >  *本质：就是将所有传入的数据都包装成一个Proxy对象*
> >
> > *传递的不是一个对象或者其他对象，无法实现响应式，数据更新界面不刷新*



> **ref**
>
> > *本质：其实还是reactive*
> >
> > *ref接收到的值后，ref函数底层会自动将ref转换成reactive*
> >
> > *ref( xx )   ---->   reactive( { value: xx } )*



> **递归监听**
>
> > *默认情况下ref和reactive都是递归监听*
> >
> > 递归监听的问题：数据量较大时，非常损耗性能



> **非递归监听 import { shallowRef, shallowReactive, triggerRef } from 'vue'**
>
> > *shallowReactive*
> >
> > > *只监听第一层（第一层包裹Proxy）这种方式只有修改第一层再修改里层才能实现响应式*
> >
> > *shallowRef*
> >
> > > *因为Vue3监听的是state.value的变化，所以直接修改界面都不刷新*
> > >
> > > 必须使用state.value = { a: '1', ... }才可以（因为本质上这才是第一层）
> >
> > *triggerRef*
> >
> > > *Vue3只提供triggerRef没有提供triggerReactive*
> > >
> > > state.value.a.b.c.d = '4' *// 修改第四层数据，此时非递归、非响应式*
> > >
> > > triggerRef(state) *// 让state修修改的这一层触发ref，实现响应式*

```js
// ref监听简单类型   reactive监听复杂类型（对象/数组）
// reactive如果监听其他类型，比如 new Date()，不能实现页面动态刷新，只能通过重新赋值方式
import { ref, reactive } from 'vue'

let count = ref(0)

let state = reactive({
  list: [
    ...
  ] 
})
```



#### ref实例展示 - 直接使用

创建简单类型的响应式

```vue
<template>
  <div>
    <h1>{{ num }}</h1>
    <button @click="myFn">add</button>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'App',
  setup () {
    let num = ref(0) // 变量发生改变ref会自动刷新里面的值

    const myFn = () => {
      num.value += 1 // ref必须通过这种方法更新里面的值
    }

    return { num, myFn } // 外界要想使用必须通过return暴露出去
  }
}
</script>
```



#### ref实例展示 - 引用使用

实为复制，原数据不会发生改变。

```vue
<script>
import { onMounted, ref } from 'vue'

export default {
  name: 'App',
  setup () {
    const obj = {
      num: 1,
      msg: 'hello world'
    }
    let state = ref(obj, 'num')

    onMounted(() => {
      state.value = 2
      console.warn(state.value, obj.num) // 2 1
    })
  }
  
}
</script>
```





#### reactive实例展示

创建响应式对象或数组

```vue
<template>
  <div>
    <h1>{{ state.obj.num }}</h1>
    <h1>{{ state.list[0].num }}</h1>
  </div>
</template>

<script>
import { onMounted, reactive } from 'vue'

export default {
  name: 'App',
  setup () {
    let state = reactive({
      obj: {
        num: 1
      },
      list: [
        { num: 1 }
      ]
    })

    onMounted(() => {
      setInterval(() => {
        ++state.obj.num
        ++state.list[0].num
      }, 1000)
    })

    return { state }
  }
}
</script>
```



### toRef、toRefs

> ref和toRef区别：
>
> >   ref -> 复制  修改响应式数据不会影响以前的数据
> >   toRef -> 引用  修改响应式数据会影响以前的数据
> >
> >   ref -> 数据发生改变    界面自动更新
> >   toRef -> 数据发生改变  界面不会自动更新



#### toRef实例展示 - 直接使用

与ref没有区别

```vue
<template>
  <div>
    <h1>{{ state }}</h1>
  </div>
</template>

<script>
import { onMounted, reactive, toRef } from 'vue'

export default {
  name: 'App',
  setup () {
    let state = toRef(1)

    onMounted(() => {
      setInterval(() => {
        state.value++
      }, 1000)
    })

    return { state }
  }
}
</script>
```



#### toRef实例展示 - 引用使用

原属性obj与state值发生变化，界面不更新。

```vue
<script>
import { onMounted, ref, toRef } from 'vue'

export default {
  name: 'App',
  setup () {
    const obj = {
      num: 1,
      msg: 'hello world'
    }
    let state = toRef(obj, 'num')

    onMounted(() => {
      state.value = 2
      console.warn(state.value, obj.num) // 2 2
    })
  }
  
}
</script>
```



#### toRefs实例展示

相当于对目标对象自动全部使用toRef

```vue
<script>
import { onMounted, ref, toRef, toRefs } from 'vue'

export default {
  name: 'App',
  setup () {
    const obj = {
      num: 1,
      msg: 'hello world'
    }
    let state = toRefs(obj)

    onMounted(() => {
      state.num = 2
      state.msg = 'hello vue'
      console.warn(state, obj) // { num: 2, msg: 'hello vue' } { num: 2, msg: 'hello vue' } 
    })
  }
  
}
</script>
```



## ⭐️常见项目写法

怎么样，是不是很有Vue2的感觉了？

```vue
<template>
  <div>{{ num }}</div>
  <div>{{ msg }}</div>
</template>

<script>
import { onMounted, reactive, toRefs } from 'vue'

export default {
  name: 'App',
  setup () {
    const data = reactive({
      num: 1,
      msg: 'hello world'
    })

    onMounted(() => {
      init()
    })

    const init = () => {
      data.num = 2
      console.warn(data) // {num: 2, msg: 'hello world'}
    }

    return {
      ...toRefs(data) // 可用于解构加引用响应式，因此模版中可直接使用data里的变量。
    }
    
  }
}
</script>
```

> 如果对响应式对象 data 使用扩展运算符后，其内部属性就失去了响应性 。



### 模块化抽象写法

直接实现模块化，DOM里只能使用setup里的return，其他函数的return都是为了给setup使用。

```js
import useRemove from './app2-methods/remove'
import useAdd from './app2-methods/add'

export default {
  name: 'App2',
  setup() {
    let { state, remove } = useRemove() // 解构调用，直接实现模块化
    let { state2, add } = useAdd(state) // 可以用传参方法调用上一行方法内的属性

    return { state, removeStudent, state2, addStudent }
  }
}
```



### 使用ref获取dom

子组件

```vue
<template>
  <h1>son</h1>
</template>

<script>
export default {
  components: {
  },
  setup () {
    const showAlert = () => {
      alert('hello world')
    }
    return {
      showAlert
    }
  }
}
</script>
```

父组件

```vue
<template>
  <Son ref="ref_son" />
</template>

<script>
import Son from './Son.vue'
import { onMounted, reactive, ref, toRefs } from 'vue'

export default {
  name: 'App',
  components: {
    Son
  },
  setup () {
    const ref_son = ref(null)

    onMounted(() => {
      init()
    })

    const init = () => {
      ref_son.value.showAlert() // 调取子组件方法
      console.warn(ref_son.value) // 拿到div
    }

    return {
      ref_son
    }

  }
}
</script>
```

> 唯一要注意的就是变量名与ref绑定的名字要一致



### watch

在Vue3中，watch有着更多的形式。

#### ref

- 监听某个值的变化（常用）

```vue
<script>
import { onMounted, reactive, toRefs } from '@vue/runtime-core'
import { ref, watch } from 'vue'

export default {
  setup () {
    let num = ref(0)

    onMounted(() => {
      init()
    })

    // 指定监听某个值变化,当值发生改变时才触发
    watch(num, (newV, oldV) => {
      console.log(num.value)
    }, {
      // 后两个参数可选，与Vue2一致。
      immediate: true,
      deep: true
    })

    const init = async () => {
      setInterval(() => {
        ++num.value
      }, 1000)
    }
  }
}
</script>
```



- 监听所有响应式数据

```vue
<script>
import { onMounted, reactive, toRefs } from '@vue/runtime-core'
import { ref, watch } from 'vue'

export default {
  setup () {
    let num = ref(0)
    let num2 = ref(100)

    onMounted(() => {
      init()
    })

    watch(() => {
      console.log(num.value, num2.value)
    })

    const init = async () => {
      setInterval(() => {
        ++num.value
        ++num2.value
      }, 1000)
    }
  }
}
</script>
```



- 一次监听多个值

```vue
<script>
import { onMounted, reactive, toRefs } from '@vue/runtime-core'
import { ref, watch } from 'vue'

export default {
  components: {
  },
  setup () {
    let num = ref(0)
    let num2 = ref(100)

    onMounted(() => {
      init()
    })

    watch([num, num2], ([newA, newB], [oldA, oldB]) => {
      console.warn(newA, newB)
      console.warn(oldA, oldB)
    })

    const init = async () => {
      setInterval(() => {
        ++num.value
        ++num2.value
      }, 1000)
    }
  }
}
</script>
```



#### reactive

```js
  setup() {
    const user = reactive({
      a: 1,
      b: 2
    })

    // 默认监听对象里的每一个值
    // watch(user, () => {
    //   console.log(user.a + '---------' + user.b)
    // }, { immediate: true })

    // 无法对整个对象进行 新旧值 的监听
    // watch(user, (newUser, oldUser) => {
    //   console.log('new：', newUser, 'old：', oldUser) // 无法监听新旧对象的变化
    //   console.log(newUser === oldUser) // 第一次false（第一次old为undefined），以后都是true
    // }, { immediate: true })

    // 只能监听对象中指定属性的 新旧值 
    // 单个值
    // watch(() => user.a, (newA, oldA) => {
    //   console.log(newA, '-------------', oldA)
    // })
    // 多个值
    // watch([() => user.a, () => user.b], ([newA, newB], [oldA, oldB]) => {
    //   console.log(newA, '----------', oldA)
    //   console.log(newB, '==========', oldB)
    // })

    return {
      ...toRefs(user)
    }
  }
```



### computed

与Vue2没多大区别

```js
  setup() {
    const state = reactive({
      num1: 0,
      num2: 0,
      result: computed(() => parseInt(state.num1) + parseInt(state.num2))
    })

    return { state }
  }
```



### 父子组件传参

方式很多，只介绍其中常用的两种。

#### 方式一

- 父传子

father

```vue
<template>
  <Son :text="msg" />
</template>

<script>
import Son from './Son.vue'
import { reactive, toRefs } from '@vue/runtime-core'

export default {
  components: {
    Son
  },
  setup () {
    let state = reactive({
      msg: 'Hello World'
    })

    return {
      ...toRefs(state)
    }
  }
}
</script>
```



son

```vue
<template>
  <h1>接收到的值：{{ text }}</h1>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      default: ''
    },
  },
  setup (props, ctx) { // 拿到一整个props对象
    console.warn(props.text) // Hello World
  }
}
</script>
```



- 子传父

son

```vue
<script>
export default {
  setup (props, { emit }) {
    const send = () => {
      emit('msg', 'Hello World')
    }
    send()
    return {
      send
    }
  }
}
</script>
```



father

```vue
<template>
  <Son @msg="sonMsg" />
</template>

<script>
import Son from './Son.vue'

export default {
  components: {
    Son
  },
  setup () {
    const sonMsg = data => {
      console.warn(data) // Hello World
    }

    return {
      sonMsg
    }
  }
}
</script>
```



#### 方式二

有了方法一作为参考，那么方法二中受变化的只有子组件。

```vue
<template>
  <h1>{{ props.text }}</h1>
</template>

<script setup>
import { onMounted } from 'vue';

const props = defineProps({
  text: {
    type: String,
    default: ''
  },
})

const emit = defineEmits(['msg', 'res'])
emit('msg', 'Hello World')
emit('res', 'resresresres')

</script>
```

>  此示例中使用了setup语法糖来完成



### ⭐️setup语法糖

该语法糖默认暴露（return）所有，甚至连导入的组件都会自动注册（就一个setup，不给我自动注册都没地方手动注册😓）。

```vue
<template>
  <Son @msg="sonMsg" @res="sonRes" text="I am you father!" />
  <!-- 输出：2023年07月18日 -->
  <h1>{{ formatter('20230718223640') }}</h1>
</template>

<script setup>
import Son from './Son.vue' // 自动注册
import { formatter } from './methods.js' // 自动导出

const sonMsg = data => console.warn(data)
const sonRes = data => console.warn(data)

console.warn(formatter('20230718223640')) // 2023年07月18日
</script>
```

```js
export const formatter = str => {
  return str.slice(0, 4) + '年' + str.slice(4, 6) + '月' + str.slice(6, 8) + '日'
}
```



### 关于Vue3的事件总线

在Vue3中，官方移除了事件总线，大致理由为全局传来传去，数量一多，后期不易维护。

如果一定要用，官方推荐使用mitt，体量只有200bite，当然也可以下载Vue2的事件总线去兼容使用。



安装

```shell
npm install --save mitt
```

main.js中全局挂载

```js
import mitt from "mitt"

const app = createApp(App)
const bus = mitt()
app.config.globalProperties.$bus = bus //相当于Vue2中的:Vue.prototype.$bus = bus

app.use(store).use(router).mount('#app')
```

组件A中发送

```vue
<template>
  <Son />
</template>

<script setup>
import Son from './Son.vue'
import { getCurrentInstance, onMounted } from 'vue'

const cxt = getCurrentInstance() //相当于Vue2中的this
const bus = cxt.appContext.config.globalProperties.$bus

onMounted(() => {
  bus.emit('sayHi', 'hi guy')
})

</script>
```

组件B中接收

```vue
<template>
  <h1>{{ data.msg }}</h1>
</template>

<script setup>
import { getCurrentInstance, onMounted, onBeforeUnmount, reactive } from 'vue'

const data = reactive({
  msg: ''
})

const cxt = getCurrentInstance()
const bus = cxt.appContext.config.globalProperties.$bus

onBeforeUnmount(() => {
  bus.off('sayHi')
})

onMounted(() => {
  bus.on('sayHi', (message) => {
    data.msg = message
  })
})

</script>
```



### toRaw

获取state里的原始数据

```vue
<script>
/* 
    state本质是一个Proxy对象，只是在这个对象中引用了obj，所以不等于obj
 */

import { ref, reactive, toRaw } from 'vue'

export default {
  name: 'App7',
  setup() {
    let obj = { a: 1, b: 2 }

    // let state = reactive(obj)
    let state = ref(obj)
    
    let obj2 = toRaw(state) // 获取state里的原始数据（实际并不是深拷贝，而且指向内存地址）
    // let obj2 = toRaw(state.value) // 拿到ref里的原始数据要加.value

    function myFn() {
      // console.log(obj) // {a: 1, b: 2}
      // console.log(state) // Proxy {a: 1, b: 2}
      // console.log(obj === state) // false

      obj2.a = 666 // 如果修改某条数据不需要界面发生更新，则可以使用toRaw来实现非响应式，提高性能
      console.log(obj2) // {a: 666, b: 2}
      console.log(obj2 === obj) // true
    }

    return { obj, state, myFn }
  }
}
</script>
```



### markRaw

设置markRaw不追踪，则无法修改里面的数据，用于写死数据

```vue
import { reactive, markRaw } from 'vue'

export default {
  name: 'App',
  setup() {
    let obj = { name: '胖迪', age: 14 }
    markRaw(obj) // 不追踪
    let state = reactive(obj)

    function myFn() {
      state.age += 1 // 不会发生任何改变
    }

    return { obj, state, myFn }
  }
}
```

### customRef

```vue
// customRef，返回一个自定义ref对象，可以显式地控制依赖追踪和触发反应
import { ref, customRef } from 'vue'

function myRef(val) {
  return customRef(((track, trigger) => {
    return {
      get() {
        track() // 告诉Vue该数据是需要追踪变化的
        console.log('get', val)
        return val
      },
      set(newVal) {
        console.log('set', newVal)
        val = newVal
        trigger() // 告诉Vue添加track的地方界面发生更新
      }
    }
  }))
}

export default {
  name: 'App11',
  setup() {
    let age = myRef(18)

    function myFn() {
      age.value += 1
    }

    return { age, myFn }
  }
}
```



### readonly、shallowReadonly、isReadonly

**html**

```vue
<template>
  <div>
    <h1>{{ state.name }}</h1>
    <h1>{{ state.attrbutions.age }}</h1>
    <h1>{{ state.attrbutions.height }}</h1>
    <button @click="myFn">BUTTON</button>
  </div>
</template>
```

**js**

```js
import { readonly, isReadonly, shallowReadonly } from 'vue'

export default {
  name: 'App13',
  setup() {
    // readonly创建只读数据，并且是递归只读（每一层都是只读）
    // let state = readonly(

    // shallowReadonly只有第一层只读
    let state = shallowReadonly(
      {
        name: '胖迪',
        attrbutions: {
          age: 14,
          height: 1.7
        }
      }
    )

    function myFn() {
      // readonly 递归只读每一层都不能修改
      // state.name = 'pp'
      // state.attrbutions.age += 1

      // shallowReadonly  只有第一层只读
      state.name = 'pp' // 第一层只读
      state.attrbutions.age += 1 // 第二层数据发生改变

      // isReadonly  检测是否为只读属性  检测shallowReadonly  
      console.log(isReadonly(state)) // true
      console.log(isReadonly(state.name)) // false
      console.log(isReadonly(state.attrbutions.age)) // false

      console.log(state)
    }

    return { state, myFn }
  }
}
```

