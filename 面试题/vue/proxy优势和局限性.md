Vue3的响应式系统用Proxy实现，相比Vue2的defineProperty有哪些优势？在你们项目中是否遇到过Proxy的局限性？
  优势：defineProperty需递归遍历对象并劫持属性的getter/setter，无法自动检测新增和删除的属性；proxy直接代理整个对象，支持动态增删对象属性，深度监听深层对象的属性变化，无需初始化时递归遍历。
局限性：
  ①浏览器兼容性：降回vue2
  ②第三方库兼容性？：集成一个第三方状态管理库（假设类似于Redux或MobX的不可变数据流库）。该库为了确保状态不可变（immutable），在返回状态对象时会自动调用Object.freeze(阻止对对象的属性进行增删改操作)，将对象冻结。使用shallowRef跳过proxy代理。shallowRef仅追踪整个对象的引用变化，状态库每次更新返回全新的冻结对象时，替换shallowRef.value，触发更新。
  ③不支持基本数据类型：基本数据类型的变量被重新赋值后，原内存地址被丢弃，重新指向新的内存地址，proxy无法捕捉这种变化。使用ref包裹原始值。ref 通过包装对象（.value）和 getter/setter拦截实现响应式，仅在包裹对象时内部使用 Proxy。