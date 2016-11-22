
> 记录我自学 Laravel 的过程.

## 2016.11.21

今天优化了一些 Model, 已经减少了一些 DB 查询, 但是返回的数据有大量的重复内容, 继续优化.
通过优化, 已经掌握了一些使用 Eloquent 的技巧. 主要对预加载查询熟悉了很多.


<br />
    
---
<br />
## 2016.11.20

虽然昨天已经将数据按需求返回了, 但是我想应该只是完成了最开始的部分, 剩下的部分还有: 

1. 优化 Model 层
    - 部分数据返回是直接执行的 DB 查询, 没有用到 Eloquent 模型之间的关联, 查询效率应该会较低, 应该多用 Eloquent 模型
2. 使用 Seeder 或者 Factories 编写假数据
    - 需要让项目的其他人在别的环境中, 也能使用我的API, 我自己就必须生成一些数据, 在别人的环境上也能使用
3. 异常/错误处理和测试
    - 在完成了以上 1,2 两点之后, 根据数据库的结构, 设计出一些测试场景, 测试代码严谨程度
    - 不正常的输入和查询不到的返回, 报错
4. 代码注释
    - 帮助自己加深对项目和框架的理解, 帮助项目中的其他人理解项目业务逻辑, 节省他人时间成本.
    
至少完成了这些, 这个 pull request 才能到可以被提交的程度.





<br />
    
---
<br />
## 2016.11.19

今天放弃了使用transformer来返回数据, 通过在网上搜索更多 PHP 返回数据的例子, 现在转而使用将数据模型 toArray(), 然后直接返回的方法.
这个方法虽然复杂, 但是是可行的, 终于可以返回一个比较复杂的 json 数据了. 

方法主要是由我自己进行多次的数据库查找, 并将查找到的结果拼接到一起, 最后返回.
   






<br />
    
---
<br />
## 2016.11.18

1. 今天放弃了假数据的导入, 因为自己想了一下这部分如果有困难可以以后搞, 毕竟我还是可以手动向数据库中写入假数据的.

2. 同时开始研究怎么返回数据. 但是也碰到了困难, 因为现在项目中的API返回的数据都比较简单, 层级最多两层, 而我的任务是要返回一个有 5 层的API. 现在项目中都是使用了 transformer 来调整返回数据的规则, 但研究了很久, 没有找到transformer 中如何返回复杂数据的方法. 
   






<br />
    
---
<br />
## 2016.11.17

本来打算从假数据的导入开始这个项目, 在弄假数据的时候, 遇到了困难, 实际项目中的表很多, 而网上的很多教程的假数据导入都只是 1~2 个表, 之间的联系也比较少. 一直没找到多个表关联的时候, 假数据应该如何导入. 主要是涉及到4-5表之间的key是有互相的关联.
   

同时, 今天在看 Laravel 文档的时候, 在对照英文原版文档看的时候, 发现了一个中文的逻辑翻译错误, 文档是 [Laravel-China.org](https://laravel-china.org) 版本的.

发现他们是用github 来管理这些返回文档的, 所以就提交了一个 [Issue](https://github.com/laravel-china/laravel-docs/issues/86) 和一个修复翻译错误的 [Pull Request](https://github.com/laravel-china/laravel-docs/pull/85), 他们反应很快, 一天不到这个 PR 就被 merge 了. 

很高兴.

<img src="https://assets-cdn.github.com/images/modules/profile/profile-first-pr.png" align="middle" width = "80%" />






<br />
    
---
<br />
## 2016.11.16

开始写一些实际的代码了, 但是在参考项目之前代码的时候, 遇到了一些问题:

1. 数据库表建立的时候是否需要外键?
    现阶段的服务器技术, 尽量不需要加外键, 会对服务器性能造成影响. 而服务器内一般会有一层架构, 在数据库执行sql前, 帮助数据库进行数据的校验.
2. 数据库各个表中, 各个字段之间的联系是如何被建立的?
    通过 Eloquent ORM, 在 Laravel 中每个数据库表都可以建立相对应的 Model 类去跟数据库交互.
3. Laravel 是如何知道某个 Model 类是控制某个数据库表的? 有2种方式:
    1. 通过 Model 类的名字, 一般是类名字的复数形式并加's', 即: Task 类 => 'tasks'表
    2. 在 Model 类中, 通过 protected $table = 'tasks'; 显式的将 Model 类与数据库表绑定
4. 在 Model 类中的写的那些方法, 比如 belongTo hasMany 等, 这些方法有什么用? 
    1. 可以说明数据库表之间的联系
    2. 执行便捷和强大的查询, 方便的获取相关联的模型, 或者对这些模型进行操作
5. Eloquent 中的工厂中的用到的几个函数的含义分别是什么? 
    1. create() 方法创建模型，并在内部调用一次 save() 将创建的模型保存至数据库, [相关资料1](https://laravel.com/docs/5.2/eloquent-relationships#inserting-related-models), [相关资料2](https://laravel.com/docs/5.2/testing#working-with-databases)
    2. make() 方法创建模型，但不将它们保存至数据库
    3. create 和 make 都会将模型返回出来
    4. [save()](https://laravel.com/docs/5.2/eloquent-relationships#inserting-related-models) 方法是在插入关联的数据模型时使用到
6. 关于一次持久化多个模型到数据库, [Writing Seeders](https://laravel.com/docs/5.2/seeding#writing-seeders)
     
        factory(App\User::class, 50)->create()->each(
          function($u)
            {
              $u->posts()->save(
                factory(App\Post::class)->make()
              );
            }
        );

7. 上面的代码中的 each 方法, 又涉及到了一个新的知识点, 即为 [Eloquent ORM 中的集合](http://laravelacademy.org/post/3031.html)的概念, 集合为我们提供了非常丰富的工具, 让我们能够方便的对集合中的元素进行操作. 单说 [each](http://laravelacademy.org/post/178.html#ipt_kb_toc_178_9), each会迭代集合中的每个数据项, 并将数据项传递给 each 内的函数并执行该函数
    






<br />
    
---
<br />
## 2016.11.15

研究了项目中以往已经建好的数据表的格式, 以及, 建立这些表的方式.

现在看来, 将数据导入到数据库中的方式是有2种

1. 编写 Seeder 文件, 并在 DatabaseSeeder 中调用
2. 在 Model Factories 中用 Fake 方法直接程序生成假数据

两种方法的应用场景, 在我看来是这样的:

1. Seeder 文件中的假数据是非常具体的, 跟SQL语句非常类似, 一般用于真实数据的导入, 比如

       ['张三', '男', '29'], 导入这一条数据到某个表中

2. Factories 中的数据是随机生成的, 并且可以批量按照脚本的方式导入到数据库中, 比如
      
       'status' => $faker->numberBetween(0, 6),

同时, 一般情况下, 在数据库表中的关联越多, 使用Seeder方法生成数据的效率就会越高.

再附注一篇关于 Laravel 数据填充的很好的文章, https://laravel-china.org/index.php/topics/2923



<br />
    
---
<br />
## 2016.11.14

针对项目小组给出的数据库表的字段和格式, 开始新建数据库表, 并写入数据.



<br />
    
---
<br />
## 2016.11.13

任务的需求比较简单或者说简陋, 并没有文字的需求列表. 是按照一张 Android UI 图中的显示, 来分析和设计要返回的数据. 

基本上写完了, 要返回的数据格式(json), 明天可以开始进行实际的代码编写了.



<br />
    
---
<br />
## 2016.11.12

加入了一个网上的PHP学习小组, 小组内的成员通过开发实际项目来进行 PHP 的学习和提升. 今天领到了一个从数据库中查询并处理和返回数据的任务. 今天开始对这个任务和项目的业务逻辑进行了解.



<br />
    
---
<br />
## 2016.11.11

今天从社区里下了一个开源代码, 在初始化项目的时候, 执行 php artisan db:seed 的时候, 报错

    [InvalidArgumentException]
    You requested 1 items, but there are only 0 items in the collection

经过反复调试和查看文档, 发现这个错误是由于, 在填充数据时, 数据库A表的填充依赖B表的数据, 而B表此时还没有初始化, 是空表.

所以需要将B表的初始化代码. 提前到A表的初始化代码之前, 就可以解决这个问题了.

提交了一个 Pull Request 给代码所在的 Gitlab


<br />
    
---
<br />
## 2016.11.10
今天从这个 技术社区 (http://larabase.com/collection/1/post/33) 里获取了一些关于 Laravel 的架构和基本概念知识. 

看了这个社区里的文章, 再看别人的项目代码, 又看懂了一些东西:

1.  用 Repository 模式来对 Controller 中的业务逻辑进行解耦
    1.  在 Repositories 文件夹下分别编写接口和其对应的实现
    2.  在 ServiceProvider 中的 register() 中, 绑定了接口
2.  使用 Type-Hint 可以在很多场景, 比如 Controller 的构造函数中 注入对象
3.  Service Provider/Container 是Laravel的核心机制. $app 是IOC容器, 有了这个容器, 就可以在任何地方解析得到注册过的类的实例. 而注册就是在 Service Provider 中完成的, Container 也就是容器.


<br />
    
---
<br />
## 2016.11.09
今天从网上找了一些项目代码来看, 虽然也是教学性质的项目, 但这些项目的复杂度已经不是前两天官方的简单教程的程度了.
 
比如里面有一些概念还有代码的逻辑还是看不懂. 比如项目里各个文件夹里都是放了一些什么内容. 唯一知道的就是, 当一个请求发送过来之后, 这个服务器程序应该是从路由 route.php 开始工作的. 

找到了一个比较不错的技术社区, 看了一些大概, 觉得应该有我这一段时间入门所需要的知识.
明天继续研究一下. 社区地址是: http://larabase.com/collection/1/post/33


<br />
    
---
<br />
## 2016.11.08

初步了解了 Laravel 的 MVC架构.

###Blog项目 
完成了简单 blog 项目(https://github.com/johnlui/Learn-Laravel-5/issues) 中的所有过程(登录、后台编辑、前台评论)都跑通了. 
代码记录在 https://github.com/Hexor/learnlaravel5 中.

项目中的前端页面中的代码, 能基本看懂是干嘛的, 但是还不能理解前端的这些语言的写法和规则, 所以前端页面代码是直接抄, 暂时先不去深究前端的显示方式.
所以这个教学项目中附注的最后的大作业暂时没有进行.

完成了这个项目之后, 自己能理解 Laravel 框架的一些基础知识了, 包括

1.  路由: 就是将不同的url请求分配到不同的controller去处理
2.  Controller: 接收前台发送的一些数据, 并且进行处理, 这些处理有可能是跟Model或者View进行交互, 进行数据的增删查改, 新页面的返回
3.  Model: 连接整个框架和数据库, 为其他组件提供了一种面向对象的方式来与数据库中的数据进行交互
4.  View:  可以应用框架提供的模板, 并且将Controller提供的数据填充在页面中, 一起返回到前端显示


###laravel-china 教程任务清单
看了laravel-china.org上的初级任务清单, 感觉内容跟blog项目比较类似. 就不再重复一遍了.

打算完成laravel-china.org 上的中级任务清单. 但是过程中遇到了一些困难, 这个教程里的教学代码跟教程提供的github中的代码有出入, 感觉是版本没有对应好. 所以这个项目没有跑起来. 
但还是了解了一些新的概念, 以下是我现在的理解:

1. 依赖注入: 还没搞明白是什么
2. Blade 模板: 这个模板是Laravel使用的, 在前端页面显示时一套辅助PHP逻辑的框架
3. 中间件: 在前端发送请求到后端时, 在后端接受到请求之前,将请求再次加工处理, 或者过滤
4. 授权策略: 还没明白. 这个跟 Auth中间件 有什么区别


<br />
    
---
<br />

## 2016.11.07

继续入门Laravel, 能看懂基本的代码逻辑了. 完成了的 https://github.com/johnlui/Learn-Laravel-5/issues 中的第1-3步

<br />
    
---
<br />
## 2016.11.06

在忙家里的事情, 没有学习.

<br />
    
---
<br />
## 2016.11.05

简单的看了一遍hfphp, 内容跟以前学过的知识都比较相近, 所以没有细看. 看这个主要还是让我有了一些自信心和入手点.

通过那本电子书(七天学会PHP), 大致了解php的一些语法知识, 以及关键字, 面向对象特性等基础知识.

另外, 昨天说的 再配置基础的php环境就不必了. 虚拟机内的环境足够我进行基本的测试了. 只要把代码放到 public下就可以了. 访问服务器的域名时, 默认会直接使用index.php来进行返回.

接下来可以看 Laravel的入门教程了. 遇到看不懂或者不会的地方, 就直接搜索, 或者查php的官方文档.

<br />
    
---
<br />
## 2016.11.04

用了昨天半天加上今天的大半天, 跑通了 前端 和 Laravel 框架的环境设置.
之后初步搜索了一些关于 Laravel 入门的文章. 发现大多数的教程 都需要读者有 PHP 网站运行的基本知识.

这部分的知识我目前是欠缺的. 所以, 在我直接看项目代码的时候, 大部分的关键字, 语法我基本上都看不懂. 所以还是要先补一下PHP的基础知识.

对于如何入门 Laravel , 找到了一篇还不错的教程. https://github.com/johnlui/Learn-Laravel-5/issues

准备在了解PHP基础之后, 来跟着这个教程走一下, 走到一定程度, 应该就可以看懂现在的API的项目了.

那么如何了解PHP基础呢?

1.  Headfirst PHP 书

2.  七天学会PHP http://phpbook.phpxy.com/43512

2.  https://www.zhihu.com/question/39078661 燕十八回答的第一轮/第二轮迭代

在学习PHP基础的过程中, 实际代码编写肯定又是免不了的. 虽然之前用的Homestead 建设过完整PHP环境的虚拟机, 但是感觉这个虚拟机应该是为Laravel定制的, 这次的代码编写就不用这个虚拟机了. 还是要先在自己的主机里配置一下基础的PHP环境.

接下来的目标: 了解PHP基础
