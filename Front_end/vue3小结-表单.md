## vue3获取表单中的值

### 获取表单中的值:

#### ref

```javascript
export default{
    methods: {
        doSubmit(){
           // <input type="text" ref="age" />
            console.log(this.$refs.age.value)
        }
    }
}
```

#### 双向绑定

```javascript
export default{
    data(){
        return {
            username: "张三"
        }
    }
    methods: {
        doSubmit(){
            // <input type="text" v-model="username" />
            console.log(this.username)
        }
    }
}
```

​      

## 父子组件通信

#### 父级组件调用子组件信息

```vue
<v-sub ref="child"></v-sub>
<!--
this.$refs.child.属性
this.$refs.child.方法
-->
```

#### 子组件调取父级组件信息

```vue
<!--
this.$parent.属性
this.$parent.方法
-->
```

#### 事件监听、自定义事件

```vue
<template>
  <v-sub @send="getChild"></v-sub>
</template>

<script>
import SubMod from "./SubMod.vue";

export default {
  name: "App",
  methods: {
    getChild(cxt) {
      console.log(cxt);
    },
  },
  components: {
    "v-sub": SubMod,
  },
};
</script>
```

```vue
<!-- 子模块：SubMod.vue -->
<template>
  <button @click="sendParent">按钮</button>
</template>

<script>
export default {
  methods:{
    sendParent(){
      this.$emit("send","hello parent")
    }
  }
}
</script>
```

## 父子组件双向绑定

父组件

```vue
<v-input v-model:keyword="内容"/>
```

子组件

```vue
<template>
<input type="text" :value="keyword"  @input="$emit('update:keyword', $event.target.value)"/>
</template>
<script>
export default {
    props: ["keyword"]
}
</script>
```

##  自定义Attribute继承

一个非 prop 的 attribute 是指向一个组件，但是该组件并没有相应 props 或 emits 定义的 attribute。常见的示例包括 class、style 和 id 属性。

**当组件返回单个根节点时，非 prop attribute 将自动添加到根节点的 attribute 中。**

```vue
<!-- 子组件 -->
<template>
	<!-- 自动继承父组件id属性、
	<button class="date-picker" id="time">    </button>
	-->
	<button class="date-picker">    </button>
</template>
```

```vue
<!-- 父组件 -->
<template>
<v-button id="time"></v-button>
</template>
```

**同样的规则适用于事件监听器：**

```vue
<v-button @change="submit"></v-button>
```

**如果不希望组件的根元素继承 attribute, 可以在组件中设置选项 inheritAttrs: false **。例如：

禁用 attribute 继承的常见情况是需要将 attribute 应用于根节点之外的其他元素。

**通过将 inheriAttrs 选项设置为 false,  你可以访问组件的 $attrs property, 该 property 包括组件 props 和 emits property 中未饮食的所有属性(例如, class、style、v-on监听器等)**

```vue
<template>
<div class="data-picker">
    <input type="date" v-bind="$attrs"/>
</div>
</template>
<script>
export default {
    inheritAttrs: false,
}
</script>
```

**与单个根节点不同，具有多个根节点的组件不具有自动 attribute 回退行为。如果未显示绑定 $attrs, 将发出运行警告。**



