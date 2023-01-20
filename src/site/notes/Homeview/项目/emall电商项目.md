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
``` mysql
create table user  
(  
    id           bigint auto_increment comment '主键id 用户id'  ,
    userAccount  varchar(128)                       not null comment '用户账号',  
    userPassword varchar(256)                       not null comment '用户密码',  
    username     varchar(128)                       null comment '用户昵称',  
    avatarUrl    varchar(1024)                      null comment '用户头像',  
    gender       tinyint                            null comment '性别 1-男 0-女',  
    phone        varchar(32)                        null comment '电话',  
    email        varchar(64)                        null comment '邮箱',  
    birthday     date                               null comment '生日',  
    userStatus   int      default 0                 not null comment '状态 0-正常 ',  
    isDelete     tinyint  default 0                 not null comment '是否删除 0-正常 1-删除',  
    userRole     tinyint  default 0                 null comment '用户角色 0-普通 1-管理员',  
    createTime   datetime default CURRENT_TIMESTAMP null comment '注册时间 创建时间',  
    updateTime   datetime default CURRENT_TIMESTAMP null on update CURRENT_TIMESTAMP null comment '更新时间'  ,
            primary key (id)  
)  
    comment '用户表';
```
### 分类表
``` mysql
create table category  
(  
    id         int auto_increment comment '主键 分类id主键'  
        primary key,  
    name       varchar(32) default ''                not null comment '分类名称',  
    parentId   int                                   not null comment '父级id 一级分类为0 其余依赖上一级',  
    level      int                                   not null comment '分类层级 1-一级 2-二级 3-三级',    
    isDelete   tinyint     default 0                 not null comment '是否删除 0-正常 1-删除',  
    createTime datetime    default CURRENT_TIMESTAMP null comment '注册时间 创建时间',  
    updateTime datetime    default CURRENT_TIMESTAMP null on update CURRENT_TIMESTAMP comment '更新时间',  
    createUser int                                   not null comment '创建者id',  
    updateUser int                                   null comment '修改者id'  
)  
    comment '商品分类';
```
### 属性表

### 产品表

### 属性值表

### 产品图片表

### 评价表

### 订单表

### 订单项表

### banner表
``` mysql
create table banner  
(  
    id           bigint auto_increment comment 'banner id',  
    bannerUrl    varchar(1024)     not null comment 'banner图片地址',  
    goodsId      bigint            null comment '商品id',  
    categoryId   bigint            null comment '商品分类id',  
    indexType    int               not null comment '判断轮播图类型 根据商品id或分类id进行跳转 0-商品 1-分类',  
    bannerStatus tinyint default 0 not null comment '状态 0-正常 1-下架',  
    isDelete     tinyint default 0 not null comment '是否删除 0-正常 1-删除',  
    createTime   datetime default CURRENT_TIMESTAMP null comment '注册时间 创建时间',  
    updateTime   datetime default CURRENT_TIMESTAMP null on update CURRENT_TIMESTAMP null comment '更新时间',  
        primary key (id)  
)    comment 'banner 表';
```