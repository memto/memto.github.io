---
layout: post
ads: true
comments: true
published: true
title: Spring datasource config
tags:
  - Spring
categories:
  - program
---
```bash
# H2 DB
spring.datasource.url=jdbc:h2:file:C:/temp/test
spring.datasource.username=sa
spring.datasource.password=
spring.datasource.driverClassName=org.h2.Driver
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

# PostgreSQL
spring.datasource.url=jdbc:postgresql://<HOST>:<PORT>/<DB>?currentSchema=<SCHEMA>
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQL81Dialect

# MySQL
spring.datasource.url=jdbc:mysql://<HOST>:<PORT>/<DB>
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect

# Oracle
spring.datasource.url=jdbc:oracle:thin:@<HOST>:<PORT>:<DB>
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
spring.jpa.database-platform=org.hibernate.dialect.Oracle10gDialect

# SQL Server
spring.datasource.url=jdbc:sqlserver://<HOST>:<PORT>;databaseName=<DB>
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriver
spring.jpa.hibernate.dialect=org.hibernate.dialect.SQLServer2012Dialect
```
