一直不太理解vue的生命周期，因为在实际项目中，只用过mounted这个钩子，其他并没有接触过。今天查阅了初雪日的[vue生命周期中，钩子函数执行顺序](https://blog.csdn.net/tionsu/article/details/78204107)，然后自己把这个知识点梳理了一下，初步了解了vue生命周期。
![vue生命周期](/img/vue生命周期的理解.png)
## beforeCreate
#### 官方讲解：
在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。
#### 我的理解：
创建之前，其实就是只实例化了一个vue，此时没有事件，没有data，没有this。其实不太建议在此时进行操作，防止页面空白太久。

## created
#### 官方讲解：
在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。
#### 我的理解：
此时有data,methods所有事件也已经加载完毕。此时还没有开始渲染页面，{{name}}此时还只是个占位符。watch/event开始工作了

## beforeMount
#### 官方讲解：
在挂载开始之前被调用：相关的 render 函数首次被调用。
**该钩子在服务器端渲染期间不被调用。**
#### 我的理解：
此时也还没有开始渲染页面，{{name}}此时还是占位符。此时还是不要进行过多的操作

## mounted
#### 官方讲解：
el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。

注意 mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted：
``` javascript
mounted: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
  })
}
```
**该钩子在服务器端渲染期间不被调用。**
#### 我的理解：
页面已经渲染好了，{{name}}绑定完毕，渲染成了值。如果这个时候，修改了name的值，相当于name又被渲染了一次。此时可以进行各种ajax操作，获取数据，渲染页面。

## beforeUpdate
#### 官方讲解：
数据更新时调用，发生在虚拟 DOM 打补丁之前。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。
**该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务端进行。**
#### 我的理解：
``` javascript
vm.name = '高级切图仔';
```
比如我在某个函数里面改变了name的值，在name变成“高级切图仔”前会调用改函数。在数据改变前做你想做的。

## update
#### 官方讲解：
由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。

注意 updated 不会承诺所有的子组件也都一起被重绘。如果你希望等到整个视图都重绘完毕，可以用 vm.$nextTick 替换掉 updated：
``` JavaScript
updated: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been re-rendered
  })
}
```
**该钩子在服务器端渲染期间不被调用。**
#### 我的理解：
``` javascript
vm.name = '高级切图仔';
```
嗯嗯～我的名字变成了“高级切图仔”，之后要干嘛？当然是爱干嘛干嘛。前提：不要修改data里面的任何数据，不然卡死你。

## activated
## 官方讲解：
keep-alive 组件激活时调用。
**该钩子在服务器端渲染期间不被调用。**
参考：
[构建组件 - keep-alive](https://cn.vuejs.org/v2/api/#keep-alive)
[动态组件 - keep-alive](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#%E5%9C%A8%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-keep-alive)

## deactivated
#### 官方讲解：
keep-alive 组件停用时调用。
**该钩子在服务器端渲染期间不被调用。**

## beforeDestroy
#### 官方讲解：
实例销毁之前调用。在这一步，实例仍然完全可用。
**该钩子在服务器端渲染期间不被调用。**
#### 我的理解：
我现在要切换路由了，在这里，你的所有组件，data，事件都会被销毁。不过在这些东西被销毁之前，让我把我想做的事情做完。

## destroyed
#### 官方讲解：
Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
**该钩子在服务器端渲染期间不被调用。**
#### 我的理解：
路由已经切换完了。什么东西都被销毁了。是时候处理“后事”了。