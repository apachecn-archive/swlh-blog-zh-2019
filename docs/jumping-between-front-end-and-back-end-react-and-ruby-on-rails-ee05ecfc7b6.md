# 在前端和后端之间跳转(React 和 Ruby on Rails)

> 原文：<https://medium.com/swlh/jumping-between-front-end-and-back-end-react-and-ruby-on-rails-ee05ecfc7b6>

# 背景

作为一个 blooming 全栈开发人员，需要对前端代码和后端代码有一个透彻的理解。对于我的例子，前端代码是 React，一个基于 Javascript 的框架，后端代码是 Ruby on Rails，一个基于 Ruby 的框架。虽然理解我正在使用的编程语言/框架很重要，但我也需要完全理解前端如何与后端代码通信。不然我真的是全栈开发者吗？

# React(和普通 Javascript)

React 是前端代码的框架，这意味着它直接与客户端交互。客户端可能想要输入来自后端服务器的新信息，通常是一个 API，这意味着在我们前端代码的某个地方，我需要向后端服务器发出请求，以给出客户端发布的特定信息。在 React 中，幸运的是在普通 Javascript 中，实现这一点的方法是一个 fetch 请求。这里的链接[将提供获取请求的完整文档。以下代码是上述请求的一个示例:](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

第一个参数是我发出请求的 URL，而第二个参数是我发出的请求的类型。在 headers hash 中，对于“Content-Type”，我发送的数据类型是 JSON 格式。主体散列是我发送的数据本身。对于 JSON.stringify()，这个[链接](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)解释了这个函数的作用。现在我知道我发送的数据本身以及它的格式。接下来的问题是，后端服务器会接受这个吗？

# Ruby on Rails

在大多数情况下，如果您使用现有的服务器，应该有一个 README 文件，指导开发人员如何将特定数据和数据本身格式化为后端服务器通常接受的格式。对于这个例子，因为我用 Ruby on Rails 从头开始创建了一个 API，所以我可以自由选择要接收的数据类型，假设 Ruby on Rails 有这个能力。到目前为止，我见过的最常见的格式是 JSON 格式。为了简单起见，我遵循了 RESTful routes 约定，在这里可以更详细地解释[。](https://guides.rubyonrails.org/routing.html)

在上面的 fetch 示例中，在第一个参数的末尾，它以“/students”结尾，而第二个参数中的方法(通常也称为 HTTP 动词)是“POST”。基于 RESTful 路由，这用于创建一个新学生(如客户端所期望的)。然后，这个 RESTful 路径将进入我的学生控制器，并进入“创建”操作，如下所示:

现在，信息(名字:“John”，姓氏:“Smith”)被传递到我的后端服务器，下一步是在后端服务器中创建和持久存储这些数据，如下所示:

您可以假设一个学生模型已经存在并且运行正常。

信息被传递到后端服务器并存储在 params 散列中。对于这篇博文的相关性，您只需要知道 params hash 内置于 Ruby on Rails 中，它充当将任何输入信息传递给 Rails 控制器的媒介。

在第 4 行中，创建了一个新学生，并将其保存在 API 中，存储为变量@student。在第 5 行中,@student 变量被呈现为 JSON 格式。您可以将此视为来自后端服务器的“响应”。尽管第 5 行无论如何都不是必需的，但是提供某种“响应”作为创建成功(或失败)的确认通常是一个好的实践。这种“响应”现在可以在前端进行操作。我们不需要为此创建一个新函数，而是扩展我们的 fetch 示例来实现。

第 12–17 行是。then()函数可以按照我想要的方式处理数据。一个重要的注意事项是 Rails 示例中的第 5 行“render json: [@student](http://twitter.com/student) ”，以及 fetch 示例中的第 4 行“Accept”:“application/JSON”。在 Rails 示例中，我明确声明将@student 信息呈现为 JSON 格式。在 fetch 示例中，头散列的一个键是“Accept”，其值是“application/json”。这个键值对允许 fetch 接收 JSON 格式的数据。这允许从后端服务器到前端的通信在彼此之间传输信息。

# 抽象前端和后端流程

我让 React 和 Rails 相互通信的过程可以在任何前端和后端之间进行抽象。每一步也可以与我的例子相关联。

**前端到后端:**

1.  **我的前端如何发送信息？**在 React 中，这可以通过“POST”方法通过 fetch 发送。
2.  我的后端服务器接收什么格式？ Rails 可以接收 JSON 格式的信息。
3.  **我的前端能否将信息转换成适合后端服务器的格式？**是的，通过头:{ " Content-Type ":" application/JSON " }在我的获取中。
4.  **我的后端服务器如何接收信息？**在 Rails 中，它将信息存储在 params 散列中。

**后端到前端:**

1.  **我的前端接收什么格式？** React 可以通过头接收 JSON 格式的消息:{"Accept": "application/json"}。
2.  我的后端服务器能把信息转换成适合我的前端的格式吗？可以，通过 render json: @student。
3.  **前端如何接收信息？作为我取完东西后的承诺。**

# 关键外卖

在很大程度上，任何可交付成果都可以被抽象和分解成步骤。每一步都应该有一个清晰简洁的“迷你可交付物”,它构建到原始的可交付物中。如果“最小交付”是不可能的，仔细检查你的假设，如果失败了…总是有堆栈溢出！