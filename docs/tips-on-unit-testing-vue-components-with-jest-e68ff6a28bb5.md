# 用 Jest 对 Vue 组件进行单元测试的技巧

> 原文：<https://medium.com/swlh/tips-on-unit-testing-vue-components-with-jest-e68ff6a28bb5>

使用 Jest 对 Vue.js 组件进行单元测试可能会很棘手。我们需要一个单独的 [Vue Test Utils](https://vue-test-utils.vuejs.org) `(@vue/test-utils)`范围的包，以便虚拟地挂载我们的组件，并使用 [Jest](https://jestjs.io/docs/en/api) 来执行测试。因为这两个库一起工作，所以确保我们不会混淆哪个 API 调用属于哪个库是很重要的。除了这些库，我们还需要注意 Jest 附带的 [JSDOM](https://github.com/jsdom/jsdom) (虚拟浏览器环境)特定方法。不得不处理所有这些可能会令人困惑，并且会阻碍我们编写单元测试。

![](img/5f9178cc74dc4baf7e6e9256ceec59e4.png)

May feel like a juggling jest(er), Credit: [https://dribbble.com/fedecook](https://dribbble.com/fedecook)

例如，`shallowMount()`是创建浅层`wrapper`组件的 Vue 测试实用程序方法(使用浅层渲染来避免渲染子组件)，而`beforeEach()`是在每次测试之前执行回调参数的 Jest 方法。我们在`beforeEach()`内部运行`shallowMount()`，因此在每次测试之前都会安装一个组件。

我列出了我的团队用来成功地对我们的 Vue 组件进行单元测试的常用测试方法。希望这将有助于其他人经历类似的过程。

# 初始设置

![](img/8dade56360edb6c1342e92749f0e52b6.png)

让我们从建立 Jest 开始。将 Jest npm 包安装为`devDependencies`

```
$ npm i jest -D
```

现在让我们安装 Vue 测试工具和其他依赖项，如`babel-jest`、`vue-jest`等。(这直接来自 Vue 测试工具文档)

```
$ npm i @vue/test-utils vue-jest babel-jest -D
```

一旦我们设置好所有的包，在我们项目的根文件夹中创建一个 Jest 配置文件`jest.config.js`。作为替代，我们也可以将`module.exports`中的 JSON 对象添加到`jest: {}`到`package.json`的属性中，这减少了我们必须管理的配置文件的数量。

确保`.babelrc`文件具有安装了`babel-preset-env`模块的以下配置。

一旦我们设置了选项配置(主要是`collectCoverageFrom`源路径),我们就可以运行 Jest 了！

现在我们需要做的就是将`scripts: { “test”: “jest” }`添加到 package.json 中，然后运行这个脚本。

```
$ npm run test
```

# 编写测试文件

在我们有 Jest 可以运行的测试文件之前，上面的脚本不是很有用，所以让我们添加一个。

Jest 将在我们的项目文件夹中拾取`*.test.js`或`*.spec.js`文件。我喜欢将测试文件放在与我的 Vue 组件相同的文件夹中，并使用组件文件名。例如`button.vue`组件在`components/`文件夹中会有`button.test.js`测试文件。这有助于我管理组件文件和测试文件，因为它们位于同一个文件夹中，并且在 [VSCode](https://code.visualstudio.com/docs/getstarted/userinterface) 的`Explorer`中彼此相邻。

## 测试文件模板

这是我用来开始的一个基本模板测试文件。注意`shallowMount()`有像`propsData, mocks, stubs, methods`一样的各种属性，我们可以设置我们挂载的组件来模仿或存根组件的各种属性。通常的做法是在每次测试之前调用`shallowMount()`并将模拟组件存储在`wrapper`中，并在每次测试之后销毁`wrapper`。这样，我们每次测试都从一个新的状态开始，这使得测试更加可预测。

Jest 具有`describe()`、 `test()`和`expect()`测试功能来设置每个测试和资产期望值。我喜欢在开始时检查`wrapper.isVueInstance`测试通过，以确保我模拟了组件的默认属性。

Sample Jest test template

如果我们的组件使用 Vuex，我们需要使用`use()` 将 Vuex 添加到一个 Vue 实例(这里命名为`localVue`)中，并将该实例传递到`shallowMount()`。

Template with Vuex mocks

## 常用测试方法

![](img/146f18101baacbe3680e5afe5fbebd66.png)

Credit: [https://www.behance.net/taylorrafael](https://www.behance.net/taylorrafael)

让我们来看看一些涵盖大多数单元测试类型的测试方法。

*   **DOM 元素的存在** 我们通过检查`wrapper`是否拥有我们期望在组件挂载时呈现的所有默认元素来开始测试。我们通过检查实际的元素标签或其他属性(如类、id 等)来做到这一点。Jest 的断言函数如`toBe()`以及来自 Vue Test Utils 的`wrapper.contains()`或`wrapper.findAll().length`可以帮助这个测试。

我们还可以使用`wrapper.find()`返回的 DOM 元素的`.text()`函数来检查元素的`innerHTML`。
例如`expect(wrapper.find('.blah').text()).toBe('blah text')`。

*   **DOM action events** 我们可以测试一个动作在组件中执行时应该发出的事件。例如，当点击关闭按钮时，应该通过监听`close`事件来关闭对话框。我们使用 Vue Test Utils 中的`trigger()`函数来触发包装器所选 DOM 元素上的事件，并且`wrapper.emitted()`给出了一个发出事件的列表，用于检查所需事件的存在。

上面我们在`closeBtn` DOM 选择器上触发了`click`事件。我们还可以触发其他事件，如按键的`keydown`等。

*   **访问 Vue 包装器属性**
    如果我们需要访问或更改我们的 Vue 组件的`data`、`computed`、`methods`和`props`，我们可以使用`wrapper.vm`对象，因为 Vue Test Utils 在`wrapper`中创建了组件的一个实例。当我们必须模拟其中一个属性的值时，这很有帮助。

```
wrapper.vm.name = 'test'; // changes name data propertywrapper.vm.save(); // invokes save() methodwrapper.setProps({ propsName: newValue }); // changes propsName to newValue
```

一个例外是`computed`属性，它需要使用`shallowMount()`重新挂载并作为属性传入。因为我们在`beforeEach()`中安装了组件，所以我们可以将`shallowMount()`存储在一个工厂函数中，这个工厂函数可以在`beforeEach()`期间和测试中轻松调用，测试需要将`computed`属性的覆盖作为 getter 函数传递。

*   编写单元测试时需要知道的一件重要的事情是，如何伪造或模仿一个模块或方法的执行，它不是我们正在测试的单元的一部分。这有助于使我们的测试更加可预测，也避免了任何外部代码变更导致我们的测试失败。我们可以使用 Jest 的`fn()`和`mock()`函数来返回被嘲笑的版本。Vue Test Utils 中的`shallowMount()`内还有一个`mocks`属性，用于模拟我们的组件在没有 import 语句的情况下可能使用的任何全局函数。

我们来看看嘲讽函数。

Mocking methods using jest.fn()

上面的测试，我们用`jest.fn()`来模拟各种函数的返回值。我们可以在`fn(() => true)`内部传递一个参数，该参数将被模拟调用。如果留空，则返回一个`undefined`函数。

我们还模仿了`window.open()`全局函数，因为 JSDOM 没有提供，但是我们的测试需要执行它。

这些函数也是`spies`，它让我们窥探它们的行为，并使用`expect().toBeCalled()`函数来验证它们是否在测试中被调用。我们也可以使用`jest.spyOn(Object, function name)` Jest 函数来实现类似的行为。

其他有用的函数模拟包括`setInterval`和`setTimeout`，因为 Javascript 原生定时器并不像 Jest 中预期的那样工作。我们可以使用 Jest 的函数如`advanceTimersByTime(x)`或`runOnlyPendingTimers()`来清除所有的定时器，只要确保在测试文件的顶部声明`jest.userFakeTimers()`即可。

```
jest.useFakeTimers(); // Declare at top of test filejest.advanceTimersByTime(1000); // Inside test function if you have specific millisjest.runOnlyPendingTimers(); // Fast-forward until all pending timers have been executed
```

模仿导入的模块非常简单。

Mocking modules using jest.mock()

如果我们有一个`Constants`模块导入到我们的组件中，那么我们返回带有我们的组件使用的任何属性的模拟对象，例如`Constants.myConstant`。

*   **异步回调** 

# 演示

链接到[vue-jest-unit-test](https://github.com/achhunna/vue-jest-unit-test)repo I 设置，带有`App`组件及其测试。

# 结论

如果我们能很好地处理 Jest 和 Vue Test Utils APIs，那么为我们的 Vue 组件编写单元测试将会变得轻而易举，我们甚至会乐在其中！这很重要，因为对我们的组件进行单元测试有助于我们编写设计更好且易于重构的代码。