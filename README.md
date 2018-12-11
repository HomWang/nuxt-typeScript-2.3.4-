# 说明
  nuxt-property-decorator 该库完全依赖于 vue-class-component
  在这里感谢两个的插件维护者

## 用法

6个装饰器:

* `@Inject`
* `@Model`
* `@Prop`
* `@Provide`
* `@Watch`
* `@Component` (**exported from** `vue-class-component`)

```typescript
import { Component, Inject, Model, Prop, Vue, Watch } from 'nuxt-property-decorator'

const s = Symbol('baz')

@Component({
  components: { comp }
})
export class MyComponent extends Vue {

  @Inject() foo!: string
  @Inject('bar') bar!: string
  @Inject(s) baz!: string

  @Model('change') checked!: boolean

  @Prop()
  propA!: number

  @Prop({ default: 'default value' })
  propB!: string

  @Prop([String, Boolean])
  propC!: string | boolean

  @Prop({ type: null })
  propD!: any

  @Provide() foo = 'foo'
  @Provide('bar') baz = 'bar'

  @Watch('child')
  onChildChanged(val: string, oldVal: string) { }

  @Watch('person', { immediate: true, deep: true })
  onPersonChanged(val: Person, oldVal: Person) { }
}

```
相当于

```js
const s = Symbol('baz')

export const MyComponent = Vue.extend({
  name: 'MyComponent',
  components: { comp },
  inject: {
    foo: 'foo',
    bar: 'bar',
    [s]: s
  },
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean,
    propA: Number,
    propB: {
      type: String,
      default: 'default value'
    },
    propC: [String, Boolean],
    propD: { type: null }
  },
  data () {
    return {
      foo: 'foo',
      baz: 'bar'
    }
  },
  provide () {
    return {
      foo: this.foo,
      bar: this.baz
    }
  }
  methods: {
    onChildChanged(val, oldVal) { },
    onPersonChanged(val, oldVal) { }
  },
  watch: {
    'child': {
      handler: 'onChildChanged',
      immediate: false,
      deep: false
    },
    'person': {
      handler: 'onPersonChanged',
      immediate: true,
      deep: true
    }
  }
})
```

正如你可以看到在propA和propB时，它是一个简单类型的类型可以推断。对于更复杂的类型，如枚举，您需要专门指定它。此库也需要emitDecoratorMetadata设置true为此工作。

## 参考链接

可以看看:
* [Vue Property Decorator](https://github.com/kaorun343/vue-property-decorator)
* [Vue class component](https://github.com/vuejs/vue-class-component)
* [Vuex Class](https://github.com/ktsn/vuex-class/)
* [Nuxt Class Component](https://github.com/nuxt-community/nuxt-class-component)

# NuxtTS

> My super Nuxt.js project

## Build Setup

``` bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn run dev

# build for production and launch server
$ yarn run build
$ yarn start

# generate static project
$ yarn run generate
```

For detailed explanation on how things work, checkout [Nuxt.js docs](https://nuxtjs.org).
