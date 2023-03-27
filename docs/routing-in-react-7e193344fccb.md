# React 中的路由

> 原文：<https://medium.com/swlh/routing-in-react-7e193344fccb>

我将介绍如何在 react 中设置路由！

React 路由通过操纵窗口来工作。如果你打开控制台并输入“window.history ”,你会得到类似这样的结果:

```
History {length: 4, scrollRestoration: "auto", state: null}
```

您可以通过执行以下操作来更改状态:

```
window.history.pushState({newState: "This is a new state!"}, "new state", "new-state")
```

如果你再次输入“window.history ”,你会看到结果已经改变。您将状态设置为对象{newState:“这是一个新状态！”}，您将会看到在 url 的末尾添加了一个“新状态”。

如果您现在键入“window.history.back()”，url 将被更改为以前的形式，如果您键入“window.history”，状态也将被改回。如果你再次输入“window.history.back()”，你将返回整个页面。ReactRouter 是通过此方法手动操作 url 和状态的方式

现在我们先来设置一个新的 react app。

```
create-react-app (route-testing)
```

接下来启动并运行服务器。

```
npm start
```

现在安装 react-router-dom。

```
npm install react-router-dom
```

将“BrowserRouter”组件作为
Router”导入，并将“Route”组件导入到“index.js”中。另外，添加一个函数，并让路径呈现该函数。

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';
import {BrowserRouter as Router, Route} from 'react-router-dom'const Test = () => {
  return (
    <div>
    Test
    </div>
  )
}ReactDOM.render((
  <Router>
    <Route path="/" render={Test} />
  </Router>),
  document.getElementById('root')
);serviceWorker.unregister();
```

现在您将看到测试函数的内容。我们给了页面一个呈现 Test 的路径，因为我们现在就在这个路径上，所以它可以正确地呈现。

如果你去你的浏览器栏，在/后面添加随机的字母，它现在仍然可以工作。这是因为我们称它为“path ”,默认情况下会将所有内容呈现给该路径。如果我们想让它只在使用精确的单词时才呈现某些东西，我们使用“精确路径”。如果将该行改为:

```
<Route exact path="/" render={Test} />
```

你会注意到，当我们在它后面有随机字符时，它不再工作，但如果我们删除它们，并将其更改回 just /它将再次正确显示。

再来补充一些其他的路线和功能。

```
const One = () => {
  return (
    <div>
    ONE
    </div>
  )
}const Punch = () => {
  return (
    <div>
    Punch
    </div>
  )
}ReactDOM.render((
  <Router>
    <Route exact path="/" render={Test} />
    <Route exact path="/one" render={One} />
    <Route exact path="/punch" render={Punch} />
  </Router>),
  document.getElementById('root')
);
```

如果你现在在 url 中输入/one，它将显示单词 ONE under test。如果你用 punch 代替它，它将显示单词 punch under test。

现在，让我们添加一些单独的组件，并为它们添加这些路线。

创建两个文件，“Hi.js”和“Bye.js”。

在 hi 中输入以下代码:

```
import React from 'react';function Hi() {
  return (
    <div>
      Hi
    </div>
  );
}export default Hi;
```

下面是拜拜的内容:

```
import React from 'react';function Bye() {
  return (
    <div>
      Bye
    </div>
  );
}export default Bye;
```

将它们都导入“index.js”并添加到渲染中..

```
import Hi from './Hi';
import Bye from './Bye';ReactDOM.render((
  <Router>
    <Route path="/" render={Test} />
    <Route exact path="/one" render={One} />
    <Route exact path="/punch" render={Punch} />
    <Route exact path="/hi" component={Hi} />
    <Route exact path="/bye" component={Bye} />
  </Router>),
  document.getElementById('root')
);
```

对于组件，将单词 render=更改为 component=。尽管如此，它仍然可以像渲染一样工作。这里的“嗨”和“再见”的用法与“一”和“打孔”一模一样。

现在添加 NavLink，当我们点击不同的部分时，它会改变浏览器的 url。

导入导航链接:

```
import {BrowserRouter as Router, Route, NavLink} from 'react-router-dom'
```

创建一个导航栏，链接到不同的路径，并将其添加到渲染的顶部。

```
const Navbar = () => {
  return (
  <div>
    <NavLink to="/" exact >Test</NavLink>
    <NavLink to="/one" exact >ONE</NavLink>
    <NavLink to="/punch" exact >Punch</NavLink>
    <NavLink to="/hi" exact >Hi</NavLink>
    <NavLink to="/bye" exact >Bye</NavLink>
  </div>
  )
}ReactDOM.render((
  <Router>
    <Navbar />
    <Route path="/" render={Test} />
    <Route exact path="/one" render={One} />
    <Route exact path="/punch" render={Punch} />
    <Route exact path="/hi" component={Hi} />
    <Route exact path="/bye" component={Bye} />
  </Router>),
  document.getElementById('root')
);
```

现在你可以在任何地方看到链接列表，如果你点击它们，不仅页面会显示你点击的链接，而且顶部的 url 也会改变！

您还可以使用路线来包装其他东西，如按钮，单击按钮会产生相同的效果。

```
const Test = () => {
  return (
    <div>
    Test
    <NavLink to="/one" exact >
      <button >ONE</button>
    </NavLink>
    <NavLink to="/punch" exact >
      <button >PUNCH</button>
    </NavLink>
    <NavLink to="/hi" exact >
      <button >Hi</button>
    </NavLink>
    <NavLink to="/bye" exact >
      <button >Bye</button>
    </NavLink>
    </div>
  )
}
```

如果你给它们添加链接和按钮，现在你有一堆按钮，当按下时会改变显示！现在，您可以使用这些链接按钮来触发页面和 url 到不同组件的更改，而不是仅仅依靠更新状态来更改它！

最后一件事！如果你有一个像商店一样的东西，在商店区域内，你可以点击不同的东西，如“购买”或“出售”，这将改变商店内的一些东西，但外面的区域保持不变。在这种情况下，您并没有更改整个页面，只是更改了商店部分。这将是一个嵌套的链接，而不是从“购物”到“购买”，而是从“购物”到“购物/购买”。让我们设置组件。

再做两个文件。“Hi2.js”和“Hi3.js”。(这些真的是很有创意的名字吧？)

“Hi2”应该是这样的:

```
import React from 'react';function Hi2() {
  return (
    <div>
      Hi2
    </div>
  );
}export default Hi2;
```

“Hi3.js 应该是这样的:

```
import React from 'react';function Hi3() {
  return (
    <div>
      Hi3
    </div>
  );
}export default Hi3;
```

现在让我们通过导入 Hi 的 2 和 3 来更新我们原来的“Hi.js”文件。

```
import React from 'react';
import Hi2 from './Hi2';
import Hi3 from './Hi3';
import { Link, Route } from 'react-router-dom';function Hi(props) {
  return (
    <div>
      <Link to={`${props.match.url}/hi2`}>
        <button>H2 BUTTON</button>
      </Link>
      <br/>
      <Link to={`${props.match.url}/hi3`}>
        <button>H3 BUTTON</button>
      </Link> <Route path={`${props.match.url}/hi2`} component={Hi2} />
      <Route path={`${props.match.url}/hi3`} component={Hi3} />
    </div>
  );
}export default Hi;
```

我们正在创建两个按钮，并将它们包装在由 react-router-dom 提供的 Link 中，并在底部设置路由。传入的 props 包含 match.url 包含到目前为止将您带到页面的当前链接，因此您只需在之后添加嵌套部分。

现在，如果你点击其中一个按钮，它也应该显示“Hi2”或“Hi3”，这取决于你点击了哪一个按钮！

…

什么都没出现？

回想一下我们的 index.js。当我们为“Hi”组件设置路由时，我们键入:

```
<Route exact path="/hi" component={Hi} />
```

我们使用了精确的路径来创建它，所以如果你在“/hi”后面输入随机的 mumbo-jumbo，它就会失败。但是这实际上同样使得嵌套链接不起作用！将“精确路径”改为“路径”。现在一切都应该完全按照预期运行了！

这是 react 路由的基础！现在，这使您能够后退和前进，如果人们想要跳转到特定的页面，他们现在只需在 url 中键入他们想要的页面，而不必点击浏览，这大大提高了用户体验！祝你用你新学到的自定义路由知识制作应用程序好运！