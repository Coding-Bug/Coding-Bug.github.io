---
title: sptingboot集成mybatis
date: 2021-03-30 12:23:17
catagories:
- springboot
tags:
- JAVA
- SptingBoot
- 数据库
---


# SpringBoot集成mybatis


> ### 添加依赖

&emsp;&emsp;在pom.xml文件中添加MySQL驱动依赖和mybatis依赖,MySQL不用加版本号，继承了父类的版本号，也可以自己指定版本号。
```
<!--MySQL驱动-->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>


<!--MyBatis整合springboot框架的起步依赖-->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.0.0</version>
		</dependency>
```
>### 属性设置
&emsp;&emsp;在GeneratorMapper.xml文件中进行连接数据库的属性设置，设置完成后生成即可。