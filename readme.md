[TOC]

# 项目结构



## 客户端

使用Unity，版本为Unity 5.5.0f3。  

### 介绍

本项目是一个卷轴射击游戏，玩家操纵飞机，在太空中与敌人的飞机进行战斗。

#### 游戏玩法 
玩家按住鼠标左键扫描并且锁定敌人，松开鼠标左键会自动发射导弹消灭敌人，最多同时扫描6个敌人。

#### 游戏流程
玩家有3条命应对数波敌人和一个boss。击败boss通关。  

#### 得分机制  
每消灭一个敌人能够得到一定分数。同时扫描多名敌人并且消灭会增加所得的分数，即实际的得分为每个敌人的基础分乘上倍率。同时扫描的敌人越多，倍率越高。  

游戏保存本次游戏最高得分和网络上所有玩家的最高得分，鼓励玩家不断挑战高分。  

### 游戏场景

#### 开始场景
显示游戏logo和一些简单的背景和文字引导。

#### 主要游戏场景
玩家通过鼠标朝向控制飞机，躲避并且扫描消灭敌机。    

显示游戏相关UI，如自机数，分数，扫描情况，分数倍率等。  

控制整个游戏的流程。

#### 结束场景
显示游戏结束，本局分数，本次游玩最高分，网络最高分等。

### 主要技术  
主要包括场景的搭建，动画效果，音效等的设置，材质的数值调整，游戏脚本的编写，UI的制作等。
这里主要介绍与编程相关度较高的游戏脚本的编写。

本游戏一共有56个游戏脚本，控制方方面面游戏中的元素。由c#语言编写。这里选择两个较为关键的技术介绍。

#### 有限状态自动机
对于boss不同阶段的行动设置状态，每个状态相应地编写移动方式和攻击方式和转移条件。这样能够避免大量的if...else...或者是switch。编写程序时也能很好地模块化。

#### 碰撞网格
在Unity引擎中，当激光快速移动时，碰撞器可能会漏掉一些物体，如果将碰撞器改成连续检查则会增加计算量导致卡顿。解决方法是以自己为中心，生成三角形碰撞网格，将碰撞网格设置为碰撞器或通过计算几何算法解决。

### 网络通信
使用HTTP协议，主要通过Unity自带的UnityWebRequest类完成。



## 后端

采用java，框架选择springboot + mybatis，使用restful api提供作为数据接口
关于各类组件的版本依赖，见pom.xml
提供用户登录（包括权限控制）服务和排行榜信息。
对于云端提供更多服务，有待后续增加（如云存档）



### 数据库
数据库使用mysql，mysql版本8.x，jdbc和connector版本见pom.xml

#### 游戏信息
- 排行榜sys_ranking
  - id, user_id, user_name, user_score

其他信息待补充

#### 用户信息

其中id全部为自增主键

- 用户表sys_user
  - id, user_name, user_password, user_email, user_info, head_img, create_time
- 权限表sys_privilege
  - id, privilege_name, privilege_url
- 角色表sys_role
  - id, role_name, enabled, create_by, create_time
- 用户角色关联表sys_user_role
  - user_id, role_id
- 角色权限关联表sys_role_privilege
  - role_id, privilege_id

**注意：**	 
此处为了方便对表进行直接操作， 此处没有创建表之间的外键关系。
对于表之间的关系， 会通过业务逻辑来进行限制 。




### 服务器
#### 持久层(DAO)

使用mybatis生成mapper，既数据库交互，针对每一张表生成一个mapper文件。

#### 业务层(service)

使用springboot框架，负责业务逻辑。

- 根据数据库信息生成排行榜
  - 进行排序，封装成Ranking，最后返回list
  - 进行筛选数据，一次取50条
- 根据数据库生成用户信息
  - 根据用户名或密码进行匹配
  - 针对不同的用户错误返回不同的结果，能区分是否存在用户

#### 展现层(web)

使用springboot框架和restful api，提供数据接口。

- 对客户端的登录反应进行判断
- 传送排行榜
- 封装数据

#### 实体类(domain)

POJO(Plain Old Java Object)

- 使用mcg生成的数据库对应实体类
- 符合客户端要求的数据类













