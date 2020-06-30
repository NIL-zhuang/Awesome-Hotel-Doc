# Awesome Hotel详细设计文档

|    修改人员    |    日期    |        修改原因        | 版本号 |
| :------------: | :--------: | :--------------------: | :----: |
| 庄子元，程荣鑫 | 2020.05.17 |       文档初始化       |  v0.1  |
|     庄子元     | 2020.05.21 | 初始业务逻辑层接口规范 | v0.2.1 |
|     程荣鑫     | 2020.05.22 |    结构视角部分图片    | v0.2.2 |
| 庄子元，程荣鑫 | 2020.06.18 |     逻辑新功能补充     |  v0.3  |
| 庄子元，程荣鑫 | 2020.06.22 |  question, vip新逻辑   |  v0.4  |
| 庄子元，程荣鑫 | 2020.06.28 |      文档代码核对      |  v0.5  |
| 庄子元，程荣鑫 | 2020.06.29 | 删除冗余代码和对应文档 |  v1.0  |

[TOC]

## 1. 引言

### 1.1 编制目的

本报告详细完成对Awesome Hotel酒店预定系统的详细设计，达到指导后续软件构造的目的，同时实现和测试人员及用户的沟通。

本报告面向开发人员、测试人员及最终用户而编写，是了解系统的导航。

本报告挑选主要内容阐述，不一定涵盖系统的每个细节。

### 1.2 词汇表

|     名称      |     含义     |
| :-----------: | :----------: |
| Awesome Hotel | 酒店预定系统 |

### 1.3 参考资料

## 2 产品概述

参考Awesome Hotel酒店预定系统用例文档和软件需求规格说明文档中对产品的概括描述。

## 3. 体系结构设计概述

参考Awesome Hotel酒店预定系统体系结构文档中对体系结构设计的概述。

## 4. 结构视角

### 4.1 业务逻辑层的分解

开发包图在软件体系结构文档中已经给出。

根据体系结构的设计，我们将系统分为展示层、业务逻辑层、数据层。每一层之间为了增加灵活性，我们会添加接口。presentation层与bussiness logic层之间的接口为blservice，bussiness logic层与data层的接口即为mapper。为了隔离业务逻辑职责和逻辑控制职责，我们开发了controller包，里面有各自模块的controller。所有后端返回给前端的对象都被封装进ResonseVO类中，方便前端统一处理。下面的部分阐述具体的模块。

#### 4.1.1 Adminbl模块

##### 4.1.1.1 概述

adminbl模块承担的需求参见需求规格说明文档功能需求和相关非功能需求。

adminbl模块的接口参见软件体系结构描述文档中的接口表。

##### 4.1.1.2 整体结构

adminbl主要功能是管理酒店工作人员和网站营销人员账户。
UserForm是前端传回的用户信息对象，由AdminController调用业务逻辑层方法，将UserForm转化为User，User对象被设计为持久化对象，存储在数据库中。

- 设计图

![adminBL](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/adminbl.png)

- 类的职责

|       模块       |                  职责                  |
| :--------------: | :------------------------------------: |
| AdminServiceImpl | 实现系统管理员管理工作人员所需要的服务 |

- 接口规范

提供的服务（供接口）

- AdminService.addManager
  - 语法 : `ResponseVO addManager(UserForm userForm)`
  - 前置条件 : Admin已登录
  - 后置条件 : 在User数据库中插入Manager
- AdminService.addSalesPerson
  - 语法 : `ResponseVO addSalesPerson(UserForm userForm);`
  - 前置条件 : Admin已登录
  - 后置条件 : 在User数据库中找到所有的网站营销人员
- AdminService.getAllManagers
  - 语法 `List<User> getAllManagers()`
  - 前置条件 : Admin已登录
  - 后置条件 : 从User数据库中查找并返回所有的HotelManager
- AdminService.getAllSalesPerson()
  - 语法`List<User> getAllSalesPerson()`
  - 前置条件 : Admin已登录
  - 后置条件 : 从数据库中查找并返回所有的SalesPerson
- AdminService.deleteManager
  - 语法 : `ResponseVO deleteManager(Integer id);`
  - 前置条件 : Admin已登录
  - 后置条件 : 删除酒店工作人员
- AdminService.deleteSalesPerson
  - 语法 : `ResponseVO deleteSalesPerson(Integer id)`
  - 前置条件 : Admin已登录
  - 后置条件 : 删除网站营销人员

需要的服务（需接口）-- 无

- 业务逻辑层的动态模型

    管理HotelManger相关方法和管理SalesPerson的差不多，这里省略后者。

    - addManager顺序图

    ![addManager](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/addManager.png)

    - getAllManager顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getAllManagers.png)

    - deleteHotelManager顺序图

    ![deleteManager](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/deleteHotelManger.png)

    - Admin状态图

    ![admin](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/admin.png)

- 业务逻辑层的设计原理

    利用委托式控制风格，每个界面需要访问的业务逻辑由各自的BLServiceImpl委托给不同的领域对象。

#### 4.1.2 Couponbl模块

##### 4.1.2.1 概述

Couponbl模块承担的需求参见需求规格说明文档功能需求和相关非功能需求。
Couponbl模块的接口参见软件体系结构描述文档中的接口表。

##### 4.1.2.2 整体结构

Couponbl主要功能是网站工作人员、酒店工作人员设立VIP优惠、促销优惠券业务。
CouponVO是前端传回的用户信息对象，分为BirthdayCouponVO, TimeVO, BizRegionVO, CorporateCouponVO, HotelTargetMoneyCouponVO, ManyRoomCouponVO，由CouponController调用业务逻辑层方法，将CouponVO转化为Coupon，Coupon对象被设计为持久化对象，存储在数据库中。

- 设计图

![CouponBL](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/couponbl.png)

- 类的职责

|             模块              |             职责              |
| :---------------------------: | :---------------------------: |
|  BirthdayCouponStrategyImpl   |   生日特惠优惠券的判断逻辑    |
|  BizRegionCouponStrategyImpl  |      商圈VIP优惠业务逻辑      |
|  CorporateCouponServiceImpl   |      企业优惠券业务逻辑       |
|   ManyRoomCouponServiceImpl   |     多间房间优惠业务逻辑      |
| TargetMoneyCouponServiceImpl  |       满减优惠业务逻辑        |
| TimeCouponStrategyServiceImpl |      限时优惠券业务逻辑       |
|       CouponServiceImpl       |      优惠券管理业务逻辑       |
|       BirthdayCouponVO        |    生日优惠券领域模型对象     |
|       BizRegionCouponVO       | 特定商圈VIP优惠券领域模型对象 |
|       CorporateCouponVO       |   企业VIP优惠券领域模型对象   |
|   HotelTargetMoneyCouponVO    |    满减优惠券领域模型对象     |
|        ManyRoomCoupon         |    多间优惠券领域模型对象     |
|         TimeCouponVO          |    限时优惠券领域模型对象     |
|            Coupon             |     优惠券的领域模型对象      |

- 接口规范

提供的服务（供接口）

- CouponService.getMatchOrderCoupon
  - 语法 : `public List<Coupon> getMatchOrderCoupon(OrderVO orderVO)`
  - 前置条件 : 从数据库中取得所有CouponPO对象
  - 后置条件 : 查找所有匹配的Coupon并返回CouponPO对象
- CouponService.getHotelAllCoupon
  - 语法 : `public List<Coupon> getHotelAllCoupon(Integer hotelId)`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 查找对应Hotel的Coupon并返回CouponPO对象
- CouponService.addHotelTargetMoneyCoupon
  - 语法 : `public CouponVO addHotelTargetMoneyCoupon(HotelTargetMoneyCouponVO couponVO)`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 向数据库中插入HotelTargetMoneyCoupon
- CouponService.addBirthdayCouponVO
  - 语法 : `CouponVO addBirthdayCouponVO(BirthdayCouponVO couponVO);`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 向数据库中插入BirthdayCoupon
- CouponService.addManyRoomCoupon
  - 语法 : `CouponVO addManyRoomCoupon(ManyRoomCouponVO couponVO);`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 向数据库中插入ManyRoomCoupon
- CouponService.addTimeCouponVO
  - 语法 : `CouponVO addTimeCouponVO(TimeCouponVO couponVO);`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 向数据库中插入TimeCoupon
- CouponService.addCorporateCouponVO
  - 语法 : `CouponVO addCorporateCouponVO(CorporateCouponVO couponVO);`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 向数据库中插入CorporateCoupon
- CouponService.addBizRegionCouponVO
  - 语法 : `CouponVO addBizRegionCouponVO(BizRegionCouponVO couponVO);`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 向数据库中插入BizRegionCoupon
- CouponService.getWebsiteCoupon
  - 语法 : `List<Coupon> getWebsiteCoupon();`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 从数据库中查找WebsiteCoupon
- CouponService.annulCoupon
  - 语法 : `void annulCoupon(Integer couponId);`
  - 前置条件 : 得到Coupon数据库的引用
  - 后置条件 : 将对应的Coupon设为无效

需要的服务（需接口）

|               服务名                |       服务       |
| :---------------------------------: | :--------------: |
| `HotelService.retrieveHotelDetails` | 获取酒店详细信息 |

- 业务逻辑层的动态模型

    - addCoupon顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/addCoupon.png)

    - annulCoupon顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/annulCoupon.png)

    - getOrderMatchCoupon顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getOrderMatchCoupons.png)

    - Coupon状态图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/coupon.png)

- 业务逻辑层的设计原理

    利用委托式控制风格，每个界面需要访问的业务逻辑由各自的BLServiceImpl委托给不同的领域对象。

#### 4.1.3 hotelbl模块

##### 4.1.3.1 概述

Hotelbl模块承担的需求参见需求规格说明文档功能需求和相关非功能需求。

Hotelbl模块的接口参见软件体系结构描述文档中的接口表。

##### 4.1.3.2 整体结构

Hotelbl主要功能是酒店和酒店房间的管理业务。

HotelForm是前端传回的酒店信息对象，HotelVO和RoomVO是传递酒店信息对象，由HotelController调用业务逻辑层方法，将HotelVO, RoomVO转化为Hotel和Room，他们被设计为持久化对象，存储在数据库中。

- 设计图

![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/hotelbl.png)

- 类的职责

|        模块        |            职责            |
| :----------------: | :------------------------: |
| HotelSearchService |      酒店房间搜索业务      |
|  HotelServiceImpl  | 酒店管理和信息获取业务逻辑 |
|   RoomServicImpl   |      酒店房间业务逻辑      |
|       Hotel        |     酒店的领域模型对象     |
|        Room        |   酒店房间的领域模型对象   |

- 接口规范

- HotelService.addHotel
  - 语法 : `void addHotel(HotelForm hotelForm) throws ServiceException;`
  - 前置条件 : 登录用户已被认证为管理员，获得Hotel数据库服务的引用
  - 后置条件 : 向Hotel数据库中插入HotelPO
- HotelService.updateRoomInfo
  - 语法 : `void updateHotelInfo(Integer hotelId, HotelForm hotelForm) throws  ServiceException;`
  - 前置条件 : 获得Room数据库服务的引用
  - 后置条件 : 向Room数据库中根据hotelId和roomType更新被预定的时间
- HotelService.updateHotelInfo
  - 语法 : `void updateHotelInfo(Integer hotelId, HotelForm hotelForm) throws  ServiceException`
  - 前置条件 : 登录用户被认证为酒店管理员
  - 后置条件 : 根据HotelForm内容更新酒店
- HotelService.deleteHotel
  - 语法 : `void deleteHotel(Integer hotelId)`
  - 前置条件 : 登录用户被认证为系统管理员
  - 后置条件 : 删除该酒店
- HotelService.retrieveHotels
  - 语法 : `public List<HotelVO> retrieveHotels()`
  - 前置条件 : 获得Hotel数据库服务的引用
  - 后置条件 : 获得所有HotelPO
- HotelService.retrieveHotelDetails
  - 语法 : `public HotelVO retrieveHotelDetails(Integer hotelId)`
  - 前置条件 : 获得Hotel和Room数据库服务的引用
  - 后置条件 : 根据HotelId获得其拥有的所有RoomPO
- HotelService.getRoomCurNum
  - 语法 : `public int getRoomCurNum(Integer hotelId, String roomType)`
  - 前置条件 : 获得Room数据库服务的引用
  - 后置条件 : 根据hotelId和roomType当前房间数量
- HotelService.addComment
  - 语法 : `void addComment(CommentVO commentVO, Integer hotelId);`
  - 前置条件 : 获得Comment数据库的服务和引用，用户已下单
  - 后置条件 : 更新酒店的各类评分和comment次数
- HotelService.annulComment
  - 语法 : `void annulComment(CommentVO commentVO, Integer hotelId);`
  - 前置条件 : 获得Comment数据库的服务和引用，用户已评价
  - 后置条件 : 更新酒店的commentTime和各类评分
- RoomService.retrieveHotelRoomInfo
  - 语法 : `public List<HotelRoom> retrieveHotelRoomInfo(Integer hotelId)`
  - 前置条件 : 获得Room数据库服务的引用
  - 后置条件 : 根据HotelId获得RoomPO
- RoomService.retrieveHotelRoomInfoByType
  - 语法 : `List<HotelRoom> retrieveHotelRoomInfoByType(Integer hotelId, RoomType type);`
  - 前置条件 : 获得Room数据库服务的引用
  - 后置条件 : 根据HotelId获得RoomPO
- RoomService.insertRoomInfo
  - 语法 : `public void insertRoomInfo(HotelRoom hotelRoom)`
  - 前置条件 : 获得Room数据库服务的引用
  - 后置条件 : 向Room数据库中插入RoomPO
- RoomService.deleteRoom
  - 语法 : `void deleteRoom(Integer hotelId, String roomType);`
  - 前置条件 : 获得Room数据库服务的引用
  - 后置条件 : 在Room数据库中删除RoomPO
- RoomService.updateRoomInfo
  - 语法 : `public void updateRoomInfo(Integer hotelId, String roomType, Integer rooms)`
  - 前置条件 : 获得Room数据库服务的引用
  - 后置条件 : 根据roomType和roomId更新room的数量
- RoomService.getRoomCurNum
  - 语法 : `public int getRoomCurNum(Integer hotelId, String roomType)`
  - 前置条件 : 获得Room数据库服务的引用
  - 后置条件 : 根据hotelId和roomType获得Room的数量
- HotelSearchService.searchHotel
  - 语法 : `List<HotelVO> searchHotel(SearchBodyVO searchBody);`
  - 前置条件 : 用户已登录
  - 后置条件 : 根据searchBody获得匹配的酒店列表

需要的服务（需接口）

|            服务名            |             服务              |
| :--------------------------: | :---------------------------: |
| `AccountService.getUserInfo` | 根据ManagerId返回UserPO的数据 |

- 业务逻辑层的动态模型

    这里并不包含上述全部接口，挑了几个主要的。

    - createHotel顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/createHotel.png)

    - updateHotelInfo顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/updateHotelInfo.png)

    - deleteHotel顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/deleteHotel.png)

    - addRoomInfo顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/addRoomInfo.png)

    - retrieveHotelDetail顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/retrieveHotelDetail.png)

    - Hotel状态图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/Hotel.png)

- 业务逻辑层的设计原理

    利用委托式控制风格，每个界面需要访问的业务逻辑由各自的BLServiceImpl委托给不同的领域对象。

#### 4.1.4 orderbl模块

##### 4.1.4.1 概述

Orderbl模块承担的需求参见需求规格说明文档功能需求和相关非功能需求。

Orderbl模块的接口参见软件体系结构描述文档中的接口表。

##### 4.1.4.2 整体结构

Orderbl主要功能是管理酒店订单和用户评价业务

OrderVO是前端传回的用户信息对象，由OrderController调用业务逻辑层方法，将OrderVO转化为Order，Order对象被设计为持久化对象，存储在数据库中。

- 设计图

![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/orderbl.png)

- 类的职责

|       模块       |            职责            |
| :--------------: | :------------------------: |
| OrderServiceImpl | 负责实现订单管理的业务逻辑 |
|      Order       |     订单的领域模型对象     |
|     Comment      |   订单评价的领域模型对象   |

- 接口规范

提供的服务（供接口）

- OrderService.addOrder
  - 语法 : `public ResponseVO addOrder(OrderVO orderVO)`
  - 前置条件 : 获得Hotel, User和Order数据库的服务的引用
  - 后置条件 : 向Order数据库中添加OrderPO对象
- OrderService.getAllOrders
  - 语法 : `public List<Order> getAllOrders()`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 返回所有的订单
- OrderService.getUserOrders
  - 语法 : `public List<Order> getUserOrders(int userid)`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 根据UserId查找对应的Order信息
- OrderService.getHotelOrders
  - 语法 : `List<Order> getHotelOrders(Integer hotelId);`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 根据HotelId查找对应的Order信息
- OrderService.checkIn
  - 语法 : `ResponseVO checkIn(int orderId);`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 订单状态修改为已入住
- OrderService.probableAbnormalOrder
  - 语法 : `List<Order> probableAbnormalOrder(Integer hotelId);`
  - 前置条件 : 获得Order数据库的服务的引用，认证为管理员
  - 后置条件 : 获得酒店可能的异常订单
- OrderService.abnormalOrder
  - 语法 : `ResponseVO abnormalOrder(int orderId, double minCreditRatio);`
  - 前置条件 : 获得Order数据库的服务的引用，认证为管理员
  - 后置条件 : 标记为异常订单，扣除一定比例信誉
- OrderService.finishOrder
  - 语法 : `ResponseVO finishOrder(int orderId);`
  - 前置条件 : 获得Order数据库的服务的引用，认证为管理员
  - 后置条件 : 完成订单，添加信用
- OrderService.getComment
  - 语法 : `CommentVO getComment(int orderId);`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 获取订单的评价
- OrderService.annulComment
  - 语法 : `ResponseVO annulComment(int orderId);`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 删除该评价
- OrderService.getHotelComment
  - 语法 : `List<CommentVO> getHotelComment(int hotelId);`
  - 前置条件 : 获得Order数据库的服务的引用,认证为管理员
  - 后置条件 : 获得对应酒店的所有评价
- OrderService.addComment
  - 语法 : `ResponseVO addComment(CommentVO commentVO);`
  - 前置条件 : 获得Order数据库的服务的引用，订单已入住或完成
  - 后置条件 : 为订单添加评价
- OrderService.getUserComment
  - 语法 : `ResponseVO getUserComments(Integer userId);`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 获取用户的所有评价
- OrderService.getOrdersInMonth
  - 语法 : `List<List<Order>> getOrdersInMonth(List<Order> orders)`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 从订单输入流中返回近30天的订单
- OrderService.getOrdersInMonthOfHotel
  - 语法 : `List<List<Order>> getOrdersInMonthOfHotel(Integer hotelId);`
  - 前置条件 :获得Order数据库的服务的引用
  - 后置条件 : 获取对应酒店近30天的订单
- OrderService.getOrdersInMonthOfAll
  - 语法 : `List<List<Order>> getOrdersInMonthOfAll();`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 获取所有近30天的Order
- OrderService.filterOrders
  - 语法 : `List<Order> filterOrders(List<Order> orders,String beginTime, String endTime);`
  - 前置条件 : 获得Order数据库的服务的引用
  - 后置条件 : 找到与入住及退房时间有关联的订单
- OrderService.annulOrder
  - 语法 : `public ResponseVO annulOrder(int orderid)`
  - 前置条件 : 获得Hotel，User，Order数据库的服务的引用
  - 后置条件 : 从Order数据库中删除OrderPO对象，更新User的信誉积分和酒店房间信息
- OrderService.getOrderableRoom
  - 语法 : `HotelVO getOrderableRoom(Integer hotelId, String beginTime, String endTime)`
  - 前置条件 : 获得Hotel, Order数据库服务的引用
  - 对特定的酒店，返回可预订的房间的信息
- OrderService.checkRoomByOrder
  - 语法 : `List<RoomVO> checkRoomByOrder(List<RoomVO> rooms, List<Order> orders)`
  - 前置条件 : 获得Hotel, Order数据库服务的引用
  - 后置条件 : 通过房间信息和对应的订单信息，检查酒店房间情况
- OrderService.getRoomCurNumByOrder
  - 语法 : `Integer getRoomCurNumByOrder(Integer hotelId, String beginTime, String endTime, String type)`
  - 前置条件 : 获得Hotel, Order数据库服务的引用
  - 后置条件 : 通过订单查找酒店特定房间的可用房间数，用于addOrder时的检验
- OrderService.argueAbnormalOrder
  - 语法 : `ResponseVO argueAbnormalOrder(Integer orderId, String reason)`
  - 前置条件 : 获得Order数据库服务的引用，已被标记为异常订单
  - 后置条件 : 将订单的申诉内容记录在comment里

需要的服务（需接口）

|                服务名                |              服务              |
| :----------------------------------: | :----------------------------: |
|     `HotelService.getRoomCurNum`     |   获取酒店房间已被预订的时间   |
| `HotelService.retrieveHotelDetailis` |       获取酒店的详细信息       |
|     `AccountService.getUserInfo`     |   根据订单用户id获得用户信息   |
|    `HotelService.updateRoomInfo`     |          更新房间信息          |
|  `RoomService.getRoomCurNumByTime`   | 根据时间获取对应房间的可用数量 |
|     `AccountService.getUserInfo`     |          获取用户信息          |
|    `HotelService.updateRoomInfo`     |          更新房间信息          |
|     `HotelService.annulComment`      |        修改Hotel的评分         |
|      `HotelService.addComment`       |        修改Hotel的评分         |

- 业务逻辑层的动态模型

    同样，这里并不包含上述全部接口，挑了几个主要的。

    - addOrder顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/reverseHotel.png)

    - annulOrder顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/annulOrder.png)

    - getUserOrders顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/retrieveUserOrders.png)

    - addComment顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/addComment.png)

    - getComment顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getComment.png)

    - getOrderableRoom顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getOrderableRoom.png)

    - Order状态图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/order.png)

- 业务逻辑层的设计原理

    利用委托式控制风格，每个界面需要访问的业务逻辑由各自的BLServiceImpl委托给不同的领域对象。

#### 4.1.5 questionbl模块

##### 4.1.5.1 概述

Questionbl模块承担的需求参见需求规格说明文档功能需求和相关非功能需求。

Questionbl模块的接口参见软件体系结构描述文档中的接口表。

##### 4.1.5.2 整体结构

Questionbl主要功能是管理用户提问
QuestionVO， AnswerVO是前端传回的用户信息对象，由HotelController调用业务逻辑层方法，将QuestionVO, AnswerVO转化为Question, Answer对象被设计为持久化对象，存储在数据库中。

- 设计图

![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/questionbl.png)

- 类的职责

|        模块         |           职责           |
| :-----------------: | :----------------------: |
|  AnswerServiceImpl  |  问题回答的业务逻辑实现  |
| QuestionServiceImpl | 对酒店提问的业务逻辑实现 |
|       Answer        |    回答的领域模型对象    |
|      Question       |    提问的领域模型对象    |

- 接口规范

- AnswerService.addAnswer
  - 语法 : `ResponseVO addAnswer(AnswerVO answerVO);`
  - 前置条件 :获得Answer数据库的引用
  - 后置条件 :添加Answer
- AnswerService.getAllAnswers
  - 语法 : `List<AnswerVO> getAllAnswers();`
  - 前置条件 :获得Answer数据库的引用
  - 后置条件 :获取所有的Answer
- AnswerService.getQuestionAnswers
  - 语法 : `List<AnswerVO> getQuestionAnswers(Integer questionId);`
  - 前置条件 :获得Answer数据库的引用
  - 后置条件 :获得对应问题的所有回答
- AnswerService.annulAnswer
  - 语法 : `ResponseVO annulAnswer(Integer answerId);`
  - 前置条件 : 获得Answer数据库的引用
  - 后置条件 : 删除回答
- QuestionService.addQuestion
  - 语法 : `ResponseVO addQuestion(QuestionForm questionForm);`
  - 前置条件 : 获得Question数据库服务的引用
  - 后置条件 : 添加问题
- QuestionService.annulQuestion
  - 语法 : `ResponseVO annulQuestion(Integer questionId);`
  - 前置条件 :获得Question数据库服务的引用
  - 后置条件 : 将问题标记为不可达
- QuestionService.getHotelQuestion
  - 语法 : `List<QuestionVO> getHotelQuestion(Integer hotelId);`
  - 前置条件 : 获得Question和Answer数据库服务的引用
  - 后置条件 : 获取酒店的所有问题和对应答案
- QuestionService.getUserQuestion
  - 语法 : `List<QuestionVO> getUserQuestion(Integer userId);`
  - 前置条件 : 获得Question和Answer数据库服务的引用
  - 后置条件 : 获取用户的所有问题和对应答案

需要的服务（需接口） -- 无

- 业务逻辑层的动态模型

    - addQuestion顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/addQuestion.png)

    - addAnswer顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/addAnswer.png)

    - annulQuestion顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/annulQuestion.png)

    - annulAnswer顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/annulAnswer.png)

    - getAnswersOfQuestion顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getQuestionAnswer.png)

    - getHotelQuestion顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getHotelQuestion.png)

    - Question状态图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/Question状态图.png)

    - Answer状态图

    在这里，Answer对象从属与Question对象，其状态也和Question类似，因此省略其状态图

- 业务逻辑层的设计原理

    利用委托式控制风格，每个界面需要访问的业务逻辑由各自的BLServiceImpl委托给不同的领域对象。

#### 4.1.6 userbl模块

##### 4.1.6.1 概述

Userbl模块承担的需求参见需求规格说明文档功能需求和相关非功能需求。
Userbl模块的接口参见软件体系结构描述文档中的接口表。

##### 4.1.6.2 整体结构

Userbl主要功能是管理用户、收藏和信誉记录。

UserForm是前端传回的用户信息对象，由UserController调用业务逻辑层方法，将UserForm转化为User，User对象被设计为持久化对象，存储在数据库中。

- 设计图

![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/userbl.png)

- 类的职责

|         模块          |                       职责                       |
| :-------------------: | :----------------------------------------------: |
|  AccountServiceImpl   | 酒店管理员，网站营销人员，工作人员的账号管理业务 |
| CollectionServiceImpl |               酒店收藏业务逻辑实现               |
|        Credit         |             信誉变更记录领域模型对象             |
|         User          |      系统管理员、工作人员、用户领域模型对象      |

- 接口规范

- AccountService.registerAccount
  - 语法 : `public ResponseVO registerAccount(UserVO userVO)`
  - 前置条件 : 得到User数据库的服务的引用，password和email符合规范
  - 后置条件 : 向User数据库中插入UserPO对象
- AccountService.login
  - 语法 : `public User login(UserForm userForm)`
  - 前置条件 : 得到User数据库服务的引用
  - 后置条件 : 查找是否存在相应的User，根据输入的password返回登陆验证的结果
- AccountService.getUserInfo
  - 语法 : `public User getUserInfo(int id)`
  - 前置条件 : 得到User数据库服务的引用
  - 后置条件 : 根据id查找User数据库，返回匹配的对象
- AccountService.updateUserInfo
  - 语法 : `ResponseVO updateUserInfo(int id, String username, String phoneNumber, String corporation);`
  - 前置条件 : 得到User数据库服务的引用
  - 后置条件 : 根据id查找User数据库，更新用户名、手机号、公司名
- AccountService.getUserInfoByEmail
  - 语法 : `UserVO getUserInfoByEmail(String email)`
  - 前置条件:得到User数据库服务的引用
  - 后置条件:根据email查找User数据库
- AccountService.updatePassword
  - 语法 : `ResponseVO updatePassword(int id, String password);`
  - 前置条件:得到User数据库服务的引用
  - 后置条件:更新加密后的密码
- AccountService.updateCredit
  - 语法 : `ResponseVO updateCredit(int id, double credit);`
  - 前置条件: 得到Credit数据库服务的引用
  - 后置条件: 更新用户信誉
- AccountService.argueCredit
  - 语法 : `ResponseVO argueCredit(int creditId, String argue);`
  - 前置条件: 得到Credit数据库服务的引用，Credit没有申诉过
  - 后置条件: 向网站管理员提出信誉变更申诉
- AccountService.getArgueCredits
  - 语法 : `List<CreditVO> getArgueCredits();`
  - 前置条件: 得到Credit数据库服务的引用
  - 后置条件: 获取所有申诉的Credit
- AccountService.handleArgue
  - 语法 : `ResponseVO handleArgue(Integer creditId, boolean accept);`
  - 前置条件: 得到Credit数据库服务的引用
  - 后置条件: 将Argue标记为已接受或拒绝接受
- AccountService.corporateVIP
  - 语法 : `ResponseVO corporateVIP(int id, String corporate);`
  - 前置条件: 得到VIP数据库服务的引用
  - 后置条件: 将企业标记为CorporateVIP
- AccountService.updateBirthday
  - 语法 : `void updateBirthday(int id, String birthday)`
  - 前置条件: 得到User数据库服务的引用
  - 后置条件: 更新用户生日
- AccountService.registerAsVIP
  - 语法 : `void registerAsVIP(int id);`
  - 前置条件: 得到VIP数据库服务的引用
  - 后置条件: 将用户注册为VIP
- AccountService.freezeVIP
  - 语法 : `void freezeVIP(int id);`
  - 前置条件: 得到VIP数据库服务的引用
  - 后置条件: 停止用户的VIP服务
- AccountService.updatePortrait
  - 语法 : `ResponseVO updatePortrait(int userId, String url);`
  - 前置条件:得到User数据库服务的引用
  - 后置条件: 更新头像
- AccountService.chargeCredit
  - 语法 : `ResponseVO chargeCredit(int userId, int change, String reason);`
  - 前置条件: 得到Credit数据库服务的引用
  - 后置条件: 添加用户的信誉变更记录
- AccountService.getUserCreditChanges
  - 语法 : `ResponseVO getUserCreditChanges(int userId);`
  - 前置条件: 得到Credit数据库服务的引用
  - 后置条件: 获取用户的信誉变更记录
- AccountService.getAllUsers
  - 语法 : `List<UserVO> getAllUsers();`
  - 前置条件:得到User数据库服务的引用
  - 后置条件: 获取所有UserPO
- AccountService.getAllPhoneNumOfSalesPerson
  - 语法 : `List<String> getAllPhoneNumOfSalesPerson();`
  - 前置条件:得到User数据库服务的引用
  - 后置条件:获取所有网站营销人员的电话号
- CollectionService.addCollection
  - 语法 :`ResponseVO addCollection(CollectionVO collectionVO);`
  - 前置条件: 获取Collection数据库服务的引用
  - 后置条件:添加Collection
- CollectionService.getUserCollection
  - 语法 :`List<CollectionVO> getUserCollection(Integer userId);`
  - 前置条件:获取Collection数据库服务的引用
  - 后置条件:获取用户的所有收藏
- CollectionService.getHotelCollection
  - 语法 :`Integer getHotelCollection(Integer hotelId);`
  - 前置条件:获取Collection数据库服务的引用
  - 后置条件:获取酒店的所有收藏数量
- CollectionService.userCollection
  - 语法 :`Boolean userCollection(Integer userId, Integer hotelId);`
  - 前置条件:获取Collection数据库服务的引用
  - 后置条件:判断用户是否收藏了该酒店
- CollectionService.annulCollectionByUserId
  - 语法 :`ResponseVO annulCollectionByUserId(Integer hotelId, Integer userId);`
  - 前置条件:获取Collection数据库服务的引用，用户已收藏该酒店
  - 后置条件:删除用户对这家酒店的收藏

需要的服务（需接口）-- 无

- 业务逻辑层的动态模型

    - login顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/login.png)

    - registerAccount顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/registerAccount.png)

    - getUserInfo顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getUserInfo.png)

    - updateUserInfo顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/updateUserInfo.png)

    - chargeCredit顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/chargeCredit.png)

    - addCollection顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/addCollection.png)

    - annulCollection顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/annulCollection.png)

    - getUserCollection顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getUserCollection.png)

    - getUserCreditChange顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getUserCreditChange.png)

    - User对象状态图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/user状态图.png)

- 业务逻辑层的设计原理

    利用委托式控制风格，每个界面需要访问的业务逻辑由各自的BLServiceImpl委托给不同的领域对象。

#### 4.1.7 VIPbl模块

##### 4.1.7.1 概述

VIPbl模块承担的需求参见需求规格说明文档功能需求和相关非功能需求。

VIPbl模块的接口参见软件体系结构描述文档中的接口表。

##### 4.1.7.2 整体结构

VIPbl主要功能是管理酒店工作人员和网站营销人员账户。

前端传回的用户注册VIP信息，由UserController调用业务逻辑层方法，将VIP注册信息转化为ClientVIP或CorpVIP，VIP对象被设计为持久化对象，存储在数据库中。

- 设计图

- 类的职责

|       模块       |           职责            |
| :--------------: | :-----------------------: |
| LevelServiceImpl | 管理VIP等级和对应优惠方式 |
|  VIPServiceImpl  |   VIP用户管理和业务逻辑   |
|     CorpVIP      | 企业VIP用户的领域模型对象 |
|    ClientVIP     |    个人VIP领域模型对象    |

- 接口规范

提供的服务（供接口）

- LevelService.formulateVIPLevel
  - 语法：`ResponseVO formulateVIPLevel(Integer level, String type, Integer requestConsumption, double reduction);`
  - 前置条件：
  - 后置条件：
- LevelService.getTheRequestOfLevel
  - 语法：`ResponseVO getTheRequestOfLevel(Integer level, String type);`
  - 前置条件:获得VIPLevel数据库服务的引用
  - 后置条件：获取对应VIP等级需要的消费额
- VIPService.registerAsClientVIP
  - 语法：`ResponseVO registerAsClientVIP(Integer userId, String Birthday);`
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：注册为普通会员
- VIPService.freezeClientVIP
  - 语法：`ResponseVO freezeClientVIP(Integer userId);`
  - 前置条件：获得VIP数据库服务的引用，被认证为网站管理员
  - 后置条件：停用该普通会员
- VIPService.restoreClientVIP
  - 语法：`ResponseVO restoreClientVIP(Integer userId);`
  - 前置条件：获得VIP数据库服务的引用，被认证为网站管理员，会员被冻结
  - 后置条件：恢复普通会员的特权
- VIPService.getVIPByUserId
  - 语法：`ResponseVO getVIPbyUserId(Integer userId);`
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：根据ID获取会员情况
- VIPService.addVIPClientConsumption
  - 语法：`ResponseVO addVIPClientConsumption(Integer userId, Integer amount);`
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：添加个人会员的消费额度
- VIPService.registerAsCorpVIP
  - 语法：`ResponseVO registerAsCorpVIP(String corporationName);`
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：注册为企业会员
- VIPService.freezeCorpVIP
  - 语法：`ResponseVO freezeCorpVIP(String corporationName);`
  - 前置条件：获得VIP数据库服务的引用，被认证为网站管理员
  - 后置条件：停用企业会员服务
- VIPService.restoreCorpVIP
  - 语法：`ResponseVO restoreCorpVIP(String corporationName);`
  - 前置条件：获得VIP数据库服务的引用，被认证为网站管理员，会员被冻结
  - 后置条件：恢复企业会员服务
- VIPService.getVIPByCorpName
  - 语法：`ResponseVO getVIPbyCorpName(String corporationName);`
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：根据企业名获取VIP类型
- VIPService.addVIPCorpConsumption
  - 语法：`ResponseVO addVIPCorpConsumption(String corporationName, Integer amount);`
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：添加企业VIP消费
- VIPService.VIPCorpCheck
  - 语法：`boolean VIPCorpCheck(String corporationName);`
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：检查企业是否是VIP
- VIPService.getAllVIPClient
  - 语法：ResponseVO getAllVIPClient();``
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：获取所有个人VIP
- VIPService.getAllVIPCorp
  - 语法：`ResponseVO getAllVIPCorp();`
  - 前置条件：获得VIP数据库服务的引用
  - 后置条件：获取所有企业VIP

需要的服务（需接口）

|             服务名              |     服务      |
| :-----------------------------: | :-----------: |
| `AccountService.registerAsVIP`  | 注册成VIP客户 |
| `AccountService.updateBirthday` | 更新生日信息  |
|   `AccountService.freezeVIP`    |    停用VIP    |

- 业务逻辑层的动态模型

    企业会员相关方法的顺序图与普通会员没有太大不同，所以这里只给出普通会员的相关图例，省略企业会员。

    - registerClientVIP顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/registerClientVIP.png)

    - freezeClientVIP顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/freezeClientVIP.png)

    - restoreClientVIP顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/restoreClientVIP.png)

    - getUserVIP顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getUserVIP.png)

    - addVIPConsumption顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/addVIPConsumption.png)

    - formulateALevel顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/formulateALevel.png)

    - getTheRequestOfLevel顺序图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/getTheRequestOfLevel.png)

    - VIP状态图

    ![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/VIP.png)

- 业务逻辑层的设计原理

    利用委托式控制风格，每个界面需要访问的业务逻辑由各自的BLServiceImpl委托给不同的领域对象。

## 5. 依赖视角

我们的产品是web app，因此开发重点是服务端，服务端各自包的依赖关系如下

![](https://lemonzzy.oss-cn-hangzhou.aliyuncs.com/img/服务端开发包图.png)
