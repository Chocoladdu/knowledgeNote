---
{"dg-publish":true,"permalink":"/homeview//emall/"}
---


# emall

---
## 数据库表设计

### 通用字段
```mysql
id
isDelete
createTime
updateTime
```


### 用户表
#### 存放用户信息，如**、**
``` mysql
CREATE TABLE `user`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键id 用户id',
  `userAccount` varchar(128) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '用户账号',
  `userPassword` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '用户密码',
  `username` varchar(128) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '用户昵称',
  `avatarUrl` varchar(1024) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '用户头像',
  `gender` tinyint NULL DEFAULT NULL COMMENT '性别 1-男 0-女',
  `phone` varchar(32) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '电话',
  `email` varchar(64) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '邮箱',
  `birthday` date NULL DEFAULT NULL COMMENT '生日',
  `userStatus` int NOT NULL DEFAULT 0 COMMENT '状态 0-正常 ',
  `isDelete` tinyint NOT NULL DEFAULT 0 COMMENT '是否删除 0-正常 1-删除',
  `userRole` tinyint NULL DEFAULT 0 COMMENT '用户角色 0-普通 1-管理员',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '用户表' ROW_FORMAT = Dynamic;
```
### 分类表
#### 存放分类信息，如女装，平板电视，沙发等
``` mysql
DROP TABLE IF EXISTS `category`;
CREATE TABLE `category`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键 分类id主键',
  `name` varchar(32) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '分类名称',
  `parentId` int NOT NULL COMMENT '父级id 一级分类为0 其余依赖上一级',
  `level` int NOT NULL COMMENT '分类层级 1-一级 2-二级 3-三级',
  `isDelete` tinyint NOT NULL DEFAULT 0 COMMENT '是否删除 0-正常 1-删除',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '分类表' ROW_FORMAT = Dynamic;
```
### 属性表
#### 存放属性信息，如颜色，重量，品牌，厂商，型号等
``` mysql
CREATE TABLE `property`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键 属性id主键',
  `cid` bigint NULL DEFAULT NULL COMMENT '分类id',
  `name` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '属性名',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `fk_property_category`(`cid` ASC) USING BTREE,
  CONSTRAINT `fk_property_category` FOREIGN KEY (`cid`) REFERENCES `category` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB AUTO_INCREMENT = 1 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '属性表' ROW_FORMAT = Dynamic;
```
### 产品表
#### 存放产品信息，如LED40EC平板电视机，海尔EC6005热水器
``` mysql
CREATE TABLE `product`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键 产品id主键',
  `cid` bigint NULL DEFAULT NULL COMMENT '分类id',
  `name` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '产品名',
  `subTitle` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '小标题',
  `originalPrice` decimal(10, 0) NULL DEFAULT NULL COMMENT '原始价格',
  `promotePrice` decimal(10, 0) NULL DEFAULT NULL COMMENT '优惠价格',
  `stock` int NULL DEFAULT NULL COMMENT '库存',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `fk_product_category`(`cid` ASC) USING BTREE,
  CONSTRAINT `fk_product_category` FOREIGN KEY (`cid`) REFERENCES `category` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '产品表' ROW_FORMAT = Dynamic;
```
### 属性值表
#### 存放属性值信息，如重量是900g,颜色是粉红色
``` mysql
CREATE TABLE `propertyvalue`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键 属性值id主键',
  `pid` bigint NULL DEFAULT NULL COMMENT '属性id',
  `ptid` bigint NULL DEFAULT NULL COMMENT '产品id',
  `value` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '值',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `fk_propertyValue_property`(`ptid` ASC) USING BTREE,
  INDEX `fk_propertyValue_product`(`pid` ASC) USING BTREE,
  CONSTRAINT `fk_propertyValue_product` FOREIGN KEY (`pid`) REFERENCES `product` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT,
  CONSTRAINT `fk_propertyValue_property` FOREIGN KEY (`ptid`) REFERENCES `property` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '属性值表' ROW_FORMAT = Dynamic;

```
### 产品图片表
#### 存放产品图片信息，如产品页显示的5个图片
``` mysql
CREATE TABLE `productimg`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键 产品图片id主键',
  `pid` bigint NULL DEFAULT NULL COMMENT '产品id',
  `type` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '类型 单个图片和详情图片两种',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `fk_productImg_product`(`pid` ASC) USING BTREE,
  CONSTRAINT `fk_productImg_product` FOREIGN KEY (`pid`) REFERENCES `product` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '产品图片表' ROW_FORMAT = Dynamic;

```
### 评论表
#### 存放评论信息，如买回来的蜡烛很好用，么么哒
``` mysql
CREATE TABLE `content`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键 产品id主键',
  `content` text CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT '评论',
  `uid` bigint NULL DEFAULT NULL COMMENT '用户id',
  `pid` bigint NULL DEFAULT NULL COMMENT '产品id',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `fk_content_product`(`pid` ASC) USING BTREE,
  INDEX `fk_content_user`(`uid` ASC) USING BTREE,
  CONSTRAINT `fk_content_product` FOREIGN KEY (`pid`) REFERENCES `product` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT,
  CONSTRAINT `fk_content_user` FOREIGN KEY (`uid`) REFERENCES `user` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '评论表' ROW_FORMAT = Dynamic;
```
### 订单表
#### 存放订单信息，包括邮寄地址，电话号码等信息
``` mysql
CREATE TABLE `orders`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键 订单id主键',
  `orderCode` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '订单号',
  `address` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '地址',
  `post` varchar(16) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '邮编',
  `receiver` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '收货人信息',
  `phone` varchar(32) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '手机号',
  `userMessage` varchar(256) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '用户备注信息',
  `payDate` datetime NULL DEFAULT NULL COMMENT '支付日期',
  `deliveryDate` datetime NULL DEFAULT NULL COMMENT '发货日期',
  `confirmDate` datetime NULL DEFAULT NULL COMMENT '确认收货日期',
  `uid` bigint NULL DEFAULT NULL COMMENT '用户id',
  `oredrStatus` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '订单状态',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `fk_orders_user`(`uid` ASC) USING BTREE,
  CONSTRAINT `fk_orders_user` FOREIGN KEY (`uid`) REFERENCES `user` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '订单表' ROW_FORMAT = Dynamic;
```
### 订单项表
#### 存放订单项信息，包括购买产品种类，数量等
``` mysql
CREATE TABLE `oredeitem`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键 订单项id主键',
  `pid` bigint NULL DEFAULT NULL COMMENT '产品id',
  `oid` bigint NULL DEFAULT NULL COMMENT '订单id',
  `uid` bigint NULL DEFAULT NULL COMMENT '用户id',
  `number` bigint NULL DEFAULT NULL COMMENT '购买数量',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `fk_orderItem_user`(`uid` ASC) USING BTREE,
  INDEX `fk_orderItem_product`(`pid` ASC) USING BTREE,
  INDEX `fk_orderItem_orders`(`oid` ASC) USING BTREE,
  CONSTRAINT `fk_orderItem_orders` FOREIGN KEY (`oid`) REFERENCES `orders` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT,
  CONSTRAINT `fk_orderItem_product` FOREIGN KEY (`pid`) REFERENCES `product` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT,
  CONSTRAINT `fk_orderItem_user` FOREIGN KEY (`uid`) REFERENCES `user` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '订单项表' ROW_FORMAT = Dynamic;
```
### banner表
#### 存放首页轮播图
``` mysql
CREATE TABLE `banner`  (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'banner id',
  `bannerUrl` varchar(1024) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'banner图片地址',
  `goodsId` bigint NULL DEFAULT NULL COMMENT '商品id',
  `categoryId` bigint NULL DEFAULT NULL COMMENT '商品分类id',
  `indexType` int NOT NULL COMMENT '判断轮播图类型 根据商品id或分类id进行跳转 0-商品 1-分类',
  `bannerStatus` tinyint NOT NULL DEFAULT 0 COMMENT '状态 0-正常 1-下架',
  `isDelete` tinyint NOT NULL DEFAULT 0 COMMENT '是否删除 0-正常 1-删除',
  `createTime` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间 创建时间',
  `updateTime` datetime NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 1 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '轮播图表' ROW_FORMAT = Dynamic;
```