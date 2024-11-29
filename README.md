# campus-take-out

Campus take-out is a software product specially customized for campus restaurants, aimed at improving the operational efficiency of campus catering and the ordering experience of customers. This project aims to build a food delivery platform that integrates management backend and user side mini programs, enabling campus catering dish management, order processing, data analysis, and other functions. At the same time, it provides convenient ordering, payment, and delivery tracking services for consumers with specific identities such as students and faculty on campus. It is a project built on the basis of [itheima's](http://www.itheima.com/) the sky take-out.

Campus take-out adopts a front-end and back-end separation development model. The front-end uses Vue.js framework to build the user interface, and the back-end uses Spring Boot framework to build the service layer. MySQL is used for data storage in the database. The overall system follows the RESTful API design principles to ensure efficient and stable front-end and back-end interaction.

In terms of technology selection, Campus take-out uses multiple technology stacks, including front-end Vue.js+ElementUI+WeChat mini program, back-end Spring Boot+SpringMVC+MyBatis+MySQL, as well as other technologies such as Redis (caching), Nginx (reverse proxy and load balancing), JWT (security authentication), etc. These technologies ensure high performance and scalability of the system, improve system response speed, and implement user authentication and authorization through JWT, ensuring system data security.



# Project Structure

The project structure is shown in the following figure:

```asciiarmor
           |  ┌───────────────┐    ┌───────────────┐
Front end  |  │ Management end│    │   User end    │
           |  │     (Web)     │    │ (mini Program)│
           |  └───────────────┘    └───────────────┘
___________|           │                   │
           |           ▼                   ▼
           |      ┌─────────────────────────────┐
Back end   |      │   Backend services (Java)   │
           |      └─────────────────────────────┘
___________|                     │
           |                     ▼
           |      ┌─────────────────────────────┐
Database   |      │   Databases (Mysql/SQlite)  │
           |      └─────────────────────────────┘
```

Use nginx proxy on the management end in the "nginx-1.20.2" directory.Click on "nginx. exe" to start.



Campus_take_out.sql is a database script file.



# Database Design Document

| ID   | Table name    | Chinese name   |
| ---- | ------------- | -------------- |
| 1    | employee      | 员工表         |
| 2    | category      | 分类表         |
| 3    | dish          | 菜品表         |
| 4    | dish_flavor   | 菜品口味表     |
| 5    | setmeal       | 套餐表         |
| 6    | setmeal_dish  | 套餐菜品关系表 |
| 7    | user          | 用户表         |
| 8    | address_book  | 地址表         |
| 9    | shopping_cart | 购物车表       |
| 10   | orders        | 订单表         |
| 11   | order_detail  | 订单明细表     |

## 1. employee

The employee table is an employee table used to store employee information within the merchant. The specific table structure is as follows:

| Field name  | Data type   | Statement    | Note        |
| ----------- | ----------- | ------------ | ----------- |
| id          | bigint      | 主键         | 自增        |
| name        | varchar(32) | 姓名         |             |
| username    | varchar(32) | 用户名       | 唯一        |
| password    | varchar(64) | 密码         |             |
| phone       | varchar(11) | 手机号       |             |
| sex         | varchar(2)  | 性别         |             |
| id_number   | varchar(18) | 身份证号     |             |
| status      | int         | 账号状态     | 1正常 0锁定 |
| create_time | datetime    | 创建时间     |             |
| update_time | datetime    | 最后修改时间 |             |
| create_user | bigint      | 创建人id     |             |
| update_user | bigint      | 最后修改人id |             |

## 2. category

The category table is a classification table used to store the classification information of products. The specific table structure is as follows:

| Field name  | Data type   | Statement    | Note                 |
| ----------- | ----------- | ------------ | -------------------- |
| id          | bigint      | 主键         | 自增                 |
| name        | varchar(32) | 分类名称     | 唯一                 |
| type        | int         | 分类类型     | 1菜品分类  2套餐分类 |
| sort        | int         | 排序字段     | 用于分类数据的排序   |
| status      | int         | 状态         | 1启用 0禁用          |
| create_time | datetime    | 创建时间     |                      |
| update_time | datetime    | 最后修改时间 |                      |
| create_user | bigint      | 创建人id     |                      |
| update_user | bigint      | 最后修改人id |                      |

## 3. dish

Dish table is a menu table used to store information about dishes. The specific table structure is as follows:

| Field name  | Data type     | Statement    | Note        |
| ----------- | ------------- | ------------ | ----------- |
| id          | bigint        | 主键         | 自增        |
| name        | varchar(32)   | 菜品名称     | 唯一        |
| category_id | bigint        | 分类id       | 逻辑外键    |
| price       | decimal(10,2) | 菜品价格     |             |
| image       | varchar(255)  | 图片路径     |             |
| description | varchar(255)  | 菜品描述     |             |
| status      | int           | 售卖状态     | 1起售 0停售 |
| create_time | datetime      | 创建时间     |             |
| update_time | datetime      | 最后修改时间 |             |
| create_user | bigint        | 创建人id     |             |
| update_user | bigint        | 最后修改人id |             |

## 4. dish_flavor

The dish_favor table is a dish flavor table used to store taste information of dishes. The specific table structure is as follows:

| Field name | Data type    | Statement | Note     |
| ---------- | ------------ | --------- | -------- |
| id         | bigint       | 主键      | 自增     |
| dish_id    | bigint       | 菜品id    | 逻辑外键 |
| name       | varchar(32)  | 口味名称  |          |
| value      | varchar(255) | 口味值    |          |

## 5. setmeal

The setmeal table is a package table used to store information about packages. The specific table structure is as follows:

| Field name  | Data type     | Statement    | Note        |
| ----------- | ------------- | ------------ | ----------- |
| id          | bigint        | 主键         | 自增        |
| name        | varchar(32)   | 套餐名称     | 唯一        |
| category_id | bigint        | 分类id       | 逻辑外键    |
| price       | decimal(10,2) | 套餐价格     |             |
| image       | varchar(255)  | 图片路径     |             |
| description | varchar(255)  | 套餐描述     |             |
| status      | int           | 售卖状态     | 1起售 0停售 |
| create_time | datetime      | 创建时间     |             |
| update_time | datetime      | 最后修改时间 |             |
| create_user | bigint        | 创建人id     |             |
| update_user | bigint        | 最后修改人id |             |

## 6. setmeal_dish

The setmean-dish table is a set meal dish relationship table used to store the association between sets and dishes. The specific table structure is as follows:

| Field name | Data type     | Statement | Note     |
| ---------- | ------------- | --------- | -------- |
| id         | bigint        | 主键      | 自增     |
| setmeal_id | bigint        | 套餐id    | 逻辑外键 |
| dish_id    | bigint        | 菜品id    | 逻辑外键 |
| name       | varchar(32)   | 菜品名称  | 冗余字段 |
| price      | decimal(10,2) | 菜品单价  | 冗余字段 |
| copies     | int           | 菜品份数  |          |

## 7. user

The user table is a user table used to store information about C-end users. The specific table structure is as follows:

| Field name  | Data type    | Statement          | Note |
| ----------- | ------------ | ------------------ | ---- |
| id          | bigint       | 主键               | 自增 |
| openid      | varchar(45)  | 微信用户的唯一标识 |      |
| name        | varchar(32)  | 用户姓名           |      |
| phone       | varchar(11)  | 手机号             |      |
| sex         | varchar(2)   | 性别               |      |
| id_number   | varchar(18)  | 身份证号           |      |
| avatar      | varchar(500) | 微信用户头像路径   |      |
| create_time | datetime     | 注册时间           |      |

## 8. address_book

The address_book table is an address table used to store the shipping address information of C-end users. The specific table structure is as follows:

| Field name     | Data type    | Statement    | Note                 |
| -------------- | ------------ | ------------ | -------------------- |
| id             | bigint       | 主键         | 自增                 |
| user_id        | bigint       | 用户id       | 逻辑外键             |
| consignee      | varchar(50)  | 收货人       |                      |
| sex            | varchar(2)   | 性别         |                      |
| phone          | varchar(11)  | 手机号       |                      |
| campus_code    | varchar(12)  | 校区编码     |                      |
| campus_name    | varchar(32)  | 校区名称     |                      |
| dormitory_code | varchar(12)  | 宿舍楼编码   |                      |
| dormitory_name | varchar(32)  | 宿舍楼名称   |                      |
| dorm_code      | varchar(12)  | 宿舍编码     |                      |
| dorm_name      | varchar(32)  | 宿舍名称     |                      |
| detail         | varchar(200) | 详细地址信息 | 具体到门牌号         |
| label          | varchar(100) | 标签         | 学生宿舍、教职工宿舍 |
| is_default     | tinyint(1)   | 是否默认地址 | 1是 0否              |

## 9. shopping_cart

The shopping_cart table is a shopping cart table used to store the shopping cart information of C-end users. The specific table structure is as follows:

| Field name  | Data type     | Statement    | Note     |
| ----------- | ------------- | ------------ | -------- |
| id          | bigint        | 主键         | 自增     |
| name        | varchar(32)   | 商品名称     |          |
| image       | varchar(255)  | 商品图片路径 |          |
| user_id     | bigint        | 用户id       | 逻辑外键 |
| dish_id     | bigint        | 菜品id       | 逻辑外键 |
| setmeal_id  | bigint        | 套餐id       | 逻辑外键 |
| dish_flavor | varchar(50)   | 菜品口味     |          |
| number      | int           | 商品数量     |          |
| amount      | decimal(10,2) | 商品单价     |          |
| create_time | datetime      | 创建时间     |          |

## 10. orders

The orders table is an order table used to store order data for C-end users. The specific table structure is as follows:

| Field name              | Data type     | Statement    | Note                                            |
| ----------------------- | ------------- | ------------ | ----------------------------------------------- |
| id                      | bigint        | 主键         | 自增                                            |
| number                  | varchar(50)   | 订单号       |                                                 |
| status                  | int           | 订单状态     | 1待付款 2待接单 3已接单 4派送中 5已完成 6已取消 |
| user_id                 | bigint        | 用户id       | 逻辑外键                                        |
| address_book_id         | bigint        | 地址id       | 逻辑外键                                        |
| order_time              | datetime      | 下单时间     |                                                 |
| checkout_time           | datetime      | 付款时间     |                                                 |
| pay_method              | int           | 支付方式     | 1微信支付 2支付宝支付                           |
| pay_status              | tinyint       | 支付状态     | 0未支付 1已支付 2退款                           |
| amount                  | decimal(10,2) | 订单金额     |                                                 |
| remark                  | varchar(100)  | 备注信息     |                                                 |
| phone                   | varchar(11)   | 手机号       |                                                 |
| address                 | varchar(255)  | 详细地址信息 |                                                 |
| user_name               | varchar(32)   | 用户姓名     |                                                 |
| consignee               | varchar(32)   | 收货人       |                                                 |
| cancel_reason           | varchar(255)  | 订单取消原因 |                                                 |
| rejection_reason        | varchar(255)  | 拒单原因     |                                                 |
| cancel_time             | datetime      | 订单取消时间 |                                                 |
| estimated_delivery_time | datetime      | 预计送达时间 |                                                 |
| delivery_status         | tinyint       | 配送状态     | 1立即送出  0选择具体时间                        |
| delivery_time           | datetime      | 送达时间     |                                                 |
| pack_amount             | int           | 打包费       |                                                 |
| tableware_number        | int           | 餐具数量     |                                                 |
| tableware_status        | tinyint       | 餐具数量状态 | 1按餐量提供  0选择具体数量                      |

## 11. order_detail

The order_detail table is an order detail table used to store order detail data for C-end users. The specific table structure is as follows:

| Field name  | Data type     | Statement    | Note     |
| ----------- | ------------- | ------------ | -------- |
| id          | bigint        | 主键         | 自增     |
| name        | varchar(32)   | 商品名称     |          |
| image       | varchar(255)  | 商品图片路径 |          |
| order_id    | bigint        | 订单id       | 逻辑外键 |
| dish_id     | bigint        | 菜品id       | 逻辑外键 |
| setmeal_id  | bigint        | 套餐id       | 逻辑外键 |
| dish_flavor | varchar(50)   | 菜品口味     |          |
| number      | int           | 商品数量     |          |
| amount      | decimal(10,2) | 商品单价     |          |

