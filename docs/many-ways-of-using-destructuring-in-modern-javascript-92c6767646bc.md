# 在现代 JavaScript 中使用析构的许多方法

> 原文：<https://medium.com/swlh/many-ways-of-using-destructuring-in-modern-javascript-92c6767646bc>

![](img/a108b677dbff3091440f31b1ea8dde87.png)

毫无疑问，析构是我最喜欢的 JavaScript 语言特性之一。它的主要目的是*同时改进你的代码*和*简化它*。

# 那么，什么是解构？

根据定义，析构允许您将一个或多个属性从一个对象中提取到一个单独的变量中，但是范围会超出这个范围。析构也可以用于数组。它可以用在循环中，函数中，交换变量等等。
在这篇文章中，我将向你介绍使用这个强大功能的许多方法。

让我们首先创建一个简单的*用户*对象。

```
const user = { 
  fullName: 'Sam Fisher',
  age: 62,
  job: 'spy'
}
```

在析构之前，如果我们想创建一个变量并赋予它一个属性，我们会这样做，

```
const name = user.fullName;
console.log(name) // 'Sam Fisher';
```

其中变量*名称*指的是我们的*用户*对象的*全名*属性。现在让我们来看一下使用析构特性的同一个例子:

```
const { fullName } = user;
console.log(fullName) // 'Sam Fisher';
```

在这种情况下，变量 *fullName* 不仅引用了 *fullName* 属性，而且仅共享相同的名称。换句话说，当我们说

```
const { fullName } = user;
```

JavaScript 在名为*全名*的*用户*对象中寻找一个属性，并将其赋给一个名为*全名*的变量。很整洁对吗？现在让我们提取多个属性并比较结果。

```
**// Old way:** const fullName = user.fullName;
const age = user.age;
const job = user.job;
console.log(fullName, age, job); // 'Sam Fisher' 62 'spy'**// Destructuring way:** const { fullName, age, job } = user;
console.log(fullName, age, job); // 'Sam Fisher' 62 'spy'
```

我们可以清楚地看到，使用析构我们简化了代码，用一行代码替换了三行代码！

> 请记住，你对你析构的属性所做的任何改变只会影响你创建的变量，而对象内部的属性将保持不变。

我们也可以这样做

```
const { age, job, fullName } = user;
```

并且输出将保持不变:

```
console.log(fullName, age, job); // 'Sam Fisher' 62 'spy'
```

当处理对象时，属性的顺序并不重要。重要的是，我们要匹配要提取的每个属性的确切名称。

也就是说，析构不仅可以用于将变量记录到控制台。因为我们提取的每个属性都是一个变量，就像任何其他属性一样，它可以像任何其他属性一样使用:

```
const { age } = user;
if ( age < 100 ) {
  return 'Hello';
}
return 'World';
```

但是如果物业名称太长或者我们只是不喜欢这个名称呢？我们能做点什么吗？是的，我们可以做到这一点，

## 别名

当您从对象中提取属性时，您可以给它一个别名，然后使用别名来引用该属性。

```
const { fullName: fn } = user;
```

从这里开始，每当我们想要使用*全名*属性时，我们将使用 *fn* 别名。

```
console.log(fn); // 'Sam Fisher'
```

## 默认值

当析构属性时，我们可以将**默认值**添加到*未定义的属性*中。请考虑这个例子:

```
const user = {
  name: undefined
}
```

很明显，如果我们记录*用户名*，输出将是*未定义的*。在这种情况下，我们可以为 name 属性设置一个默认值。

```
const { name = 'Sam' } = user;
console.log(name); // Sam
```

现在我们知道了这是如何工作的，我们可以将这个逻辑应用于所有没有被定义的属性。

```
const user = {
  name: 'Sam',
  lastName: 'Fisher'  
}const { name, lastName, job = 'spy' } = user;
```

我们给了我们的*用户*对象两个属性，*名*和*姓*，但是*作业*属性不是*用户*对象的一部分，因此它是*未定义的*。这意味着**默认值**背后的逻辑可以应用于此。

> 在一条语句中，我们可以从用户对象中取出属性，放入它们自己的变量中，创建一个新变量 ***job*** ，并为其赋值。

```
console.log(name, lastName, job); // 'Sam' 'Fisher' 'spy'
```

![](img/859ab3225fe60959cd000ef6ba6edbb0.png)

# 命名出口

如果你曾经使用过 Angular、React 或任何其他 JavaScript 框架，毫无疑问你会遇到一个名为 Export 的**。就像析构一个对象一样，使用 *import* 关键字我们可以选择我们想要包含的模块导出:**

```
import React, { Component } from 'React';
```

这里我们将 React 导入到我们的项目中，但是我们也从 React 导入组件到它自己的名为 Component 的变量中。从现在开始，每当我们想要创建一个新的组件时，我们扩展 component，而不添加 React 前缀:

```
**// Old way** import React from 'React';
class MyComponent extends React.Component{}**// New way** import React, { Component } from 'React';
class MyComponent extends Component{}
```

同样的原则适用于我们导入的任何其他包，例如:

```
import { isEmail, isCreditCard } from 'validator';
```

这里我们只导入我们想要的函数，然后独立使用这些函数:

```
console.log(isEmail('my@email.com')); // true
```

![](img/af8f5ebbc7898d5be6a72c7ebaa80cce.png)

# 解构数组

如前所述，在 JavaScript 中我们也可以析构数组。概念几乎是相同的，但只是这一次我们使用了方括号和元素的顺序**问题**。
让我们创建一个基本数组:

```
const numbers = [ 10, 20, 30 ];// We can assign each element to a variable
const a = numbers[0];
const b = numbers[1];
const c = numbers[2];console.log(a); // 10
console.log(b); // 20
console.log(c); // 30
```

和之前的对象一样，使用析构，我们可以从数组中取出元素，并立即将它们存储到变量中。

```
const numbers = [ 10, 20, 30 ];
const [ a ] = numbers;console.log(a); // 10
```

通过用方括号将变量括起来，我们创建了一个类似数组的元素，其中变量 *a* 被放置为第 0 个索引，并引用 numbers 数组中的第**个**元素。
我们添加的每个新变量都将引用数组中的下一个元素:

```
const numbers = [ 10, 20, 30 ];
const [ a, b, c ] = numbers;console.log(a); // 10
console.log(b); // 20
console.log(c); // 30
```

我们也可以使用逗号来跳过元素:

```
const numbers = [ 10, 20, 30 ];
const [ a, , b] = numbers;
```

变量 *a* 是数字数组的第一个元素，但是 *b* 是第二个**而不是第三个****。 *b* 是第三个元素。中间的逗号代表被*跳过的*元素。因此:**

```
console.log(a); // 10
console.log(b); // 30
```

**使用**展开** / **剩余**操作符" **…** "我们可以将数组中剩余的*元素*作为目标:**

```
const numbers = [ 10, 20, 30 ];
const [ a, ...b ] = numbers;
```

**变量 *a* 将再次成为第一个元素。变量 *b* 有一个 **rest** 操作符前缀，这意味着 b 是一个填充了 numbers 数组中剩余元素的数组。**

```
console.log(a); // 10
console.log(b); // [ 20, 30 ];
```

**请记住，为了让这个操作生效， **rest** 操作符只能赋给我们正在析构的变量中的最后一个元素。**

****默认值****

**与对象非常相似，我们可以给所有*未定义的*数组元素默认值。**

```
const arr = [ undefined, undefined, 3 ];
const [ first = 1, second = 2, third ] = arr;console.log(first); // 1
console.log(second); // 2
console.log(third); // 3
```

**这同样适用于不属于数组的元素:**

```
const nums = [ 1, 2, 3 ];
const [ a, b, c, d = 5 ] = nums;console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
console.log(d); // 5
```

**变量 *d* 不是 *nums* 数组的一部分，因此它的值是*未定义的*，我们可以再次应用**默认值**背后的逻辑，创建一个新变量并为其赋值(在本例中为 5)。**

**一个未能析构数组的特性是**别名**。这并不奇怪，因为从数组中提取元素时，我们可以随意命名变量。**

**![](img/aae35c5807cd844a4978a7500af75544.png)**

# **析构函数参数**

**曾经遇到过需要传递给函数的一长串参数的问题吗？我们不再多说，因为这是一个游戏规则的改变者。**

```
// **Old way**
const username = 'SamFisher';
const email ='sam.fisher@thirdechelon.com';
const password ='UqFkD8kCpW';
const age = 62;
const job = 'spy';funky(username, email, password, age, job);// **New way**
const user = {
  username: 'SamFisher',
  email: 'sam.fisher@thirdechelon.com',
  password: 'UqFkD8kCpW',
  age: 62,
  job: 'spy'
};funky(user);// **Alternative**
funky(**{** 
  username: 'SamFisher',
  email: 'sam.fisher@thirdechelon.com' ,
  password: 'UqFkD8kCpW',
  age: 62,
  job: 'spy'
**}**);
```

**我们不是将一长串属性传递给函数，而是创建一个包含这些属性的对象，然后将该对象作为参数传递给函数。然后我们只拉出**我们想要使用的**属性。**

```
// **Old way**
function funky(username, email, password, age, job) {
  console.log(email, password);
}// **New way**
function funky(**{** email: e, password: pw **}**) {
  console.log(e, pw);
}
```

**如上所述，JavaScript 在我们作为参数传递的对象中寻找属性 *email* 和*密码*，所以顺序**也无关紧要**。添加别名只是进一步简化了事情。**

**这个概念也适用于数组。这个想法是将一个数组传递给我们正在调用的函数，然后只提取我们想要的元素。让我们先看看在析构之前这是如何处理的:**

```
const people = ['Batman', 'Superman', 'Arrow'];function getPerson( arrayOfPeople ) {
  console.log(arrayOfPeople[0]); // 'Batman'
  console.log(arrayOfPeople[1]); // 'Superman'
  console.log(arrayOfPeople[2]); // 'Arrow'
}getPerson(people);
```

**如前所述，析构使我们能够更有效地操纵数据，这就是我们如何将它应用到前面的例子中:**

```
const people = ['Batman', 'Superman', 'Arrow'];function getPerson( [ a, b, c ] ) {
  console.log(a); // 'Batman'
  console.log(b); // 'Superman'
  console.log(c); // 'Arrow'
}getPerson(people);
```

**数组参数中的每个元素都代表该索引处的 *people* 数组中的一个元素。我们可以再次使用 **rest** 操作符来定位数组的其余部分:**

```
function getPerson( [ a, **...**b ] ) {
  console.log(a); // 'Batman'
  console.log(b); // [ 'Superman', 'Arrow' ]
}getPerson(people);
```

**![](img/2e51b54f54f43bf6e93408206cc604a3.png)**

# **析构返回语句**

**就像函数参数一样，我们可以析构函数返回的值。这可以应用于返回对象或数组的函数。
**情况 1** :函数返回一个对象**

```
function getUserDetails() {
  return { name: 'Sam Fisher', age: 62, job: 'spy' }
}
const { name } = getUserDetails();console.log(name); // 'Sam Fisher'
```

****情况 2** :函数返回一个数组**

```
function getAvengers() {
  return ['Ironman', 'Captain America', 'Thor', 'Hulk']
}const [ a, , , b ] = getAvengers();// Notice that I created two commas in the middle. This means that I'm skipping both Captain America and Thorconsole.log(a); // 'Ironman'
console.log(b); // 'Hulk'
```

**就这么简单。**

**![](img/449b6aa5cedad418b35a0c8cd6df2dee.png)**

# **循环内部的析构**

**到目前为止，我们已经看到了析构的许多用途，但是我们还没有触及的一个主题是循环。让我们首先创建一个数组，然后使用 *for of* 循环来迭代该数组。**

```
const numbers = [ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ];for (let num of numbers) {
  console.log(num);
}
// [ 1, 2, 3 ]
// [ 4, 5, 6 ]
// [ 7, 8, 9 ]
```

**对于 *numbers* 数组中的每个内部数组，我们打印该数组的迭代。现在让我们应用析构并从每次迭代中取出元素。然后我们得到这样的结果:**

```
const numbers = [ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ];for (let [ a, b, c ] of numbers) {
  console.log(a, b, c);
}
// 1 2 3
// 4 5 6
// 7 8 9
```

**很酷吧？
这里是另一个结合**默认值**的例子。**

```
const simpsons = [
  { name: 'Homer', age: 36 },
  { name: 'Marge', age: 34},
  { name: 'Bart' },
  { name: 'Lisa', age: 7 },
  { name: 'Maggie' }
];
```

**很明显，有些对象没有*年龄*属性。我们可以遍历所有对象，而不是为每个没有*年龄*属性的用户返回 undefined，我们可以给这些用户一个*年龄*，默认值为 0。**

```
for (let { name, age = 0 } of simpsons ) {
  console.log(name, age);
}// 'Homer' 36
// 'Marge' 34
// 'Bart' 0
// 'Lisa' 7
// 'Maggie' 0
```

**![](img/f045e551fafd842b1bd78bf5e9fce28b.png)**

# **析构嵌套对象和数组**

**析构简化了我们编写 JavaScript 的方式，但这并不意味着析构就意味着简单的用途。**

****解构嵌套对象****

**考虑一下这个假的气象 API 数据:**

```
const weatherData = {
  city: 'London',
  temperature: {
    minimum: 10,
    expected: 30,
    current: 25
  }
}
```

**考虑构建一个天气应用程序，并使用以前的 API 向用户呈现当前温度。你可以写下
*weather data . temperature . current*，但是你可以应用析构。**

**让我们一步一步解开这个 API 数据。
我们首先拉出一个*温度*对象，并将其分配给其父对象*天气数据*:**

```
const { temperature } = weatherData;
```

**然后，在这两个属性之间放置另一个属性或一组我们希望取出的属性，并用另一对花括号将它们括起来。
然后我们得到看起来像这样的东西，**

```
const { temperature: { current } } = weatherData;
```

**其中*当前*是*温度*对象的属性，*温度*是*天气数据*对象的属性。现在我们已经创建了一个名为 *current* 的变量，我们可以随心所欲地使用它。**

```
console.log(current); // output: 25
```

**我们也可以在这里使用一个**别名**。**

```
const { temperature: { current: c, expected: e } } = weatherData;
console.log(c); // output: 25
console.log(e); output: 30
```

**如果属性有一个*未定义的*值，我们可以给它一个**缺省值**或者如果它甚至不存在于我们的对象中，我们可以像一个析构变量一样创建它，**

```
const { temperature: { likelyNot: LN = 9999 } } = weatherData;
console.log(LN); // output: 9999
```

**其中 *LN* 是 *likelyNot* 的别名。**

**同样的概念可以应用于嵌套更深的对象。**

```
const weatherData = {
  city: 'London',
  temperatureCelsius: {
    minimum: 10,
    expected: 30,
    current: 25
  },
  temperatureFahrenheit: {
    minimum: 50,
    expected: 86,
    current: 77
  }
}
```

**现在让我们从两个物体上获得当前温度。**

```
oonst { 
  temperatureCelsius: { current: currentC },
  temperatureFahrenheit: { current: currentF }
} = weatherData;console.log(currentC); // output: 25
console.log(currentF); // output: 77
```

**请注意，我在这里使用别名是为了避免两个不同的温度变量同名的冲突。**

****析构嵌套数组****

**嵌套数组在 JavaScript 中很常见，使用现代 JavaScript 已经大大简化了对它们的处理。
把这个英雄阵列算一算:**

```
const heroes = [
 [ 'Spiderman', 'Iron Man', 'Captain America' ],
 ['Batman', 'Superman', 'Wonder Woman']
]
```

**让我们将*漫威*和 *DC* 英雄提取到两个独立的数组中:**

```
const [ marvel, dc ] = heroes;console.log(marvel); 
// output: [ 'Spiderman', 'Iron Man', 'Captain America' ]console.log(dc); 
// output: ['Batman', 'Superman', 'Wonder Woman']
```

**现在让我们把这个例子向前推进一步，在我们的 *heroes* 数组中添加另一个数组。在我们的内部数组中，我们将定义一个属性，在这种情况下，它将引用第一个内部数组的第一个元素。**

```
const [ [ spider ] ] = heroes;
console.log(spider); // output: 'Spiderman'
```

**就像之前我们可以用逗号来跳过数组元素一样。让我们从第一个数组中提取最后一个元素。**

```
const [ [ , , captain ] ] = heroes
console.log(captain); // output: 'Captain America'
```

**如果我们在我们的外部(*英雄*)数组中添加另一个内部数组，用逗号与第一个内部数组(*漫威*)隔开，它将引用一个 *DC 英雄*的数组。从那里我们可以从 *DC* 阵列中抽取一个或多个英雄。**

```
const [ [ , , captain ], [ bat ] ] = heroes
console.log(captain); // output: 'Captain America'
console.log(bat); // output: 'Batman'
```

**解构嵌套数组的另一种方法是使用 ES9 [**Array.flat()**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) 函数，其目的是*将嵌套数组平坦化*为普通数组。**

```
const flatten = heroes.flat();
console.log(flatten);
// output: [ 'Spiderman', 'Iron Man', 'Captain America', 'Batman', 'Superman', 'Wonder Woman']
```

**然后我们可以像选择普通队伍一样选择我们的英雄:**

```
const [ spider, , captain, bat] = flatten;console.log(spider); // output: 'Spiderman'
console.log(captain); // output: 'Captain America'
console.log(bat); // output: 'Batman'
```

****析构嵌套函数参数****

**假设我们想要处理一个 HTML 元素的变化事件，并且我们想要获取一个我们输入的值。**

```
<input type="text" onchange="handleChange(event)"/>
```

**使用嵌套析构，我们可以从一个*事件*对象中提取一个*目标*对象，然后从目标中提取一个*值*属性:**

```
**# 1**
function handleChange(event) {
  const { value } = event.target;
  console.log(value);
}**# 2**
function handleChange({ target }) {
  const { value } = target;
  console.log(value);
}**# 3**
function handleChange({ target: { value } }) {
  console.log(value);
}
```

**嵌套数组也是如此。**

```
const nestedArray = [ [ 'A', 'B', 'C' ], [ 'D', 'E', 'F' ] ];
handleNestedArray(nestedArray);**# 1**
function handleNestedArray(full) {
console.log(full); // output: [ ['A', 'B', 'C'], ['D', 'E', 'F'] ]
}**# 2**
function handleNestedArray([ firstArray ]) {
console.log(firstArray); // output: ['A', 'B', 'C']
}**# 3**
function handleNestedArray([ [], [ , letterE ] ]) {
console.log(letterE); // output: 'E'
}
```

****析构嵌套返回语句****

**如果你正在处理一个返回嵌套数组或对象的函数，你可以利用析构只提取你需要的属性。**

```
function handleObject() { 
  return { target: { value: 'Hello' } };
}**# 1**
const result = handleObject();
console.log(result); // output: { target: { value: 'Hello' } }**# 2**
const { target } = handleObject();
console.log(target); // output: { value: 'Hello' }**# 3**
const { target: { value } } = handleObject();
console.log(value); // output: 'Hello'
```

**这也适用于数组。**

```
function handleArray() {
  return [ [ 1, 2], [3, 4] ];
}**# 1**
const fullArray = handleArray();
console.log(fullArray); // output: [ [ 1, 2], [3, 4] ]**# 2**
const [ first ] = handleArray();
console.log(first); // output: [1, 2]**# 3**
const [ [ one, two ] ] = handleArray();
console.log(one, two); // output: 1, 2
```

**![](img/498cf0f5bbaf9bbe9179bd0c71c5ecd3.png)**

# **用析构交换变量**

**使用析构赋值，我们可以做到这一点，交换变量！然而，这个**并不仅限于**数字。我们可以交换任何类型，两个字符串，两个数组，两个对象，等等。在某些情况下，我们甚至可以交换不同类型的变量。**

```
**// Let's declare two variables** let hero = 'Mario';
let villain = 'Bowser';
console.log(hero, villain); **// 'Mario' 'Bowser'****// Now let's swap them** [ hero, villain ] = [ villain, hero ];console.log(hero, villain); **// 'Bowser' 'Mario'**
```

**我们将两个变量都放在方括号中，并给它们分配另一对包装的变量，只是第二次交换位置。这导致变量转换值，突然布瑟成为英雄，马里奥成为恶棍！**

# **日常使用中的破坏**

**当我写代码时，我倾向于尽可能使用析构。
你知道你可以析构一个 *console.log* 吗？**

```
const { log, warn, error } = console;log('Hello World!'); **// equivalent to console.log('...');**
warn('Watch out!'); **// console.warn('...');**
error('Something went wrong!'); **// console.error('...');**
```

**因为 console 是一个对象，所以你可以像对其他对象一样析构它的属性。
但是你也可以析构 **HTML 元素**的属性。**

```
const { value } = document.querySelector('input');
```

**这样，从输入中提取一个*值*属性就容易多了，就像从按钮中提取一个禁用的属性一样。**

```
const { disabled } = document.querySelector('button');
```

**这同样适用于所有其他属性。**

# **包扎**

**我希望您在阅读这篇文章时感到愉快，并对现代 JavaScript 的这一惊人特性有所了解。如果你有兴趣了解更多，有一个 [**要点**](https://gist.github.com/mikaelbr/9900818) 充满了这些例子，作者是 [Mikael Brevik](https://medium.com/u/d2e9832e3bd5?source=post_page-----92c6767646bc--------------------------------) ，如果你有任何问题，请在下面的评论中告诉我。**