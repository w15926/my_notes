# vite（试取代webpack工具）

### install

```shell
npm i -g create-vite-app
```

### create Vue3 project

```shell
create-vite-app projectName
```

### use

```shell
cd projectName
npm i
npm run dev
```



# 生命周期

- `beforeCreate` -> use `setup()`
- `created` -> use `setup()`
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

### ref、reactive（响应式监听）

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
> > > state.value.gf.f.s.d = '4' *// 修改第四层数据，此时非递归、非响应式*
> > >
> > > triggerRef(state) *// 让state修修改的这一层触发ref，实现响应式*

```js
// ref监听简单类型   reactive监听复杂类型（对象/数组）
// reactive如果监听其他类型，比如 new Date()，不能实现页面动态刷新，只能通过重新赋值方式
import { ref, reactive } from 'vue'

let count = ref(0)

let state = reactive({
  students: [
    ...
  ] 
})
```



#### example-one

```vue
<template>
  <div>
    <h1>{{ count }}</h1>
    <button @click="myFn">add</button>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'App',
  setup() {
    let count = ref(0) // 变量发生改变ref会自动刷新里面的值

    function myFn() {
      console.log(count)
      count.value += 1 // 通过这种方法更新里面的值，但是必须配合ref
    }

    return { count, myFn } // 外界要想使用必须通过return暴露出去
  }
}
</script>
```



#### example-two

直接实现模块化，DOM里只能使用setup里的return，其他函数的return都是为了给setup使用

```js
import useRemoveStudent from './app2-methods/remove'
import useAddStudent from './app2-methods/add'

export default {
  name: 'App2',
  setup() {
    let { state, removeStudent } = useRemoveStudent() // 解构调用，直接实现模块化
    let { state2, addStudent } = useAddStudent(state) // 可以用传参方法调用上一行方法内的属性

    return { state, removeStudent, state2, addStudent }
  }
}
```



### computed

**计算属性，数值发生变化时自动调用**

```vue
    <input type="text" v-model="state.num1">
    <span>+</span>
    <input type="text" v-model="state.num2">
    <span>=</span>
    <input type="text" v-model="state.result">
    {{state.result}}
```

```js
import { reactive, computed } from 'vue'
```

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



### watch

监听单个值

```js
setup() {
    let a = ref(1)
    let b = ref(2)

    // 默认监听所有数据对变化，写哪个值就监听哪一个
    // watch(() => {
    //   console.log(a.value + '--------' + b.value)
    // })

    // 指定监听某个值变化,当值发生改变时才触发
    // watch(a, () => {
    //   console.log('----------')
    // }, { immediate: true }) // 加入immediate会打开界面默认触发一次

    // 可以监听新值与旧值
    // watch(a, (newA, oldA) => {
    //   console.log(newA + '----------' + oldA)
    // })

    // 监听多个值
    // watch([a, b], ([newA, newB], [oldA, oldB])=> {
    //   console.log(newA + '----------' + oldA)
    //   console.log(newB + '==========' + oldB)
    // })

    return {
      a,
      b
    }
  }
```

监听对象

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





### 父子组件传参（父传子不变）

**子传父 - son**

```vue
<button @click="emitFn">子传父</button>
```

```js
  // setup(props, ctx) { // ctx为上下文，ctx.emit
  setup(props, { emit }) { // es6中可以直接解构使用
    const state = reactive({
      num1: 0,
      num2: 0,
      result: computed(() => parseInt(state.num1) + parseInt(state.num2))
    })

    let emitFn = () => {
      emit('sendMsg', state.result) // 自定义事件名，实参
    }

    return { state, emitFn }
  }
```

**子传父 - father**

```vue
<HelloWorld @sendMsg="handler" />
```

```js
  setup() {
    const handler = value => {
      console.log(value);
    }

    return { handler }
  }
```



### toRaw

获取state里的原始数据

```js
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

设置markRaw不追踪，贼无法修改里面的数据，用于写死数据

```js
import { reactive, markRaw } from 'vue'

export default {
  name: 'App7',
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



### toRef

```js
<script>
/* 
  ref和toRef区别：
  ref -> 复制  修改响应式数据不会影响以前的数据
  toRef -> 引用  修改响应式数据会影响以前的数据
  
  ref -> 数据发生改变    界面自动更新
  toRef -> 数据发生改变  界面不会自动更新
 */

import { toRef } from 'vue'

export default {
  name: 'App9ww',
  setup() {
    let obj = { name: '胖迪', age: 14 }
    let state = toRef(obj, 'age') // 引用 obj 里的 age 属性

    function myFn() {
      state.value += 1 // age +1
      console.log(obj) // 愿数据改变（界面不更新）
      console.log(state.value) // 响应式数据改变（界面不更新）
    }

    return { obj, state, myFn }
  }
}
</script>
```



### toRefs

```js
<script>
// toRefs相当于toRef的简写
import { toRef, toRefs } from 'vue'

export default {
  name: 'App10',
  setup() {
    let obj = { name: '胖迪', age: 14 }

    let state = toRefs(obj)
    // 相当于这两行代码
    // let name = toRef(obj, 'name')
    // let age = toRef(obj, 'age')

    function myFn() {
      state.name.value = 'pp'
      state.age.value += 1
      console.log(obj)
      console.log(state)
    }

    return { obj, state, myFn }
  }
}
</script>
```



### customRef

```js
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



### ref获取标签元素、onMounted

  *在Vue2中可以给标签添加 ref='xxx' ，然后代码中通过this.$refs.xxx获取值*

  *Vue3中setup没有this和$，通过下面方法获取* 

**html**

```vue
<template>
  <div>
    <div ref="box">div</div>
    <button @click="myFn">BUTTON</button>
  </div>
</template>
```

**js**

```js
// onMounted等价于生命周期中的mounted
import { ref, onMounted } from 'vue'

export default {
  name: 'App11',
  setup() {
    let box = ref(null) // reactive({value:null})
    console.log(box.value)

    onMounted(_ => console.log('onMounted', box.value)) // DOM渲染完毕后拿到值 <div>div</div>

    function myFn() {
      console.log(box.value) // 因为DOM渲染完毕后才会加载按钮，其实这里在onMounted里执行 <div>div</div>
    }

    return { box, myFn }
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

