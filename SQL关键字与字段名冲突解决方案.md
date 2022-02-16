---
title: SQL关键字与字段名冲突解决方案
categories:
  - SQL
tags:
  - SQL
  - backend
abbrlink: 51d77a6d
date: 2021-03-27 19:29:50
---

## 一.问题描述

在一次写项目时,使用`SpringBoot`+`Mybatis`构建了框架,然后当我写好XML文件后,正自信满满的使用idea自动生成了测试类,在启动后却发现报以下错误

```bash
org.springframework.jdbc.BadSqlGrammarException: 
### Error querying database.  Cause: java.sql.SQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ',picname,shopintr,price,cpurl
     
        FROM tb_family_agent_info
        OR' at line 3
```

![image-20210327193453360](https://i.loli.net/2021/03/27/XVLWstp6Kz2PIaF.png)

entity实体如下:



```java

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
@Data
@NoArgsConstructor
@AllArgsConstructor
public class FamilyAgent {

    //主键
    private Integer id;

    //排名
    private Integer rank;
    ...
}
```

可以看到其中有个rank字段,就是这个rank字段和mysql的rank函数冲突

Mapper.xml如下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.moni.car_learning.mapper.FamilyAgentMapper">
    <sql id="Base_Column">
        id,shopname,shopurl,subpoints,rank,picname,shopintr,price,cpurl
    </sql>

    <select id="findAll" resultType="cn.moni.car_learning.entity.FamilyAgent">
        select
        <include refid="Base_Column"></include>
        from tb_family_agent_info
        ORDER BY rank DESC
    </select>
</mapper>
```

因为在idea控制台只能看到那么多信息,所以想去mysql中具体看看是什么问题

在Navicat中显示如下

![image-20210327214722889](https://i.loli.net/2021/03/27/MwzrcqVBxtkAPme.png)



## 二.问题解决

这时有两种解决的方式:

- 修改该字段为其他名字, MySQL的关键字/保留字可自行百度, 博主这次遇到的**”rank"**字
	**强烈建议不要使用MySQL的关键字/保留字作为数据库的 库名/表名/字段名**

- 如果一定要使用关键字/保留字, 则不论是 **建库/建表/插入/查询** 时, 都在该字段前后加上撇号: ```(键盘Esc键下面那个键)
	**示例** :

```mysql
select id, `rank' from user;
```

## 三.我的解决方式

这里我是使用的第二种方式

```xml
<sql id="Base_Column">
        id,shopname,shopurl,subpoints,`rank`,picname,shopintr,price,cpurl
    </sql>
<select id="findAll" resultType="cn.moni.car_learning.entity.FamilyAgent">
        select
        <include refid="Base_Column"></include>
        from tb_family_agent_info
        ORDER BY `rank` DESC
    </select>
```

在xml文件中加上```

![image-20210327214956703](https://i.loli.net/2021/03/27/xQ7cqkTH3pFs1fO.png)

test通过!!!