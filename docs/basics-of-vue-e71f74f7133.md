# Vue 的基础知识

> 原文：<https://medium.com/swlh/basics-of-vue-e71f74f7133>

与 React 和 Angular 一样，Vue.js 是流行的 JavaScript 框架之一。在这篇文章中，我将描述如何启动一个 Vue 应用程序及其一些基础知识。

```
npm install -g @vue/cli
vue create vue-test
```

选择默认。

光盘放入其中:

```
cd vue-test
```

使用以下命令运行服务器:

```
npm run serve
```

将本地 url 粘贴到您的浏览器中。

打开“src”文件夹中的“App.vue”。您应该会看到类似这样的内容:

```
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script><style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

我们不需要底部的样式或顶部的图像，所以让我们把它们都去掉。

它现在应该看起来像这样:

```
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>
```

完美。

您在当前页面上看到的所有内容都来自 components 文件夹中的“HelloWorld.vue”文件。打开“HelloWorld.vue ”,删除底部的样式，以及模板内除了 h1 以外的所有内容。它现在应该看起来像这样:

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>
```

该页面现在是一个只有 h1 的空白页面，h1 包含从“App.vue”文件传入的“msg prop”的内容。{{}}允许您显示传入的道具的结果，在本例中是 msg 道具。

在“HelloWorld.vue”文件的底部，您可以找到以下内容:

```
<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>
```

这是允许“HelloWorld.vue”从 App.vue 接收字符串形式的“msg”属性所必需的。根据传入的属性名称及其数据类型，属性会有所不同。

您也可以传入更多的道具，只需将另一个带有值的变量放入<helloworld.vue>中，并在“HelloWorld.vue”底部的导出默认脚本中的“props”中添加道具名称及其数据类型即可。</helloworld.vue>

将“App.vue”更新为:

```
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"
    msg2="This is message 2!"/>
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>
```

和“HelloWorld.vue”到:

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    msg2: String
  }
}
</script>
```

现在将显示第二个标题“这是消息 2！”，我们传进去的新“msg2”道具。

您还可以将“数据”添加到“App.vue”的导出中，它返回可以传递和访问的键值对，并插入到<helloworld :variablename="“keyName”">中。这类似于 react 的道具。</helloworld>

```
<template>
 <div id="app">
   <HelloWorld :title="title" />
 </div>
</template>
<script>
import VueTest from './components/VueTest.vue'
export default {
 name: 'app',
 components: {
   VueTest
 },
 data() {
   return {
     title: "THIS IS A TITLE"
   }
 }
}
</script>
```

转到 HelloWorld.vue，添加一行显示标题道具。

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
    <h1>{{title}}</h1>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    msg2: String,
    title: String
  }
}
</script>
```

您可以将:variableName 命名为您想要的任何名称，尽管它必须被设置为与引号内的键的名称相同，并且 props 必须具有被传入的变量的名称及其数据类型。

有一些特殊的属性叫做以“v-bind”开头的指令。如果想让文本在悬停时出现，可以在 span 中使用 v-bind:title="keyName"。尽管 V-bind 是可选的，它也可以与 just :title="keyName "一起工作。

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
    <h1 v-bind:title="hover">{{title}}</h1>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    msg2: String,
    title: String,
    hover: String
  }
}
</script>
```

在 App.vue 中，在数据中添加悬停和相应的值。

```
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"
    msg2="This is message 2!"
    :title="title"
    :hover="hover"
    />
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data() {
   return {
     title: "THIS IS A TITLE",
     hover: "Hover Hand"
   }
 }
}
</script>
```

现在，如果你将鼠标悬停在标题上，“悬停手”就会出现。

为了使用函数，我们回到“App.vue ”,使用类似于我们使用组件的“方法”。

```
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"
    msg2="This is message 2!"
    :title="title"
    :hover="hover"
    :changeTitle="changeTitle"
    />
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data() {
   return {
     title: "THIS IS A TITLE",
     hover: "Hover Hand"
   }
 },
 methods: {
   changeTitle() {
     this.title = "Kappa"
   }
 },
}
</script>
```

可以通过 this.keyName 访问数据内部的属性。changeTitle 方法会将数据内部的 Title 属性更改为“Kappa”。

在“HelloWorld.vue”上添加一个调用 changeTitle 函数的按钮。我们使用一个点击时调用函数的指令。v-on:click="functionName "。你也可以只写@click="functionName "

```
<template>
  <div>
    <span v-bind:title="hover">{{title}}</span>
    <br />
    <button v-on:click="changeTitle">BUTTON!!!</button>
  </div>
</template><script>
export default {
  name: 'VueTest',
  props: {
    title: String,
    hover: String,
    changeTitle: Function
  }
}
</script>
```

这将在标题下面给你一个按钮，它有一个迷人的名字“按钮！！!"。点击它，你的头衔会变成 Kappa。

你可以在 v-if="keyName "中使用条件句。在“App.vue”中，将“hasTitle: true”添加到数据中，并添加方法“toggleTitle() {this.hasTitle =！this.hasTitle} "到方法。

```
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"
    msg2="This is message 2!"
    :title="title"
    :hover="hover"
    :changeTitle="changeTitle"
    :hasTitle="hasTitle"
    :toggleTitle="toggleTitle"
    />
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data() {
   return {
     title: "THIS IS A TITLE",
     hover: "Hover Hand",
     hasTitle: true,
   }
 },
 methods: {
   changeTitle() {
     this.title = "Kappa"
   },
   toggleTitle() {
     this.hasTitle = !this.hasTitle
   }
 },
}
</script>
```

返回到“HelloWorld.vue ”,添加一个标题，如果属性“hasTitle”为真，则只显示标题，并在该标题下添加一个按钮，单击该按钮将调用“toggleTitle”函数。

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
    <h1 v-bind:title="hover">{{title}}</h1>
    <br />
    <button v-on:click="changeTitle">BUTTON!!!</button>
    <br />
    <h1 v-if="hasTitle">{{title}}</h1>
    <br/>
    <button v-on:click="toggleTitle">TOGGLE BUTTON!!!</button>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    msg2: String,
    title: String,
    hover: String,
    hasTitle: String,
    changeTitle: Function,
    toggleTitle: Function
  }
}
</script>
```

现在有另一个标题和一个按钮，上面写着“切换按钮！！！在它下面。”如果我们单击切换按钮，第二个标题会消失，如果我们再次单击它，它会重新出现。

现在来看表格。vue 中的表单看起来像:

```
<form>
   <p>
      What is your name? <input>
   </p>
</form>
```

input 内部是您设置想要更改的变量的地方。

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
    <h1 v-bind:title="hover">{{title}}</h1>
    <br />
    <button v-on:click="changeTitle">BUTTON!!!</button>
    <br />
    <h1 v-if="hasTitle">{{title}}</h1>
    <br/>
    <button v-on:click="toggleTitle">TOGGLE BUTTON!!!</button>
    <form>
      <p>
        What is your name? <input v-model="title" />
      </p>
    </form>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    msg2: String,
    title: String,
    hover: String,
    hasTitle: String,
    changeTitle: Function,
    toggleTitle: Function
  }
}
</script>
```

这将把栏内的文本默认为数据中的“标题”值。更改此处的值将会更改“title”变量的值，上面的另外两个标题也会更改。

复选框输入看起来像:

```
<form>
   <p>
      <input v-model="hasTitle" type="checkbox />
   </p>
</form>
```

将它添加到索引表单中。

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
    <h1 v-bind:title="hover">{{title}}</h1>
    <br />
    <button v-on:click="changeTitle">BUTTON!!!</button>
    <br />
    <h1 v-if="hasTitle">{{title}}</h1>
    <br/>
    <button v-on:click="toggleTitle">TOGGLE BUTTON!!!</button>
    <form>
      <p>
        What is your name? <input v-model="title" />
        <input v-model="hasTitle" type="checkbox" />
      </p>
    </form>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    msg2: String,
    title: String,
    hover: String,
    hasTitle: String,
    changeTitle: Function,
    toggleTitle: Function
  }
}
</script>
```

这将创建一个与道具“hasTitle”链接的复选框。单击此按钮会将 hasTitle 的值从 true 更改为 false，反之亦然..

列清单做:

```
<select v-model="keyName">
   <option>
      OPTION 1
   </option>
   <option>
      OPTION 2
   </option>
   <option>
      OPTION 3
   </option>
</select>
```

在“App.vue”中，在数据下添加“选定的”:

```
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"
    msg2="This is message 2!"
    :title="title"
    :hover="hover"
    :changeTitle="changeTitle"
    :hasTitle="hasTitle"
    :toggleTitle="toggleTitle"
    :selected="selected"
    />
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data() {
   return {
     title: "THIS IS A TITLE",
     hover: "Hover Hand",
     hasTitle: true,
     selected: ''
   }
 },
 methods: {
   changeTitle() {
     this.title = "Kappa"
   },
   toggleTitle() {
     this.hasTitle = !this.hasTitle
   }
 },
}
</script>
```

添加选择栏和下面显示结果的标题，这将更新 select 的值。

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
    <h1 v-bind:title="hover">{{title}}</h1>
    <br />
    <button v-on:click="changeTitle">BUTTON!!!</button>
    <br />
    <h1 v-if="hasTitle">{{title}}</h1>
    <br/>
    <button v-on:click="toggleTitle">TOGGLE BUTTON!!!</button>
    <form>
      <p>
        What is your name? <input v-model="title" />
        <input v-model="hasTitle" type="checkbox" />
      </p>
    </form>
    <br/>
    <select v-model="selected">
      <option>
        OPTION 1
      </option>
      <option>
        OPTION 2
      </option>
      <option>
        OPTION 3
      </option>
    </select>
    <br/>
    <span>Selected: {{selected}}</span>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    msg2: String,
    title: String,
    hover: String,
    hasTitle: String,
    selected: String,
    changeTitle: Function,
    toggleTitle: Function
  }
}
</script>
```

这将为您留下一个有 3 个选项可供选择的栏，并更新下面的值以显示您选择的选项。

可以用“v-for”迭代一个循环。

在“App.vue”上的 data 下添加一个键，其值是一个包含若干项的数组。

```
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"
    msg2="This is message 2!"
    :title="title"
    :hover="hover"
    :changeTitle="changeTitle"
    :hasTitle="hasTitle"
    :toggleTitle="toggleTitle"
    :selected="selected"
    :array="array"
    />
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data() {
   return {
     title: "THIS IS A TITLE",
     hover: "Hover Hand",
     hasTitle: true,
     selected: '',
     array: [
     {name: 'item1'},
     {name: 'item2'},
     {name: 'item3'}
     ]
   }
 },
 methods: {
   changeTitle() {
     this.title = "Kappa"
   },
   toggleTitle() {
     this.hasTitle = !this.hasTitle
   }
 },
}
</script>
```

您可以将所有项目显示为列表。

```
<ul>
   <li vi-for="item in array">
      {{item.name}}
   </li>
</ul>
```

添加到“HelloWorld.vue”。

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
    <h1 v-bind:title="hover">{{title}}</h1>
    <br />
    <button v-on:click="changeTitle">BUTTON!!!</button>
    <br />
    <h1 v-if="hasTitle">{{title}}</h1>
    <br/>
    <button v-on:click="toggleTitle">TOGGLE BUTTON!!!</button>
    <form>
      <p>
        What is your name? <input v-model="title" />
        <input v-model="hasTitle" type="checkbox" />
      </p>
    </form>
    <br/>
    <select v-model="selected">
      <option>
        OPTION 1
      </option>
      <option>
        OPTION 2
      </option>
      <option>
        OPTION 3
      </option>
    </select>
    <br/>
    <span>Selected: {{selected}}</span>
    <ul>
      <li v-for="item in array">
        {{item.name}}
      </li>
    </ul>
  </div>
</template><script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    msg2: String,
    title: String,
    hover: String,
    hasTitle: String,
    selected: String,
    array: Array,
    changeTitle: Function,
    toggleTitle: Function
  }
}
</script>
```

现在这三项都显示为一个无序列表。

到目前为止，所有这些工作都是在单个组件上完成的。但是知道如何使用多个组件也很重要，因为它允许我们在想要再次显示某个内容或类似内容时重用一个组件。

在 components 文件夹中创建一个名为“NewComponent.vue”的新组件文件。打开它并输入以下行:

```
<template>
  <h1>I AM A NEW COMPONENT!</h1>
</template>
```

转到“App.vue”并检查脚本。默认情况下，当我们设置它时，它使用“import HelloWorld from”导入“HelloWorld.vue”。/components/HelloWorld.vue”行和“HelloWorld”。为“NewComponent.vue”添加一个导入行，并在“components”内的“HelloWorld”下添加“NewComponent”。同样，就像我们在模板的顶部有了<helloworld>，在它下面添加了<newcomponent>。您的“App.vue”现在应该是这样的:</newcomponent></helloworld>

```
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"
    msg2="This is message 2!"
    :title="title"
    :hover="hover"
    :changeTitle="changeTitle"
    :hasTitle="hasTitle"
    :toggleTitle="toggleTitle"
    :selected="selected"
    :array="array"
    />
    <NewComponent />
  </div>
</template><script>
import HelloWorld from './components/HelloWorld.vue'
import NewComponent from './components/NewComponent.vue'export default {
  name: 'app',
  components: {
    HelloWorld,
    NewComponent
  },
  data() {
   return {
     title: "THIS IS A TITLE",
     hover: "Hover Hand",
     hasTitle: true,
     selected: '',
     array: [
     {name: 'item1'},
     {name: 'item2'},
     {name: 'item3'}
     ]
   }
 },
 methods: {
   changeTitle() {
     this.title = "Kappa"
   },
   toggleTitle() {
     this.hasTitle = !this.hasTitle
   }
 },
}
</script>
```

如果您现在查看页面，您的新组件已经被添加到底部，就在“HelloWorld”组件下面。该页面现在显示“我是一个新组件！”在底部。

现在，您可以显示多个组件。但是如果您想在另一个组件中显示一个组件呢？比如，不是让“NewComponent”总是在“HelloWorld”组件的上方或下方，而是让它在“HelloWorld”组件的内部，也许在中间？

我们需要做的就是将它导入“HelloWorld.vue”本身，就像我们将它导入“App.vue”一样。在底部的脚本中，添加一行“import NewComponent from”。/NewComponent.vue ' "。请注意，它现在是从“”导入的。/NewComponent.vue "而不是"。/components/NewComponent.vue "。这是因为这里的这两个组件都在同一个文件夹中，您导入的东西 frmo 会根据它们相对于彼此的位置进行更改。还要在脚本标记中添加“components: {NewComponent}，即 name 和 props 之间的部分。你现在可以在“HelloWorld”中的任何地方使用<newcomponent>。让我们试着将它添加到显示“msg2”的第二个标题的正下方。</newcomponent>

```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>{{ msg2 }}</h1>
    <NewComponent />
    <h1 v-bind:title="hover">{{title}}</h1>
    <br />
    <button v-on:click="changeTitle">BUTTON!!!</button>
    <br />
    <h1 v-if="hasTitle">{{title}}</h1>
    <br/>
    <button v-on:click="toggleTitle">TOGGLE BUTTON!!!</button>
    <form>
      <p>
        What is your name? <input v-model="title" />
        <input v-model="hasTitle" type="checkbox" />
      </p>
    </form>
    <br/>
    <select v-model="selected">
      <option>
        OPTION 1
      </option>
      <option>
        OPTION 2
      </option>
      <option>
        OPTION 3
      </option>
    </select>
    <br/>
    <span>Selected: {{selected}}</span>
    <ul>
      <li v-for="item in array">
        {{item.name}}
      </li>
    </ul>
  </div>
</template><script>import NewComponent from './NewComponent.vue'export default {
  name: 'HelloWorld',
  components: {
    NewComponent
  },
  props: {
    msg: String,
    msg2: String,
    title: String,
    hover: String,
    hasTitle: String,
    selected: String,
    array: Array,
    changeTitle: Function,
    toggleTitle: Function
  }
}
</script>
```

您现在可以看到，之前添加到底部的同一行现在添加到了第二个标题的下方，我们已经成功地将“NewComponent”导入到了“HelloWorld”组件中！

最后一件事。我们之前忽略了“App.vue”底部的样式，但是我们需要回顾一下如何在 vue 中进行样式设置。非常简单，在“App.vue”的底部添加，然后像在普通的 css 表单中一样输入 css。

例如，转到“Helloworld.vue ”,向第一个 h1 添加一个 id，向第二个 h1 添加一个类。它应该看起来像这样:

```
<h1 id="id-1">{{ msg }}</h1>
<h1 class="class-1">{{ msg2 }}</h1>
```

现在在“App.vue”中给它们添加样式

```
<style>
#app {
  text-align: center;
  color: blue;
}#id-1 {
  color: red
}.class-1 {
  color: green
}
</style>
```

现在除了前两行，所有的文本都是蓝色的，前两行分别是红色和绿色。简单对吗？

本指南讲述了如何创建 Vue 项目，如何存储和传递数据和函数，如何显示和使用道具和方法来创建交互式页面，如何导入和使用多个组件，以及如何设计样式。感谢你的阅读，希望这能让你用正确的方式开始你的旅程！