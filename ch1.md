<!DOCTYPE html>
<html lang="zh-cn">
    <head>
        <meta charset="utf-8">
        <title>第一章：引言 - Introduction to Tornado 中文翻译</title>
        <meta name="author" content="你像从前一样">

        <link href="./static/css/styles.css" rel="stylesheet">
    </head>

    <body>
        <div class="container">
            <h1>第一章：引言</h1> 

            <p>在过去的五年里，Web开发人员的可用工具实现了跨越式地增长。当技术专家不断推动极限，使Web应用无处不在时，我们也不得不升级我们的工具、创建框架以保证构建更好的应用。我们希望能够使用新的工具，方便我们写出更加整洁、可维护的代码，使部署到世界各地的用户时拥有高效的可扩展性。</p>
            <p>这就让我们谈论到Tornado，一个编写易创建、扩展和部署的强力Web应用的梦幻选择。我们三个都因为Tornado的速度、简单和可扩展性而深深地爱上了它，在一些个人项目中尝试之后，我们将其运用到日常工作中。我们已经看到，Tornado在很多大型或小型的项目中提升了开发者的速度（和乐趣！），同时，其鲁棒性和轻量级也给开发者一次又一次留下了深刻的印象。</p>
            <p>本书的目的是对Tornado Web服务器进行一个概述，通过框架基础、一些示例应用和真实世界使用的最佳实践来引导读者。我们将使用示例来详细讲解Tornado如何工作，你可以用它做什么，以及在构建自己第一个应用时要避免什么。</p>
            <p>在本书中，我们假定你对Python已经有了粗略的了解，知道Web服务如何运作，对数据库有一定的熟悉。有一些不错的书籍可以为你深入了解这些提供参考（比如<span class="bookname">Learning Python</span>，<span class="bookname">Restful Web Service</span>和<span class="bookname">MongoDB: The Definitive Guide）。</span></p>
            <p>你可以在<a href="https://github.com/Introduction-to-Tornado">Github</a>上获得本书中示例的代码。如果你有关于这些示例或其他方面的任何思想，欢迎在那里告诉我们。</p>
            <p>所以，事不宜迟，让我们开始深入了解吧！</p>

            <h2>1.1 Tornado是什么？</h2>
            <p>Tornado是使用Python编写的一个强大的、可扩展的Web服务器。它在处理严峻的网络流量时表现得足够强健，但却在创建和编写时有着足够的轻量级，并能够被用在大量的应用和工具中。</p>
            <p>我们现在所知道的Tornado是基于Bret Taylor和其他人员为FriendFeed所开发的网络服务框架，当FriendFeed被Facebook收购后得以开源。不同于那些最多只能达到10,000个并发连接的传统网络服务器，Tornado在设计之初就考虑到了性能因素，旨在解决C10K问题，这样的设计使得其成为一个拥有非常高性能的框架。此外，它还拥有处理安全性、用户验证、社交网络以及与外部服务（如数据库和网站API）进行异步交互的工具。</p>
            
            <div class="box">
                <h1>延伸阅读：C10K问题</h1>
                <p>基于线程的服务器，如Apache，为了传入的连接，维护了一个操作系统的线程池。Apache会为每个HTTP连接分配线程池中的一个线程，如果所有的线程都处于被占用的状态并且尚有内存可用时，则生成一个新的线程。尽管不同的操作系统会有不同的设置，大多数Linux发布版中都是默认线程堆大小为8MB。Apache的架构在大负载下变得不可预测，为每个打开的连接维护一个大的线程池等待数据极易迅速耗光服务器的内存资源。</p>
                <p>大多数社交网络应用都会展示实时更新来提醒新消息、状态变化以及用户通知，这就要求客户端需要保持一个打开的连接来等待服务器端的任何响应。这些长连接或推送请求使得Apache的最大线程池迅速饱和。一旦线程池的资源耗尽，服务器将不能再响应新的请求。</p>
                <p>异步服务器在这一场景中的应用相对较新，但他们正是被设计用来减轻基于线程的服务器的限制的。当负载增加时，诸如Node.js，lighttpd和Tornodo这样的服务器使用协作的多任务的方式进行优雅的扩展。也就是说，如果当前请求正在等待来自其他资源的数据（比如数据库查询或HTTP请求）时，一个异步服务器可以明确地控制以挂起请求。异步服务器用来恢复暂停的操作的一个常见模式是当合适的数据准备好时调用回调函数。我们将会在<a href="./ch5.html">第五章</a>讲解回调函数模式以及一系列Tornado异步功能的应用。</p>
            </div>

            <p>自从2009年9月10日发布以来，TornadoTornado已经获得了很多社区的支持，并且在一系列不同的场合得到应用。除FriendFeed和Facebook外，还有很多公司在生产上转向Tornado，包括Quora、Turntable.fm、Bit.ly、Hipmunk以及MyYearbook等。</p>
            <p>总之，如果你在寻找你那庞大的CMS或一体化开发框架的替代品，Tornado可能并不是一个好的选择。Tornado并不需要你拥有庞大的模型建立特殊的方式，或以某种确定的形式处理表单，或其他类似的事情。它所做的是让你能够快速简单地编写高速的Web应用。如果你想编写一个可扩展的社交应用、实时分析引擎，或RESTful API，那么简单而强大的Python，以及Tornado（和这本书）正是为你准备的！</p>

            <h3>1.1.1 Tornado入门</h3>
            <p>在大部分*nix系统中安装Tornado非常容易--你既可以从PyPI获取（并使用<code>easy_install</code>或<code>pip</code>安装），也可以从Github上下载源码编译安装，如下所示<cite>[1]</cite>：</p>
            <pre class="shell">$ <kbd>curl -L -O https://github.com/facebook/tornado/archive/v3.1.0.tar.gz</kbd>
$ <kbd>tar xvzf v3.1.0.tar.gz</kbd>
$ <kbd>cd tornado-3.1.0</kbd>
$ <kbd>python setup.py build</kbd>
$ <kbd>sudo python setup.py install</kbd></pre>
            <p>Tornado官方并不支持Windows，但你可以通过ActivePython的PyPM包管理器进行安装，类似如下所示：</p>
            <pre class="shell">C:\&gt; <kbd>pypm install tornado</kbd></pre>
            <p>一旦Tornado在你的机器上安装好，你就可以很好的开始了！压缩包中包含很多demo，比如建立博客、整合Facebook、运行聊天服务等的示例代码。我们稍后会在本书中通过一些示例应用逐步讲解，不过你也应该看看这些官方demo。</p>
            <div class="warning">
                <p>本书中的代码假定你使用的是基于Unix的系统，并且使用的是Python2.6或2.7版本。如果是这样，你就不需要任何除了Python标准库之外的东西。如果你的Python版本是2.5或更低，在安装<dfn>pycURL</dfn>、<dfn>simpleJSON</dfn>和Python开发头文件后可以运行Tornado。<cite>[2]</cite></p>
            </div>

            <h3>1.1.2 社区和支持</h3>
            <p>对于问题、示例和一般的指南，Tornado官方文档是个不错的选择。在<a href="http://tornadoweb.org">tornadoweb.org</a>上有大量的例子和功能缺陷，更多细节和变更可以在<a href="http://github.com/facebook/tornado">Tornado在Github上的版本库</a>中看到。而对于更具体的问题，可以到<a href="http://groups.google.com/group/python-tornado">Tornado的Google Group</a>中咨询，那里有很多活跃的日常使用Tornado的开发者。</p>

            <h2>1.2 简单的Web服务</h2>
            <p>既然我们已经知道了Tornado是什么了，现在让我们看看它能做什么吧。我们首先从使用Tornado编写一个简单的Web应用开始。
            
            <h3>1.2.1 Hello Tornado</h3>
            <p>Tornado是一个编写对HTTP请求响应的框架。作为程序员，你的工作是编写响应特定条件HTTP请求的响应的handler。下面是一个全功能的Tornado应用的基础示例：</p>
            <div class="codelist">
                <div class="codelist-title">代码清单1-1  基础：hello.py</div>
                <pre class="codelist-code">import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web

from tornado.options import define, options
define("port", default=8000, help="run on the given port", type=int)

class IndexHandler(tornado.web.RequestHandler):
    def get(self):
        greeting = self.get_argument('greeting', 'Hello')
        self.write(greeting + ', friendly user!')

if __name__ == "__main__":
    tornado.options.parse_command_line()
    app = tornado.web.Application(handlers=[(r"/", IndexHandler)])
    http_server = tornado.httpserver.HTTPServer(app)
    http_server.listen(options.port)
    tornado.ioloop.IOLoop.instance().start()</pre>
            </div>
            <p>编写一个Tornado应用中最多的工作是定义类继承Tornado的<var>RequestHandler</var>类。在这个例子中，我们创建了一个简单的应用，在给定的端口监听请求，并在根目录（"/"）响应请求。</p>
            <p>你可以在命令行里尝试运行这个程序以测试输出：</p>
            <pre class="shell">$ <kbd>python hello.py --port=8000</kbd></pre>
            <p>现在你可以在浏览器中打开<a href="http://localhost:8000/">http://localhost:8000</a>，或者打开另一个终端窗口使用curl测试我们的应用：</p>
            <pre class="shell">$ <kbd>curl http://localhost:8000/</kbd>
Hello, friendly user!
$ <kbd>curl http://localhost:8000/?greeting=Salutations</kbd>
Salutations, friendly user!</pre>
            <p>让我们把这个例子分成小块，逐步分析它们：</p>
            <pre class="codelist-code">import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web</pre>
            <p>在程序的最顶部，我们导入了一些Tornado模块。虽然Tornado还有另外一些有用的模块，但在这个例子中我们必须至少包含这四个模块。</p>
            <pre class="codelist-code">from tornado.options import define, options
define("port", default=8000, help="run on the given port", type=int)</pre>
            <p>Tornado包括了一个有用的模块（<var>tornado.options</var>）来从命令行中读取设置。我们在这里使用这个模块指定我们的应用监听HTTP请求的端口。它的工作流程如下：如果一个与<var>define</var>语句中同名的设置在命令行中被给出，那么它将成为全局<var>options</var>的一个属性。如果用户运行程序时使用了<code>--help</code>选项，程序将打印出所有你定义的选项以及你在<var>define</var>函数的<var>help</var>参数中指定的文本。如果用户没有为这个选项指定值，则使用<var>default</var>的值进行代替。Tornado使用<var>type</var>参数进行基本的参数类型验证，当不合适的类型被给出时抛出一个异常。因此，我们允许一个整数的<var>port</var>参数作为<var>options.port</var>来访问程序。如果用户没有指定值，则默认为8000。</p>
            <pre class="codelist-code">class IndexHandler(tornado.web.RequestHandler):
    def get(self):
        greeting = self.get_argument('greeting', 'Hello')
        self.write(greeting + ', friendly user!')</pre>
            <p>这是Tornado的请求处理函数类。当处理一个请求时，Tornado将这个类实例化，并调用与HTTP请求方法所对应的方法。在这个例子中，我们只定义了一个<var>get</var>方法，也就是说这个处理函数将对HTTP的<dfn>GET</dfn>请求作出响应。我们稍后将看到实现不止一个HTTP方法的处理函数。</p>
            <pre class="codelist-code">greeting = self.get_argument('greeting', 'Hello')</pre>
            <p>Tornado的<var>RequestHandler</var>类有一系列有用的内建方法，包括<var>get_argument</var>，我们在这里从一个查询字符串中取得参数<var>greeting</var>的值。（如果这个参数没有出现在查询字符串中，Tornado将使用<var>get_argument</var>的第二个参数作为默认值。）</p>
            <pre class="codelist-code">self.write(greeting + ', friendly user!')</pre>
            <p><var>RequestHandler</var>的另一个有用的方法是<var>write</var>，它以一个字符串作为函数的参数，并将其写入到HTTP响应中。在这里，我们使用请求中<var>greeting</var>参数提供的值插入到greeting中，并写回到响应中。</p>
            <pre class="codelist-code">if __name__ == "__main__":
    tornado.options.parse_command_line()
    app = tornado.web.Application(handlers=[(r"/", IndexHandler)])</pre>
            <p>这是真正使得Tornado运转起来的语句。首先，我们使用Tornado的<var>options</var>模块来解析命令行。然后我们创建了一个Tornado的<var>Application</var>类的实例。传递给<var>Application</var>类<var>__init__</var>方法的最重要的参数是<var>handlers</var>。它告诉Tornado应该用哪个类来响应请求。马上我们讲解更多相关知识。</p>
            <pre class="codelist-code">http_server = tornado.httpserver.HTTPServer(app)
http_server.listen(options.port)
tornado.ioloop.IOLoop.instance().start()</pre>
<p>从这里开始的代码将会被反复使用：一旦<var>Application</var>对象被创建，我们可以将其传递给Tornado的<var>HTTPServer</var>对象，然后使用我们在命令行指定的端口进行监听（通过<var>options</var>对象取出。）最后，在程序准备好接收HTTP请求后，我们创建一个Tornado的<var>IOLoop</var>的实例。</p>

            <h4>1.2.1.1 参数handlers</h4>
            <p>让我们再看一眼<span class="filname">hello.py</span>示例中的这一行：</p>
            <pre class="codelist-code">app = tornado.web.Application(handlers=[(r"/", IndexHandler)])</pre>
            <p>这里的参数<var>handlers</var>非常重要，值得我们更加深入的研究。它应该是一个元组组成的列表，其中每个元组的第一个元素是一个用于匹配的正则表达式，第二个元素是一个<var>RequestHanlder</var>类。在<var>hello.py</var>中，我们只指定了一个正则表达式-<var>RequestHanlder</var>对，但你可以按你的需要指定任意多个。</p>

            <h4>1.2.1.2 使用正则表达式指定路径</h4>
            <p>Tornado在元组中使用正则表达式来匹配HTTP请求的路径。（这个路径是URL中主机名后面的部分，不包括查询字符串和碎片。）Tornado把这些正则表达式看作已经包含了行开始和结束锚点（即，字符串"/"被看作为"^/$"）。</p>
            <p>如果一个正则表达式包含一个捕获分组（即，正则表达式中的部分被括号括起来），匹配的内容将作为相应HTTP请求的参数传到<var>RequestHandler</var>对象中。我们将在下个例子中看到它的用法。</p>

            <h3>1.2.2 字符串服务</h3>
            <p>例1-2是一个我们目前为止看到的更复杂的例子，它将介绍更多Tornado的基本概念。</p>
            <div class="codelist">
                <div class="codelist-title">代码清单1-2  处理输入：string_service.py</div>
                <pre class="codelist-code">import textwrap

import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web

from tornado.options import define, options
define("port", default=8000, help="run on the given port", type=int)

class ReverseHandler(tornado.web.RequestHandler):
    def get(self, input):
        self.write(input[::-1])

class WrapHandler(tornado.web.RequestHandler):
    def post(self):
        text = self.get_argument('text')
        width = self.get_argument('width', 40)
        self.write(textwrap.fill(text, int(width)))
        
if __name__ == "__main__":
    tornado.options.parse_command_line()
    app = tornado.web.Application(
        handlers=[
            (r"/reverse/(\w+)", ReverseHandler),
            (r"/wrap", WrapHandler)
        ]
    )
    http_server = tornado.httpserver.HTTPServer(app)
    http_server.listen(options.port)
    tornado.ioloop.IOLoop.instance().start()</pre>
            </div>
            <p>如同运行第一个例子，你可以在命令行中运行这个例子使用如下的命令：</p>
            <pre class="shell">$ <kbd>python string_service.py --port=8000</kbd></pre>
            <p>这个程序是一个通用的字符串操作的Web服务端基本框架。到目前为止，你可以用它做两件事情。其一，到<code>/reverse/string</code>的<dfn>GET</dfn>请求将会返回URL路径中指定字符串的反转形式。</p>
            <pre class="shell">$ <kbd>curl http://localhost:8000/reverse/stressed</kbd>
desserts

$ <kbd>curl http://localhost:8000/reverse/slipup</kbd>
pupils</pre>
            <p>其二，到<code>/wrap</code>的<dfn>POST</dfn>请求将从参数<var>text</var>中取得指定的文本，并返回按照参数<var>width</var>指定宽度装饰的文本。下面的请求指定一个没有宽度的字符串，所以它的输出宽度被指定为程序中的<var>get_argument</var>的默认值40个字符。</p>
            <pre class="shell">$ <kbd>http://localhost:8000/wrap -d text=Lorem+ipsum+dolor+sit+amet,+consectetuer+adipiscing+elit.</kbd>
Lorem ipsum dolor sit amet, consectetuer
adipiscing elit.</pre>
            <p>字符串服务示例和上一节示例代码中大部分是一样的。让我们关注那些新的代码。首先，让我们看看传递给<var>Application</var>构造函数的<var>handlers</var>参数的值：</p>
            <pre class="codelist-code">app = tornado.web.Application(handlers=[
    (r"/reverse/(\w+)", ReverseHandler),
    (r"/wrap", WrapHandler)
])</pre>
            <p>在上面的代码中，<var>Application</var>类在"handlers"参数中实例化了两个<var>RequestHandler</var>类对象。第一个引导Tornado传递路径匹配下面的正则表达式的请求：</p>
            <pre class="codelist-code">/reverse/(\w+)</pre>
            <p>正则表达式告诉Tornado匹配任何以字符串/reverse/开始并紧跟着一个或多个字母的路径。括号的含义是让Tornado保存匹配括号里面表达式的字符串，并将其作为请求方法的一个参数传递给<var>RequestHandler</var>类。让我们检查<var>ReverseHandler</var>的定义来看看它是如何工作的：</p>
            <pre class="codelist-code">class ReverseHandler(tornado.web.RequestHandler):
    def get(self, input):
        self.write(input[::-1])</pre>
            <p>你可以看到这里的<var>get</var>方法有一个额外的参数<var>input</var>。这个参数将包含匹配处理函数正则表达式第一个括号里的字符串。（如果正则表达式中有一系列额外的括号，匹配的字符串将被按照在正则表达式中出现的顺序作为额外的参数传递进来。）</p>
            <p>现在，让我们看一下<var>WrapHandler</var>的定义：</p>
            <pre class="codelist-code">class WrapHandler(tornado.web.RequestHandler):
    def post(self):
        text = self.get_argument('text')
        width = self.get_argument('width', 40)
        self.write(textwrap.fill(text, int(width)))</pre>
            <p><var>WrapHandler</var>类处理匹配路径为<code>/wrap</code>的请求。这个处理函数定义了一个<var>post</var>方法，也就是说它接收HTTP的<dfn>POST</dfn>方法的请求。</p>
            <p>我们之前使用<var>RequestHandler</var>对象的<var>get_argument</var>方法来捕获请求查询字符串的的参数。同样，我们也可以使用相同的方法来获得<dfn>POST</dfn>请求传递的参数。（Tornado可以解析URLencoded和multipart结构的<dfn>POST</dfn>请求）。一旦我们从<dfn>POST</dfn>中获得了文本和宽度的参数，我们使用Python内建的<var>textwrap</var>模块来以指定的宽度装饰文本，并将结果字符串写回到HTTP响应中。</p>

            <h3>1.2.3 关于RequestHandler的更多知识</h3>
            <p>到目前为止，我们已经了解了<var>RequestHandler</var>对象的基础：如何从一个传入的HTTP请求中获得信息（使用<var>get_argument</var>和传入到<var>get</var>和<Var>post</var>的参数）以及写HTTP响应（使用<var>write</var>方法）。除此之外，还有很多需要学习的，我们将在接下来的章节中进行讲解。同时，还有一些关于<var>RequestHandler</var>和Tornado如何使用它的只是需要记住。</p>

            <h4>1.2.3.1 HTTP方法</h4>
            <p>截止到目前讨论的例子，每个<var>RequestHandler</var>类都只定义了一个HTTP方法的行为。但是，在同一个处理函数中定义多个方法是可能的，并且是有用的。把概念相关的功能绑定到同一个类是一个很好的方法。比如，你可能会编写一个处理函数来处理数据库中某个特定ID的对象，既使用<dfn>GET</dfn>方法，也使用<dfn>POST</dfn>方法。想象<dfn>GET</dfn>方法来返回这个部件的信息，而<dfn>POST</dfn>方法在数据库中对这个ID的部件进行改变：</p>
            <pre class="codelist-code"># matched with (r"/widget/(\d+)", WidgetHandler)
class WidgetHandler(tornado.web.RequestHandler):
    def get(self, widget_id):
        widget = retrieve_from_db(widget_id)
        self.write(widget.serialize())

    def post(self, widget_id):
        widget = retrieve_from_db(widget_id)
        widget['foo'] = self.get_argument('foo')
        save_to_db(widget)</pre>
            <p>我们到目前为止只是用了<dfn>GET</dfn>和<dfn>POST</dfn>方法，但Tornado支持任何合法的HTTP请求（<dfn>GET</dfn>、<dfn>POST</dfn>、<dfn>PUT</dfn>、<dfn>DELETE</dfn>、<dfn>HEAD</dfn>、<dfn>OPTIONS</dfn>）。你可以非常容易地定义上述任一种方法的行为，只需要在<var>RequestHandler</var>类中使用同名的方法。下面是另一个想象的例子，在这个例子中针对特定frob ID的<dfn>HEAD</dfn>请求只根据frob是否存在给出信息，而<dfn>GET</dfn>方法返回整个对象：</p>
            <pre class="codelist-code"># matched with (r"/frob/(\d+)", FrobHandler)
class FrobHandler(tornado.web.RequestHandler):
    def head(self, frob_id):
        frob = retrieve_from_db(frob_id)
        if frob is not None:
            self.set_status(200)
        else:
            self.set_status(404)
    def get(self, frob_id):
        frob = retrieve_from_db(frob_id)
        self.write(frob.serialize())</pre>
            
            <h4>1.2.3.2 HTTP状态码</h4>
            <p>从上面的代码可以看出，你可以使用<var>RequestHandler</var>类的<var>ser_status()</var>方法显式地设置HTTP状态码。然而，你需要记住在某些情况下，Tornado会自动地设置HTTP状态码。下面是一个常用情况的纲要：</p>
            <h5>404 Not Found</h5>
            <p class="indentation">Tornado会在HTTP请求的路径无法匹配任何<var>RequestHandler</var>类相对应的模式时返回404（Not Found）响应码。</p>
            <h5>400 Bad Request</h5>
            <p class="indentation">如果你调用了一个没有默认值的<var>get_argument</var>函数，并且没有发现给定名称的参数，Tornado将自动返回一个400（Bad Request）响应码。</p>
            <h5>405 Method Not Allowed</h5>
            <p class="indentation">如果传入的请求使用了<var>RequestHandler</var>中没有定义的HTTP方法（比如，一个<dfn>POST</dfn>请求，但是处理函数中只有定义了<var>get</var>方法），Tornado将返回一个405（Methos Not Allowed）响应码。</p>
            <h5>500 Internal Server Error</h5>
            <p class="indentation">当程序遇到任何不能让其退出的错误时，Tornado将返回500（Internal Server Error）响应码。你代码中任何没有捕获的异常也会导致500响应码。</p>
            <h5>200 OK</h5>
            <p class="indentation">如果响应成功，并且没有其他返回码被设置，Tornado将默认返回一个200（OK）响应码。</h5>
            <p>当上述任何一种错误发生时，Tornado将默认向客户端发送一个包含状态码和错误信息的简短片段。如果你想使用自己的方法代替默认的错误响应，你可以重写<var>write_error</var>方法在你的<var>RequestHandler</var>类中。比如，代码清单1-3是<span class="filename">hello.py</span>示例添加了常规的错误消息的版本。</p>
            <div class="codelist">
                <div class="codelist-title">代码清单1-3  常规错误响应：hello-errors.py</div>
                <pre class="codelist-code">import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web

from tornado.options import define, options
define("port", default=8000, help="run on the given port", type=int)

class IndexHandler(tornado.web.RequestHandler):
    def get(self):
        greeting = self.get_argument('greeting', 'Hello')
        self.write(greeting + ', friendly user!')
    def write_error(self, status_code, **kwargs):
        self.write("Gosh darnit, user! You caused a %d error." % status_code)

if __name__ == "__main__":
    tornado.options.parse_command_line()
    app = tornado.web.Application(handlers=[(r"/", IndexHandler)])
    http_server = tornado.httpserver.HTTPServer(app)
    http_server.listen(options.port)
    tornado.ioloop.IOLoop.instance().start()</pre>
            </div>
            <p>当我们尝试一个<dfn>POST</dfn>请求时，会得到下面的响应。一般来说，我们应该得到Tornado默认的错误响应，但因为我们覆写了<var>write_error</var>，我们会得到不一样的东西：</p>
            <pre class="shell">$ <kbd>curl -d foo=bar http://localhost:8000/</kbd>
Gosh darnit, user! You caused a 405 error.</pre>
            
            <h3>1.2.4 下一步</h3>
            <p>现在你已经明白了最基本的东西，我们渴望你想了解更多。在接下来的章节，我们将向你展示能够帮助你使用Tornado创建成熟的Web服务和应用的功能和技术。首先是：Tornado的模板系统。</p>

            <div class="cite-description">
                [1] 压缩包地址已更新到Tornado的最新版本3.1.0。<br />
                [2] 书中原文中关于Python3.X版本的兼容性问题目前已不存在，因此省略该部分。
            </div>

            <div class="footer">
                <small>&copy; 本文由<a href="http://www.pythoner.com">你像从前一样</a>翻译，转载请注明出处。本书版权由原作者拥有。</small>
            </div>

        </div>
    </body>
</html>
