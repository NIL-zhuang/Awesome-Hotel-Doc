# 软件体系结构文档模板

## 文档修改历史

|           修改人员           | 日期      | 修改原因                                                         | 版本号   |
| :--------------------------: | --------- | ---------------------------------------------------------------- | -------- |
| 庄子元、程荣鑫、韩禧、郭礼华 | 2020.4.19 | 填写初始文档，添加TODO标记                                       | 初稿     |
|            程荣鑫            | 2020.4.19 | 添加目录，完成组合视图                                           | 草稿v1.0 |
|            郭礼华            | 2020.4.23 | 添加了架构设计中的模块职责和用户界面层分解                       | 草稿v1.0 |
|            庄子元            | 2020.4.24 | 添加了架构涉及中业务逻辑层的模块职责和用户界面分解，完成逻辑视图 | 草稿v1.0 |
|             韩禧             | 2020.4.25 | 完善了数据层                                                     | 草稿v1.0 |

## 目录

[TOC]

## 引言

### 编制目的

本报告详细完成对酒店预定系统的概要设计，达到指导详细设计和开发的目的，同时实现和测试人员及用户的沟通。

本报告面向开发人员、测试人员及最终用户而编写，是了解系统的导航。

### 词汇表

|   词汇名称    | 词汇含义         | 备注 |
| :-----------: | ---------------- | ---- |
| Awesome Hotel | 奥森酒店预定系统 | -    |

### 参考资料

1. IEEE标准
2. 酒店预定系统用例文档、软件需求规格说明文档
3. 《软件工程与计算（卷二） 软件开发的技术基础》

## 产品概述

***Awesome Hotel*** 酒店房间预订系统是软件工程与计算Ⅱ开发小组制作的在线酒店房间预订系统，开发目的是为了帮助用户实现线上的酒店客房预订服务。功能包括用户个人基本信息管理、浏览酒店详细信息、预定酒店、查看订单等，以及酒店工作人员的基本信息管理、维护酒店基本信息、录入可用客房等管理功能，和系统管理员的注册酒店工作人员账号、更改管理员密码等功能。

通过 ***Awesome Hotel*** 酒店房间预订系统，可以方便用户预定酒店客房，节约时间；为酒店减少线下售房，降低经营成本，从而吸引更多顾客，提高用户的满意度和酒店的盈利。

## 逻辑视图

奥森酒店预订系统选择了分层体系结构风格，将系统分为3层（展示层、业务逻辑层、数据层）能够很好的示意整个高层抽象。展示层包含页面实现，业务逻辑层包含业务逻辑处理的实现，数据层负责数据的持久化和访问。分层体系结构的逻辑视角和逻辑设计方案如下。

![logic_view](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_logic_view.png)

![logic_design](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/logic_view.png)

## 组合视图

### 开发包图

* 表示软件组件在开发时环境中的静态组织
  * 描述开发包以及相互间的依赖
  * 绘制开发包图
    * 开发包图类似上述示意图画法

|      开发包      | 依赖的其他开发包                                                          |
| :--------------: | ------------------------------------------------------------------------- |
|     mainview     | userview, hotelmanagerview, hotelview, adminview, vo, store, router       |
|     userview     | api, userblservice, vo                                                    |
|  userblservice   | vo                                                                        |
|      userbl      | userblservice, usermapper, po, utilitybl                                  |
|    usermapper    | po                                                                        |
|     userdata     | po, databaseutility, usermapper                                           |
| hotelmanagerview | api, hotelblservice, couponblservice, vo                                  |
|    hotelview     | api, hotelblservice, orderblservice, vo                                   |
|  hotelblservice  | vo                                                                        |
|     hotelbl      | hotelblservice, orderblservice, userblservice, po, hotelmapper, utilitybl |
|   hotelmapper    | po                                                                        |
|    hoteldata     | po, databaseutility, hotelmapper                                          |
|  orderblservice  | vo                                                                        |
|     orderbl      | orderblservice, userblservice, ordermapper, po, utilitybl                 |
|   ordermapper    | po                                                                        |
|    orderdata     | po, databaseutility, ordermapper                                          |
| couponblservice  | vo                                                                        |
|     couponbl     | conponblservice, conponmapper, po, utilitybl                              |
|   couponmapper   | po                                                                        |
|    coupondata    | po, databaseutility, couponmapper                                         |
|    adminview     | api, adminblservice, vo                                                   |
|  adminblservice  | vo                                                                        |
|     adminbl      | adminblservice, adminmapper, po, utilitybl                                |
|   adminmapper    | po                                                                        |
|    admindata     | po, databaseutility, adminmapper                                          |
|      store       | api                                                                       |
|       api        | utils                                                                     |
|      router      |                                                                           |
|      utils       |                                                                           |
|        vo        |                                                                           |
|        po        |                                                                           |
|    utilitybl     |                                                                           |
| databaseutility  | MyBatis                                                                   |

由于Web应用的特殊性，奥森酒店预定系统 ***Awesome Hotel*** 的客户端不需要进行开发，只需用户自行安装浏览器即可，故略过。服务器端开发包图如下所示。

![serverpkg](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/severpkg.png)

### 运行时进程

***Awesome Hotel*** 中会有多个客户端（浏览器）进程和一个服务器端进程。结合部署图，客户端进程在客户端机器上运行，服务器端进程在服务器端机器上运行。进程图如下所示。

*注：在本Web App中，客户端就是浏览器。*

![runtime_process](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_runtime_process.png)

### 物理部署

***Awesome Hotel*** 中，客户端构件是放在客户端机器上的，服务器端构件是放在服务器端机器上的。在客户端节点上，只需要安装现代浏览器（即支持HTML、XHTML、CSS、ECMAScript及W3C DOM标准的浏览器）即可。具体部署图如下图所示。

![deploy](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_physical_deploy.png)

## 架构设计

### 模块职责

由于Web应用的特殊性，奥森酒店预定系统 ***Awesome Hotel*** 的客户端不需要进行开发，只需用户自行安装浏览器即可，故略过。服务器端模块视图如下所示。

#### 模块视图

![module_view](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_service_module.jpg)

#### 各层职责

|     层     | 职责                                                     |
| :--------: | -------------------------------------------------------- |
|  启动模块  | 负责初始化网络通信机制以及数据服务的连接，并启动用户界面 |
|   展示层   | 基于Web的互联网酒店预定系统的客户端用户界面              |
|  接口模块  | 负责客户端和服务器端的通信及数据传递                     |
| 业务逻辑层 | 对于用户界面的输入进行响应并执行业务处理逻辑             |
| 数据服务层 | 为业务逻辑层提供数据层服务接口                           |
|   数据层   | 负责数据的持久化和访问                                   |

   每一层只是使用下方直接接触的层，层与层之间仅仅是通过接口的调用来完成的，层之间调用的接口如下所示。

#### 层之间调用接口

|                                                                                                                     接口                                                                                                                      | 服务调用方         | 服务提供方         |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ------------------ | ------------------ |
|                                                                                adminAPI<br/>couponAPI<br/>hotelAPI<br>hotelManagerAPI<br/>orderAPI<br/>useAPI                                                                                 | 客户端页面         | 服务器端展示层     |
| AccountService<br>AdminService<br/>HotelService<br/>RoomService<br/>OrderService<br/>CouponMatchStrategy<br/>CouponService<br/>HotelSearchService<br/>AnswerService<br/>QuestionService<br/>CollectionService<br/>LevelService<br/>VIPService | 服务器端展示层     | 服务器端业务逻辑层 |
|                     AccountMapper<br/>AdminMapper<br/>HotelMapper<br/>RoomMapper<br/>OrderMapper<br/>CouponMapper<br/>AnswerMapper<br/>QuestionMapper<br/>CollectionMapper<br/>CreditMapper<br/>LevelMapper<br/>VIPMapper                     | 服务器端业务逻辑层 | 服务器端数据层     |

借用添加酒店用例来说明层之间的调用，如图所示。每一层之间都是由上层依赖了一个接口（需接口），而下层实现这个接口（供接口）。HotelBLService 提供了 HotelUI 界面所需要的所有业务逻辑功能。HotelMapper 提供了对数据库的增、删、改、查等操作。这样的实现就大大降低了层与层之间的耦合。

![interface](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_hotelbl_interface.jpg)

### 用户界面层分解

根据需求，系统存在14个用户界面：登录界面、注册界面、酒店浏览界面、酒店信息界面、预定酒店界面、酒店详情界面、用户个人信息界面、用户订单界面、酒店管理界面、添加酒店界面、录入房间界面、查看优惠策略界面、添加优惠策略界面、工作人员订单管理界界面。界面跳转如图所示。

![jump](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_user_interface_jump.jpg)

用户界面类如图所示。

![interface component](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_user_interface_component_class.jpg)

#### 展示层模块的职责

| 模块             | 职责                                                                       |
| ---------------- | -------------------------------------------------------------------------- |
| mainView         | 主界面，负责界面的显示和界面的跳转                                         |
| adminView        | 管理员界面，负责允许管理员进行酒店工作人员的管理                           |
| hotelmanagerView | 酒店管理员界面，负责允许酒店工作人员进行酒店信息的更改，以及酒店订单的处理 |
| userView         | 用户界面，负责允许用户管理自己的账户信息，允许用户对酒店的预定             |
| hotelView        | 酒店展示界面，负责酒店信息的显示                                           |
| couponView       | 优惠券界面，负责允许用户查看优惠券以及酒店工作人员进行优惠券的添加         |
| orderView        | 订单界面，负责允许用户查看自己的订单以及对自己订单的更改                   |

#### 展示层模块的接口规范

##### adminView 模块的接口规范

需要的服务（需接口）

| 服务名                                       | 服务                     |
| :------------------------------------------- | ------------------------ |
| AdminBLService.addManager(UserForm userForm) | 添加酒店管理人员账号     |
| AdminBLService.getAllManagers()              | 获得所有酒店管理人员信息 |

##### hotelmanagerView 模块的接口规范

需要的服务（需接口）

| HotelManagerBLService.modifyRoomInfo(Integer hotelId)  | 修改酒店的客房信息 |
| ------------------------------------------------------ | ------------------ |
| HotelManagerBLService.modifyHotelInfo(Integer hotelId) | 修改酒店的基本信息 |

##### userView 模块的接口规范

需要的服务（需接口）

| 服务名                                                                                   | 服务                                            |
| ---------------------------------------------------------------------------------------- | ----------------------------------------------- |
| UserBLService.registerAccount(UserVO userVO)                                             | 注册账号                                        |
| UserBLService.login(UserForm userForm)                                                   | 用户登陆，登录成功会将用户信息保存再session中   |
| UserBLService.getUserInfo(int id)                                                        | 获取用户个人信息                                |
| UserBLService.updateUserInfo(int id, String password,String username,String phonenumber) | 更新用户的个人信息，包括密码， 账户名和手机号码 |

##### hotelView 模块的接口规范

需要的服务（需接口）

| 服务名                                                                        | 服务                     |
| ----------------------------------------------------------------------------- | ------------------------ |
| hotelBLService.addHotel(HotelVO hotelVO)                                      | 添加酒店                 |
| hotelBLService.updateRoomInfo(Integer hotelId, String roomType,Integer rooms) | 修改剩余客房信息         |
| hotelBLService.retrieveHotels(Integer hotelId)                                | 获取所有酒店信息         |
| hotelBLService.retrieveHotelDetails(Integer hotelId)                          | 获取某家酒店详细信息     |
| hotelBLService.getRoomCurNum(Integer hotelId,String roomType)                 | 查看酒店某种房间剩余数量 |

##### couponView 模块的接口规范

需要的服务（需接口）

| 服务名                                                                       | 服务                                         |
| ---------------------------------------------------------------------------- | -------------------------------------------- |
| couponBLService.getMatchOrderCoupon(OrderVO orderVO)                         | 返回某一订单可用的优惠策略列表               |
| couponBLService.getHotelAllCoupon(Integer hotelId)                           | 查看某个酒店提供的所有优惠策略（包括失效的） |
| couponBLService.addHotelTargetMoneyCoupon(HotelTargetMoneyCouponVO couponVO) | 添加酒店满减优惠策略                         |

##### orderView 模块的接口规范

需要的服务（需接口）

| 服务名                                         | 服务                       |
| ---------------------------------------------- | -------------------------- |
| orderBLService.addOrder(OrderVO orderVO)       | 预定酒店时添加订单         |
| orderBLService.getAllOrders()                  | 获得所有订单信息           |
| orderBLService.getHotelOrders(Integer hotelId) | 查看指定酒店的所有订单     |
| orderBLService.getUserOrders(int userid)       | 获得指定用户的所有订单信息 |
| orderBLService.annulOrder(int orderid)         | 撤销订单                   |

### 业务逻辑层分解

业务逻辑层包含多个针对界面的业务逻辑处理对象。例如Admin对象负责处理酒店管理员界面的业务逻辑；Orders对象负责订单界面的业务逻辑。

![logic_layer](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_logic_layer.png)

#### 业务逻辑层模块的职责

|    模块    | 职责                                     |
| :--------: | :--------------------------------------- |
|  adminbl   | 负责实现管理员界面所需要的服务           |
|  couponbl  | 负责实现优惠券部分的业务逻辑             |
|  hotelbl   | 负责酒店页面所需要的服务                 |
|  orderbl   | 负责订单部分的业务逻辑                   |
|   userbl   | 负责酒店管理员，酒店用户界面所需要的服务 |
| questionbl | 负责用户的提问和回答逻辑                 |
|   VIPbl    | 负责VIP用户、企业用户的特权服务          |

#### 业务逻辑层的模块接口规范

##### AdminBl模块的接口规范

提供的服务（供接口）

* AdminService.addManager
  * 语法 : `ResponseVO addManager(UserForm userForm)`
  * 前置条件 : Admin已登录
  * 后置条件 : 在User数据库中插入Manager
* AdminService.addSalesPerson
  * 语法 : `ResponseVO addSalesPerson(UserForm userForm);`
  * 前置条件 : Admin已登录
  * 后置条件 : 在User数据库中找到所有的网站营销人员
* AdminService.getAllManagers
  * 语法 `List<User> getAllManagers()`
  * 前置条件 : Admin已登录
  * 后置条件 : 从User数据库中查找并返回所有的HotelManager
* AdminService.getAllSalesPerson()
  * 语法`List<User> getAllSalesPerson()`
  * 前置条件 : Admin已登录
  * 后置条件 : 从数据库中查找并返回所有的SalesPerson
* AdminService.deleteManager
  * 语法 : `ResponseVO deleteManager(Integer id);`
  * 前置条件 : Admin已登录
  * 后置条件 : 删除酒店工作人员
* AdminService.deleteSalesPerson
  * 语法 : `ResponseVO deleteSalesPerson(Integer id)`
  * 前置条件 : Admin已登录
  * 后置条件 : 删除网站营销人员

需要的服务（需接口）

|              服务名               | 服务                            |
| :-------------------------------: | :------------------------------ |
|    `AdminMapper.addManager()`     | 在数据库中插入AdminPO对象       |
|  `AdminMapper.getAllManagers()`   | 在数据库中查找所有的AdminPO对象 |
|  `AdminMapper.getAllManagers()`   | 获取所有的酒店管理人员信息      |
| `AdminMapper.getAllSalesPerson()` | 获取所有的网站营销人员信息      |
|   `AdminMapper.deleteManager()`   | 删除酒店工作人员                |
| `AdminMapper.deleteSalesPerson()` | 删除网站营销人员                |

##### CouponBl模块的接口规范

提供的服务（供接口）

* CouponService.getMatchOrderCoupon
  * 语法 : `public List<Coupon> getMatchOrderCoupon(OrderVO orderVO)`
  * 前置条件 : 从数据库中取得所有CouponPO对象
  * 后置条件 : 查找所有匹配的Coupon并返回CouponPO对象
* CouponService.getHotelAllCoupon
  * 语法 : `public List<Coupon> getHotelAllCoupon(Integer hotelId)`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 查找对应Hotel的Coupon并返回CouponPO对象
* CouponService.addHotelTargetMoneyCoupon
  * 语法 : `public CouponVO addHotelTargetMoneyCoupon(HotelTargetMoneyCouponVO couponVO)`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 向数据库中插入HotelTargetMoneyCoupon
* CouponService.addBirthdayCouponVO
  * 语法 : `CouponVO addBirthdayCouponVO(BirthdayCouponVO couponVO);`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 向数据库中插入BirthdayCoupon
* CouponService.addManyRoomCoupon
  * 语法 : `CouponVO addManyRoomCoupon(ManyRoomCouponVO couponVO);`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 向数据库中插入ManyRoomCoupon
* CouponService.addTimeCouponVO
  * 语法 : `CouponVO addTimeCouponVO(TimeCouponVO couponVO);`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 向数据库中插入TimeCoupon
* CouponService.addCorporateCouponVO
  * 语法 : `CouponVO addCorporateCouponVO(CorporateCouponVO couponVO);`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 向数据库中插入CorporateCoupon
* CouponService.addBizRegionCouponVO
  * 语法 : `CouponVO addBizRegionCouponVO(BizRegionCouponVO couponVO);`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 向数据库中插入BizRegionCoupon
* CouponService.getWebsiteCoupon
  * 语法 : `List<Coupon> getWebsiteCoupon();`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 从数据库中查找WebsiteCoupon
* CouponService.annulCoupon
  * 语法 : `void annulCoupon(Integer couponId);`
  * 前置条件 : 得到Coupon数据库的引用
  * 后置条件 : 将对应的Coupon设为无效

需要的服务（需接口）

|               服务名                | 服务                                  |
| :---------------------------------: | :------------------------------------ |
|    `CouponMatchStrategy.isMatch`    | 判断优惠券策略和需要的策略是否匹配    |
|   `CouponMapper.selectByHotelId`    | 根据hotelId来查找数据库中对应的coupon |
|     `CouponMapper.insertCoupon`     | 向数据库中插入Coupon对象              |
| `HotelService.retrieveHotelDetails` | 获取酒店详细信息                      |
|     `CouponMapper.getWebCoupon`     | 获取网站Coupon                        |
|     `CouponMapper.getBizRegion`     | 获取对应商圈的优惠券                  |

##### HotelBl模块的接口规范

提供的服务（供接口）

* HotelService.addHotel
  * 语法 : `void addHotel(HotelForm hotelForm) throws ServiceException;`
  * 前置条件 : 登录用户已被认证为管理员，获得Hotel数据库服务的引用
  * 后置条件 : 向Hotel数据库中插入HotelPO
* HotelService.updateRoomInfo
  * 语法 : `void updateHotelInfo(Integer hotelId, HotelForm hotelForm) throws  ServiceException;`
  * 前置条件 : 获得Room数据库服务的引用
  * 后置条件 : 向Room数据库中根据hotelId和roomType更新被预定的时间
* HotelService.updateHotelInfo
  * 语法 : `void updateHotelInfo(Integer hotelId, HotelForm hotelForm) throws  ServiceException`
  * 前置条件 : 登录用户被认证为酒店管理员
  * 后置条件 : 根据HotelForm内容更新酒店
* HotelService.deleteHotel
  * 语法 : `void deleteHotel(Integer hotelId)`
  * 前置条件 : 登录用户被认证为系统管理员
  * 后置条件 : 删除该酒店
* HotelService.retrieveHotels
  * 语法 : `public List<HotelVO> retrieveHotels()`
  * 前置条件 : 获得Hotel数据库服务的引用
  * 后置条件 : 获得所有HotelPO
* HotelService.retrieveHotelDetails
  * 语法 : `public HotelVO retrieveHotelDetails(Integer hotelId)`
  * 前置条件 : 获得Hotel和Room数据库服务的引用
  * 后置条件 : 根据HotelId获得其拥有的所有RoomPO
* HotelService.getRoomCurNum
  * 语法 : `public int getRoomCurNum(Integer hotelId, String roomType)`
  * 前置条件 : 获得Room数据库服务的引用
  * 后置条件 : 根据hotelId和roomType当前房间数量
* HotelService.retrieveAvailableHotelDetails
  * 语法 : `HotelVO retrieveAvailableHotelDetails(Integer hotelId, String beginTime, String endTime);`
  * 前置条件 : 获得hotel和Order数据库的服务引用
  * 后置条件 : 根据时间获得对应时间里的酒店房间数量
* HotelService.addComment
  * 语法 : `void addComment(CommentVO commentVO, Integer hotelId);`
  * 前置条件 : 获得Comment数据库的服务和引用，用户已下单
  * 后置条件 : 更新酒店的各类评分和comment次数
* HotelService.annulComment
  * 语法 : `void annulComment(CommentVO commentVO, Integer hotelId);`
  * 前置条件 : 获得Comment数据库的服务和引用，用户已评价
  * 后置条件 : 更新酒店的commentTime和各类评分
* RoomService.retrieveHotelRoomInfo
  * 语法 : `public List<HotelRoom> retrieveHotelRoomInfo(Integer hotelId)`
  * 前置条件 : 获得Room数据库服务的引用
  * 后置条件 : 根据HotelId获得RoomPO
* RoomService.retrieveHotelRoomInfoByType
  * 语法 : `List<HotelRoom> retrieveHotelRoomInfoByType(Integer hotelId, RoomType type);`
  * 前置条件 : 获得Room数据库服务的引用
  * 后置条件 : 根据HotelId获得RoomPO
* RoomService.insertRoomInfo
  * 语法 : `public void insertRoomInfo(HotelRoom hotelRoom)`
  * 前置条件 : 获得Room数据库服务的引用
  * 后置条件 : 向Room数据库中插入RoomPO
* RoomService.deleteRoom
  * 语法 : `void deleteRoom(Integer hotelId, String roomType);`
  * 前置条件 : 获得Room数据库服务的引用
  * 后置条件 : 在Room数据库中删除RoomPO
* RoomService.updateRoomInfo
  * 语法 : `public void updateRoomInfo(Integer hotelId, String roomType, Integer rooms)`
  * 前置条件 : 获得Room数据库服务的引用
  * 后置条件 : 根据roomType和roomId更新room的数量
* RoomService.getRoomCurNum
  * 语法 : `public int getRoomCurNum(Integer hotelId, String roomType)`
  * 前置条件 : 获得Room数据库服务的引用
  * 后置条件 : 根据hotelId和roomType获得Room的数量
* RoomService.getRoomCurNumByTime
  * 语法 : `Integer getRoomCurNumByTime(Integer hotelId, String beginTime, String endTime, String type);`
  * 前置条件 : 获得Room和Order数据库服务的引用
  * 后置条件 : 获得对应时间段内可用的房间数量
* HotelSearchService.searchHotel
  * 语法 : `List<HotelVO> searchHotel(SearchBodyVO searchBody);`
  * 前置条件 : 用户已登录
  * 后置条件 : 根据searchBody获得匹配的酒店列表

需要的服务（需接口）

|                服务名                |              服务              |
| :----------------------------------: | :----------------------------: |
|     `AccountService.getUserInfo`     | 根据ManagerId返回UserPO的数据  |
|      `HotelMapper.insertHotel`       | 向Hotel数据库中插入HotelPO对象 |
|     `HotelMapper.selectAllHotel`     |     获得所有hotelPO的对象      |
|    `HotelMapper.updateHotelName`     |          更新酒店名称          |
|   `HotelMapper.updateHotelAddress`   |          更新酒店地址          |
| `HotelMapper.updateHotelDescription` |          更新酒店描述          |
|      `HotelMapper.deleteHotel`       |            删除酒店            |
|   `HotelMapper.updateHotelPoints`    |          更新酒店评分          |
|       `HotelMapper.selectById`       |   根据HotelId获得HotelPO对象   |
|    `OrderService.getHotelOrders`     |     获得对应酒店的OrderPO      |
|     `OrderService.filterOrders`      |        筛选对应的Order         |
|  `RoomMapper.selectRoomsByHotelId`   |     根据HotelId获得RoomPO      |
|       `RoomMapper.insertRoom`        |    向Room数据库中插入RoomPO    |
|       `RoomMapper.deleteRoom`        |          删除对应房间          |
|     `RoomMapper.updateRoomInfo`      |          更新客房信息          |
|      `RoomMapper.getRoomCurNum`      |    获取客房当前可用房间数量    |

##### OrderBl模块的接口规范

提供的服务（供接口）

* OrderService.addOrder
  * 语法 : `public ResponseVO addOrder(OrderVO orderVO)`
  * 前置条件 : 获得Hotel, User和Order数据库的服务的引用
  * 后置条件 : 向Order数据库中添加OrderPO对象
* OrderService.getAllOrders
  * 语法 : `public List<Order> getAllOrders()`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 返回所有的订单
* OrderService.getUserOrders
  * 语法 : `public List<Order> getUserOrders(int userid)`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 根据UserId查找对应的Order信息
* OrderService.getHotelOrders
  * 语法 : `List<Order> getHotelOrders(Integer hotelId);`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 根据HotelId查找对应的Order信息
* OrderService.checkIn
  * 语法 : `ResponseVO checkIn(int orderId);`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 订单状态修改为已入住
* OrderService.probableAbnormalOrder
  * 语法 : `List<Order> probableAbnormalOrder(Integer hotelId);`
  * 前置条件 : 获得Order数据库的服务的引用，认证为管理员
  * 后置条件 : 获得酒店可能的异常订单
* OrderService.abnormalOrder
  * 语法 : `ResponseVO abnormalOrder(int orderId, double minCreditRatio);`
  * 前置条件 : 获得Order数据库的服务的引用，认证为管理员
  * 后置条件 : 标记为异常订单，扣除一定比例信誉
* OrderService.finishOrder
  * 语法 : `ResponseVO finishOrder(int orderId);`
  * 前置条件 : 获得Order数据库的服务的引用，认证为管理员
  * 后置条件 : 完成订单，添加信用
* OrderService.getComment
  * 语法 : `CommentVO getComment(int orderId);`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 获取订单的评价
* OrderService.annulComment
  * 语法 : `ResponseVO annulComment(int orderId);`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 删除该评价
* OrderService.getHotelComment
  * 语法 : `List<CommentVO> getHotelComment(int hotelId);`
  * 前置条件 : 获得Order数据库的服务的引用,认证为管理员
  * 后置条件 : 获得对应酒店的所有评价
* OrderService.addComment
  * 语法 : `ResponseVO addComment(CommentVO commentVO);`
  * 前置条件 : 获得Order数据库的服务的引用，订单已入住或完成
  * 后置条件 : 为订单添加评价
* OrderService.getUserComment
  * 语法 : `ResponseVO getUserComments(Integer userId);`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 获取用户的所有评价
* OrderService.getOrdersInMonth
  * 语法 : `List<List<Order>> getOrdersInMonth(List<Order> orders)`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 从订单输入流中返回近30天的订单
* OrderService.getOrdersInMonthOfHotel
  * 语法 : `List<List<Order>> getOrdersInMonthOfHotel(Integer hotelId);`
  * 前置条件 :获得Order数据库的服务的引用
  * 后置条件 : 获取对应酒店近30天的订单
* OrderService.getOrdersInMonthOfAll
  * 语法 : `List<List<Order>> getOrdersInMonthOfAll();`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 获取所有近30天的Order
* OrderService.filterOrders
  * 语法 : `List<Order> filterOrders(List<Order> orders,String beginTime, String endTime);`
  * 前置条件 : 获得Order数据库的服务的引用
  * 后置条件 : 找到与入住及退房时间有关联的订单
* OrderService.annulOrder
  * 语法 : `public ResponseVO annulOrder(int orderid)`
  * 前置条件 : 获得Hotel，User，Order数据库的服务的引用
  * 后置条件 : 从Order数据库中删除OrderPO对象，更新User的信誉积分和酒店房间信息

需要的服务（需接口）

|              服务名               |                    服务                    |
| :-------------------------------: | :----------------------------------------: |
|   `HotelService.getRoomCurNum`    |         获取酒店房间已被预订的时间         |
|   `AccountService.getUserInfo`    |         根据订单用户id获得用户信息         |
|   `HotelService.updateRoomInfo`   |                更新房间信息                |
|    `OrderMapper.getAllOrders`     |      从Order数据库中获取所有的OrderPO      |
|    `OrderMapper.getUserOrders`    | 从Order数据库中获得所有UserId匹配的OrderPO |
|      `OrderMapper.addOrder`       |                  添加订单                  |
|    `OrderMapper.getOrderById`     |             获取对应Order信息              |
|     `OrderMapper.annulOrder`      |                  撤销订单                  |
|       `OrderMapper.checkIn`       |                  办理入住                  |
|    `OrderMapper.abnormalOrder`    |               标记为异常订单               |
|     `OrderMapper.finishOrder`     |                  完成订单                  |
|     `OrderMapper.getComment`      |                获取订单评价                |
|    `OrderMapper.annulComment`     |                  撤销评价                  |
|   `OrderMapper.getHotelComment`   |                获取酒店评价                |
|    `OrderMapper.updateComment`    |                  修改评价                  |
| `RoomService.getRoomCurNumByTime` |       根据时间获取对应房间的可用数量       |
|   `AccountService.getUserInfo`    |                获取用户信息                |
|   `HotelService.updateRoomInfo`   |                更新房间信息                |
|    `HotelService.annulComment`    |              修改Hotel的评分               |
|     `HotelService.addComment`     |              修改Hotel的评分               |

##### UserBl模块的接口规范

提供的服务（供接口）

* AccountService.registerAccount
  * 语法 : `public ResponseVO registerAccount(UserVO userVO)`
  * 前置条件 : 得到User数据库的服务的引用，password和email符合规范
  * 后置条件 : 向User数据库中插入UserPO对象
* AccountService.login
  * 语法 : `public User login(UserForm userForm)`
  * 前置条件 : 得到User数据库服务的引用
  * 后置条件 : 查找是否存在相应的User，根据输入的password返回登陆验证的结果
* AccountService.getUserInfo
  * 语法 : `public User getUserInfo(int id)`
  * 前置条件 : 得到User数据库服务的引用
  * 后置条件 : 根据id查找User数据库，返回匹配的对象
* AccountService.updateUserInfo
  * 语法 : `ResponseVO updateUserInfo(int id, String username, String phoneNumber, String corporation);`
  * 前置条件 : 得到User数据库服务的引用
  * 后置条件 : 根据id查找User数据库，更新用户名、手机号、公司名
* AccountService.getUserInfoByEmail
  * 语法 : `UserVO getUserInfoByEmail(String email)`
  * 前置条件:得到User数据库服务的引用
  * 后置条件:根据email查找User数据库
* AccountService.updatePassword
  * 语法 : `ResponseVO updatePassword(int id, String password);`
  * 前置条件:得到User数据库服务的引用
  * 后置条件:更新加密后的密码
* AccountService.updateCredit
  * 语法 : `ResponseVO updateCredit(int id, double credit);`
  * 前置条件: 得到Credit数据库服务的引用
  * 后置条件: 更新用户信誉
* AccountService.argueCredit
  * 语法 : `ResponseVO argueCredit(int creditId, String argue);`
  * 前置条件: 得到Credit数据库服务的引用，Credit没有申诉过
  * 后置条件: 向网站管理员提出信誉变更申诉
* AccountService.getArgueCredits
  * 语法 : `List<CreditVO> getArgueCredits();`
  * 前置条件: 得到Credit数据库服务的引用
  * 后置条件: 获取所有申诉的Credit
* AccountService.handleArgue
  * 语法 : `ResponseVO handleArgue(Integer creditId, boolean accept);`
  * 前置条件: 得到Credit数据库服务的引用
  * 后置条件: 将Argue标记为已接受或拒绝接受
* AccountService.corporateVIP
  * 语法 : `ResponseVO corporateVIP(int id, String corporate);`
  * 前置条件: 得到VIP数据库服务的引用
  * 后置条件: 将企业标记为CorporateVIP
* AccountService.updateBirthday
  * 语法 : `void updateBirthday(int id, String birthday)`
  * 前置条件: 得到User数据库服务的引用
  * 后置条件: 更新用户生日
* AccountService.registerAsVIP
  * 语法 : `void registerAsVIP(int id);`
  * 前置条件: 得到VIP数据库服务的引用
  * 后置条件: 将用户注册为VIP
* AccountService.freezeVIP
  * 语法 : `void freezeVIP(int id);`
  * 前置条件: 得到VIP数据库服务的引用
  * 后置条件: 停止用户的VIP服务
* AccountService.updatePortrait
  * 语法 : `ResponseVO updatePortrait(int userId, String url);`
  * 前置条件:得到User数据库服务的引用
  * 后置条件: 更新头像
* AccountService.chargeCredit
  * 语法 : `ResponseVO chargeCredit(int userId, int change, String reason);`
  * 前置条件: 得到Credit数据库服务的引用
  * 后置条件: 添加用户的信誉变更记录
* AccountService.getUserCreditChanges
  * 语法 : `ResponseVO getUserCreditChanges(int userId);`
  * 前置条件: 得到Credit数据库服务的引用
  * 后置条件: 获取用户的信誉变更记录
* AccountService.getAllUsers
  * 语法 : `List<UserVO> getAllUsers();`
  * 前置条件:得到User数据库服务的引用
  * 后置条件: 获取所有UserPO
* AccountService.getAllPhoneNumOfSalesPerson
  * 语法 : `List<String> getAllPhoneNumOfSalesPerson();`
  * 前置条件:得到User数据库服务的引用
  * 后置条件:获取所有网站营销人员的电话号
* CollectionService.addCollection
  * 语法 :`ResponseVO addCollection(CollectionVO collectionVO);`
  * 前置条件: 获取Collection数据库服务的引用
  * 后置条件:添加Collection
* CollectionService.getUserCollection
  * 语法 :`List<CollectionVO> getUserCollection(Integer userId);`
  * 前置条件:获取Collection数据库服务的引用
  * 后置条件:获取用户的所有收藏
* CollectionService.getHotelCollection
  * 语法 :`Integer getHotelCollection(Integer hotelId);`
  * 前置条件:获取Collection数据库服务的引用
  * 后置条件:获取酒店的所有收藏数量
* CollectionService.userCollection
  * 语法 :`Boolean userCollection(Integer userId, Integer hotelId);`
  * 前置条件:获取Collection数据库服务的引用
  * 后置条件:判断用户是否收藏了该酒店
* CollectionService.annulCollectionByUserId
  * 语法 :`ResponseVO annulCollectionByUserId(Integer hotelId, Integer userId);`
  * 前置条件:获取Collection数据库服务的引用，用户已收藏该酒店
  * 后置条件:删除用户对这家酒店的收藏

需要的服务（需接口）

|                服务名                 |                    服务                    |
| :-----------------------------------: | :----------------------------------------: |
|      `BeanUtils.copyProperties`       |            从userVO拷贝到user里            |
|   `AccountMapper.createNewAccount`    |      在数据库中插入新账号的UserPO对象      |
|   `AccountMapper.getAccountByName`    |         通过user的名称取到User对象         |
|   `AccountMapper.getAccountByEmail`   |           通过email取到User对象            |
|    `AccountMapper.updatePassword`     |                  更新密码                  |
|     `AccountMapper.updateCredit`      |                更新信用变更                |
|    `AccountMapper.updateCorproate`    |                更新企业信息                |
|    `AccountMapper.updateBirthday`     |                  更新生日                  |
|     `AccountMapper.updateVIPType`     |                更新VIP类型                 |
|    `AccountMapper.updatePortrait`     |                更新头像url                 |
|     `AccountMapper.chargeCredit`      |               获取Credit记录               |
|      `AccountMapper.getAllUsers`      |               获取所有的User               |
|    `AccountMapper.getAccountById`     |          通过user的id取到User对象          |
|     `AccountMapper.updateAccount`     | 找到对应id的用户，更新用户名、手机号、密码 |
|       `CreditMapper.getCredit`        |        根据creditId获取信用变更记录        |
|      `CreditMapper.argueCredit`       |                信用变更申诉                |
|    `CreditMapper.getArgueCredits`     |           获取申诉的信誉变更记录           |
|      `CreditMapper.handleArgue`       |              处理信誉变更申诉              |
|       `CreditMapper.addCredit`        |              添加信誉变更记录              |
|     `CreditMapper.getUserCredit`      |           获取用户的信誉变更记录           |
|   `CollectionMapper.addCollection`    |                  添加收藏                  |
|  `CollectionMapper.annulCollection`   |                  撤销收藏                  |
|  `CollectionMapper.getUseCollection`  |               获取用户的收藏               |
| `CollectionMapper.getHotelCollection` |             获取酒店的所有收藏             |

##### QuestionBl模块的接口规范

提供的服务（供接口）

* AnswerService.addAnswer
  * 语法 : `ResponseVO addAnswer(AnswerVO answerVO);`
  * 前置条件 :获得Answer数据库的引用
  * 后置条件 :添加Answer
* AnswerService.getAllAnswers
  * 语法 : `List<AnswerVO> getAllAnswers();`
  * 前置条件 :获得Answer数据库的引用
  * 后置条件 :获取所有的Answer
* AnswerService.getQuestionAnswers
  * 语法 : `List<AnswerVO> getQuestionAnswers(Integer questionId);`
  * 前置条件 :获得Answer数据库的引用
  * 后置条件 :获得对应问题的所有回答
* AnswerService.annulAnswer
  * 语法 : `ResponseVO annulAnswer(Integer answerId);`
  * 前置条件 : 获得Answer数据库的引用
  * 后置条件 : 删除回答
* QuestionService.addQuestion
  * 语法 : `ResponseVO addQuestion(QuestionForm questionForm);`
  * 前置条件 : 获得Question数据库服务的引用
  * 后置条件 : 添加问题
* QuestionService.annulQuestion
  * 语法 : `ResponseVO annulQuestion(Integer questionId);`
  * 前置条件 :获得Question数据库服务的引用
  * 后置条件 : 将问题标记为不可达
* QuestionService.getHotelQuestion
  * 语法 : `List<QuestionVO> getHotelQuestion(Integer hotelId);`
  * 前置条件 : 获得Question和Answer数据库服务的引用
  * 后置条件 : 获取酒店的所有问题和对应答案
* QuestionService.getUserQuestion
  * 语法 : `List<QuestionVO> getUserQuestion(Integer userId);`
  * 前置条件 : 获得Question和Answer数据库服务的引用
  * 后置条件 : 获取用户的所有问题和对应答案

需要的服务（需接口）

|              服务名               |        服务        |
| :-------------------------------: | :----------------: |
|     `AnswerMapper.addAnswer`      |      添加回答      |
|   `AnswerMapper.getAllAnswers`    |   获取所有的回答   |
| `AnswerMapper.getQuestionAnswers` | 获得问题的所有回答 |
|    `AnswerMapper.annulAnswer`     |      删除回答      |
|   `QuestionMapper.addQuestion`    |      添加问题      |
|  `QuestionMapper.annulQuestion`   |      删除问题      |
| `QuestionMapper.getHotelQuestion` |   获取酒店的问题   |
| `QuestionMapper.getUserQuestion`  |   获取用户的问题   |

##### VIPBl模块的接口规范

提供的服务（供接口）

* LevelService.formulateVIPLevel
  * 语法：`ResponseVO formulateVIPLevel(Integer level, String type, Integer requestConsumption, double reduction);`
  * 前置条件：
  * 后置条件：
* LevelService.getTheRequestOfLevel
  * 语法：`ResponseVO getTheRequestOfLevel(Integer level, String type);`
  * 前置条件:获得VIPLevel数据库服务的引用
  * 后置条件：获取对应VIP等级需要的消费额
* VIPService.registerAsClientVIP
  * 语法：`ResponseVO registerAsClientVIP(Integer userId, String Birthday);`
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：注册为普通会员
* VIPService.freezeClientVIP
  * 语法：`ResponseVO freezeClientVIP(Integer userId);`
  * 前置条件：获得VIP数据库服务的引用，被认证为网站管理员
  * 后置条件：停用该普通会员
* VIPService.restoreClientVIP
  * 语法：`ResponseVO restoreClientVIP(Integer userId);`
  * 前置条件：获得VIP数据库服务的引用，被认证为网站管理员，会员被冻结
  * 后置条件：恢复普通会员的特权
* VIPService.getVIPByUserId
  * 语法：`ResponseVO getVIPbyUserId(Integer userId);`
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：根据ID获取会员情况
* VIPService.addVIPClientConsumption
  * 语法：`ResponseVO addVIPClientConsumption(Integer userId, Integer amount);`
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：添加个人会员的消费额度
* VIPService.registerAsCorpVIP
  * 语法：`ResponseVO registerAsCorpVIP(String corporationName);`
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：注册为企业会员
* VIPService.freezeCorpVIP
  * 语法：`ResponseVO freezeCorpVIP(String corporationName);`
  * 前置条件：获得VIP数据库服务的引用，被认证为网站管理员
  * 后置条件：停用企业会员服务
* VIPService.restoreCorpVIP
  * 语法：`ResponseVO restoreCorpVIP(String corporationName);`
  * 前置条件：获得VIP数据库服务的引用，被认证为网站管理员，会员被冻结
  * 后置条件：恢复企业会员服务
* VIPService.getVIPByCorpName
  * 语法：`ResponseVO getVIPbyCorpName(String corporationName);`
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：根据企业名获取VIP类型
* VIPService.addVIPCorpConsumption
  * 语法：`ResponseVO addVIPCorpConsumption(String corporationName, Integer amount);`
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：添加企业VIP消费
* VIPService.VIPCorpCheck
  * 语法：`boolean VIPCorpCheck(String corporationName);`
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：检查企业是否是VIP
* VIPService.getAllVIPClient
  * 语法：ResponseVO getAllVIPClient();``
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：获取所有个人VIP
* VIPService.getAllVIPCorp
  * 语法：`ResponseVO getAllVIPCorp();`
  * 前置条件：获得VIP数据库服务的引用
  * 后置条件：获取所有企业VIP

需要的服务（需接口）

|               服务名                |           服务           |
| :---------------------------------: | :----------------------: |
| `LevelMapper.getTheRequestOfLevel`  | 获取对应等级需要的消费额 |
|      `LevelMapper.changeLevel`      |       修改VIP等级        |
|   `LevelMapper.formulateVIPLevel`   |   修改VIP需要的消费额    |
|   `LevelMapper.getTheReduOfLevel`   |  获取对应VIP的折扣额度   |
|   `AccountService.registerAsVIP`    |      注册成VIP客户       |
|   `AccountService.updateBirthday`   |       更新生日信息       |
|     `AccountService.freezeVIP`      |         停用VIP          |
|   `VIPMapper.registerAsClientVIP`   |      注册成VIP客户       |
|     `VIPMapper.freezeClientVIP`     |       停用VIP服务        |
|    `VIPMapper.restoreClientVIP`     |       恢复VIP服务        |
|     `VIPMapper.getVIPByUserId`      |    根据用户ID获取VIP     |
| `VIPMapper.addVIPClientConsumption` |       添加消费记录       |
|      `VIPMapper.clientLevelUp`      |       升级VIP等级        |
|    `VIPMapper.registerAsCorpVIP`    |      注册成企业VIP       |
|      `VIPMapper.freezeCorpVIP`      |     停用企业VIP服务      |
|     `VIPMapper.restoreCorpVIP`      |     恢复企业VIP服务      |
|    `VIPMapper.getVIPByCorpName`     |  根据企业名获取VIP服务   |
|  `VIPMapper.addVIPCorpConsumption`  |     升级企业VIP等级      |
|  `VIPMapper.addVIPCorpConsumption`  |     添加企业消费额度     |
|       `VIPMapper.corpLevelUp`       |     升级企业VIP等级      |
|     `VIPMapper.getAllVIPClient`     |    获取所有的个人VIP     |
|      `VIPMapper.getAllVIPCorp`      |    获取所有的企业VIP     |

### 数据层分解

数据层主要给业务逻辑层提供数据访问服务，包括对持久化数据的增、删、改、查。Admin业务逻辑需要的服务由AdminMapper接口提供。本酒店房间预订系统主要以数据库形式存储。数据层模块的描述具体如下图：

![data_layer](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/se_data_layer.png)

#### 数据层模块的职责

数据层模块的职责如下表所示：

| 模块                   | 职责                                                                            |
| ---------------------- | ------------------------------------------------------------------------------- |
| `AdminMapper`          | 持久化数据库的接口，提供集体载入、集体保存、增、删、改、查服务                  |
| `AdminMapperMySqlImpl` | 基于MySql数据库的持久化数据库的接口，提供集体载入、集体保存、增、删、改、查服务 |

#### 数据层模块的接口规范

数据层模块的接口规范如下表所示：

**提供的服务（供接口）**

| `AdminMapper.createNewAccount` | 语法     | `public UserPO createNewAccount() throws RemoteException;` |
| ------------------------------ | -------- | ---------------------------------------------------------- |
|                                | 前置条件 | 每一个历史用户的ID存在且唯一                               |
|                                | 后置条件 | 在数据库中插入新账号的UserPO对象                           |

| `AdminMapper.getAccountByName` | 语法     | `public UserVO getAccountByName(String email) throws RomoteException;` |
| ------------------------------ | -------- | ---------------------------------------------------------------------- |
|                                | 前置条件 | 每一个历史用户的邮箱存在且唯一                                         |
|                                | 后置条件 | 通过user的email取到UserVO对象                                          |

| `AdminMapper.getAccountById` | 语法     | `public UserPO getAccountById(long id) throws RomoteException;` |
| ---------------------------- | -------- | --------------------------------------------------------------- |
|                              | 前置条件 | 每一个历史用户的ID存在且唯一                                    |
|                              | 后置条件 | 通过ID查找一个用户，并返回UserPO                                |

| `AdminMapper.updateAccount` | 语法     | `public void updateAccount(long id) throws RemoteException；` |
| --------------------------- | -------- | ------------------------------------------------------------- |
|                             | 前置条件 | 每一个历史用户（po）的ID存在且唯一                            |
|                             | 后置条件 | 找到对应id的用户，更新用户名、手机号、密码                    |

| `AdminMapper.delete` | 语法     | `public void delete(UserPO po) throws RemoteException;` |
| -------------------- | -------- | ------------------------------------------------------- |
|                      | 前置条件 | 每一个历史用户（po）的ID存在且唯一                      |
|                      | 后置条件 | 删除一个用户（po）（但ID不会重新分发）                  |

| `AdminMapper.init` | 语法     | `public void init() throws RemoteException；` |
| ------------------ | -------- | --------------------------------------------- |
|                    | 前置条件 | 无                                            |
|                    | 后置条件 | 初始化持久化数据库                            |

| `AdminMapper.finish` | 语法     | `public void finish() RemoteException;` |
| -------------------- | -------- | --------------------------------------- |
|                      | 前置条件 | 无                                      |
|                      | 后置条件 | 结束持久化数据库的使用                  |

## 信息视角

### 数据持久化对象

系统的PO类就是对应的相关的实体类。

* Coupon类包含优惠券的id, 描述，hotelId，优惠券类型，优惠券名称，优惠券使用门槛，折扣，优惠金额，可用时间，失效时间，优惠券状态。
* Hotel类包含旅店的id，名称，地址，商圈，星级，好评率，描述，联系电话，管理员Id
* HotelRoom类包含房间的id，类型，旅店ID，价格，剩余可预订房间数，该类型房间总数
* Order类包含房间的ID，预订客户的ID，旅店的ID，入住日期，退房日期，房间类型，房间数量，人数，是否有儿童，ID创建时间，客户姓名，联系电话，订单状态
* User类包含用户的ID，邮箱(用户名)，密码，姓名，电话号，信誉积分，用户类型

例如持久化对象HotelRoom的定义如下：

```java
public class HotelRoom {
    private Integer id;
    private RoomType roomType;
    private Integer hotelId;
    private double price;
    /** 当前剩余可预定房间数 */
    private int curNum;
    /** 某类型房间总数 */
    private int total;

    public Integer getId() { return id; }
    public void setId(Integer id) { this.id = id; }
    public RoomType getRoomType() { return roomType; }
    public void setRoomType(RoomType roomType) { this.roomType = roomType; }
    public Integer getHotelId() { return hotelId; }
    public void setHotelId(Integer hotelId) { this.hotelId = hotelId; }
    public double getPrice() { return price; }
    public void setPrice(double price) { this.price = price; }
    public int getCurNum() { return curNum; }
    public void setCurNum(int curNum) { this.curNum = curNum; }
    public int getTotal() { return total; }
    public void setTotal(int total) { this.total = total; }
}
```

### 数据库表

数据库中包含Answer表，Collections表，Coupon表，Credits表，Hotel表，Orderlist表，Questions表，Room表，User表，VIP表，VIPLevel表

#### Answers表

|    属性    |     类型     |
| :--------: | :----------: |
|  answerId  |   int(11)    |
|   userId   |   int(11)    |
| questionId |   int(11)    |
|   answer   | varchar(255) |

#### Collections表

|  属性   |  类型   |
| :-----: | :-----: |
|   id    | int(11) |
| userID  | int(11) |
| hotelID | int(11) |

#### coupon表

|      属性       |     类型     |
| :-------------: | :----------: |
|       id        |   int(11)    |
|   description   | varchar(255) |
|     hotelId     |   int(11)    |
|   couponType    |   int(11)    |
|   couponName    | varchar(255) |
|  target_money   |   int(11)    |
| target_room_num |   int(11)    |
|    discount     |    double    |
|     status      |   int(11)    |
|   start_time    |   datetime   |
|    end_time     |   datetime   |
| discount_money  |   int(11)    |
| corporate_name  | varchar(255) |
|    bizRegion    | varchar(255) |
|    vipLevel     |   int(11)    |

#### Credits表

|    属性    |     类型     |
| :--------: | :----------: |
|     id     |   int(11)    |
|   userId   |   int(11)    |
| changeDate | varchar(255) |
|   change   |   int(11)    |
|    now     |    double    |
|   reason   | varchar(255) |
|   status   |    int(5)    |
|   argue    | varchar(255) |

#### hotel表

|       属性       |     类型     |
| :--------------: | :----------: |
|        id        |   int(11)    |
|    hotelName     | varchar(255) |
| hotelDescription | varchar(255) |
|     address      | varchar(255) |
|    bizRegion     | varchar(255) |
|    hotelStar     | varchar(255) |
|     phoneNum     |   int(11)    |
|       rate       |    double    |
|    manager_id    |   int(11)    |
|   commentTime    |   int(11)    |
|    sanitation    |    double    |
|   environment    |    double    |
|     service      |    double    |
|    equipment     |    double    |
|     picture      | varchar(255) |

#### orderList表

|     属性     |     类型     |
| :----------: | :----------: |
|      id      |   int(11)    |
|    userId    |   int(11)    |
|   hotelId    |   int(11)    |
|  hotelName   | varchar(255) |
| checkInDate  | varchar(255) |
| checkOutDate | varchar(255) |
|   roomType   | varchar(255) |
|   roomNum    |   int(255)   |
|  peopleNum   |   int(255)   |
|  haveChild   |   tinytext   |
|  createDate  | varchar(255) |
|    price     | decimal(65)  |
|  clientName  | varchar(255) |
| phoneNumber  | varchar(255) |
|  orderState  | varchar(255) |
|   comments   | varchar(255) |
|    points    |    int(5)    |
|  sanitation  |    int(5)    |
| environment  |    int(5)    |
|   service    |    int(5)    |
|  equipment   |    int(5)    |

#### Question表

|   属性    |     类型     |
| :-------: | :----------: |
|    id     |   int(11)    |
|  userID   |   int(11)    |
| userName  | varchar(255) |
|  hotelID  |   int(11)    |
| question  | varchar(255) |
| available |  tinyint(1)  |

#### room表

|   属性    |    类型     |
| :-------: | :---------: |
|    id     |   int(1)    |
|   price   |   double    |
|  curNum   |   int(11)   |
|   total   |   int(11)   |
| hotel_id  |   int(11)   |
| roomType  | varchar(50) |
|  bedType  | varchar(50) |
| breakfast | varchar(50) |
| peopleNum |   int(11)   |

#### user表

|    属性     |     类型     |
| :---------: | :----------: |
|     id      |   int(11)    |
|    email    | varchar(255) |
|  password   | varchar(11)  |
|  username   | varchar(255) |
| phonenumber | varchar(255) |
|   credit    | double(255)  |
|  usertype   | varchar(255) |
|  birthday   | varchar(255) |
| corporation | varchar(255) |
|  annulTime  |    int(5)    |
|  jobNumber  |   int(11)    |
|   hotelID   |   int(11)    |
|  portrait   | varchar(255) |
|   vipType   | varchar(255) |

#### VIP表

|    属性     |     类型     |
| :---------: | :----------: |
|     id      |   int(11)    |
|    type     | varchar(255) |
|   userId    |   int(11)    |
| corporation |   int(11)    |
|   status    |   int(11)    |
|  reduction  |    double    |

#### VIPLevel表

|        属性        |     类型     |
| :----------------: | :----------: |
|       level        |   int(11)    |
|        type        | varchar(255) |
| requestConsumption |   int(11)    |
|     reduction      |    double    |
