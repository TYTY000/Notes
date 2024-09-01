Representational Status Transfer
应该就是只如何把需求包含在HTTP 中已实现的**GET,POST,PUT和DELETE**等方法中。
它是一种作用于HTTP接口的代码风格。
如何表征自己这种资源，如何操作资源、如何让自身相关的操作自洽，具体体现为：
这个操作会影响核心资源？
这个操作需要对那种资源进行什么具体动作？可能引发什么结果？有副作用吗？
还需要其他资源去完成这个操作吗？如果需要，如何细化指定？

|   |Non-RESTful site URI|RESTful site URI|
|---|---|---|
|1. Login to site|POST /login/dave|POST /login/dave|
|2. Welcome page|GET /welcome|GET /user/301/welcome|
|3. Add item ID 427 to cart|POST /add/427|POST /user/301/add/427|
|4. View cart|GET / cart|GET /user/301/cart|
|5. Checkout|POST /checkout|POST /user/301/checkout|
前者需假设网站存储了之前页面所有操作的相关信息，后者在权限认证后只需有状态的依赖和转换，并且访问方式很灵活。

**Command-Query Separation**
查改分离

为什么RESTful能够饿到广泛应用呢？
因为它避免了很多在web发展过程中会出现的问题：无状态、网络缓存、不依赖隐式会话等等。

为什么要考虑URI、用户的操作和视图而不是资源呢？
因为对一个网页来说，很难去预知用户的动作，他可能非主观的会发生支付失败的情况；也有可能因为网络问题刷新；也有可能清空购物车；也有可能AFK较长的时间……我们不可能穷尽所有可能去指定相应对策，这个时候网页的状态是未知的，我们不能以网页为中心进行设计，我们只能对相关服务的职能进行控制，充分设想意外情况，并且妥善处理。