title: Oracle中的自增实现(mysql中的auto_increment)
date: 2014-12-17 18:13:16
updated: 2014-12-17 18:13:16
categories:
  - Database
  - Oracle
tags:
  - oracle
---

自增功能在数据库应用中很广泛，但是Oracle中却没有如mysql中auto_increment那么简单的创建自增，不过实现起来也不麻烦，现在用一个例子来说明自增的实现：

## 1.建立数据表
```
create table t_user(
    user_id integer primary key,
    user_name varchar2(20),
    user_age integer
);
```
## 2.创建squence
```
create sequence t_user_seq
       minvalue 1
       maxvalue 99999999
       increment by 1
       start with 1;
```

Name : 创建的名字  
Min value : 最小计数  
Max value : 最大计数  
Start with : 起始计数  
Increment by : 步长  
Cache size : 缓存序列  
指定Cache，oracle会预先在内存中放置一组指定大小的序列，当使用完这些序列后再生成下一组，这样会存取得快些，但当数据库关闭等情况时，下一次再生成序列时可能会使序列间断，不是一串连续的号，当不是特别需要连续的序列时最好指定；不填写Cache值，会使用默认设置；当Cache size设置为0时 为nocache，这样会产生连续的序列。  
Cycle : 循环序列，当到达最大值后从最小值重新开始  
Order : 保证序列产生的顺序和请求的顺序是一致的，在并行模式下如果A、B同时对序列请求那么先产生的序列号必然返回给先请求的用户。例如当前序列号为10，A先请求B后请求那么11一定返回给A，12给B，在noorder的情况下，有可能11给B，12给A。这种情况只发生在oracle并行服务器上，大多数情况下不需要。

## 3.创建触发器(Trigger)
```create or replace trigger t_user_tri
    before insert on t_user /*触发条件：当向表dectuser执行插入操作时触发此触发器*/
    for each row  /*对每一行都检测是否触发*/
    begin /*触发器开始*/
        select t_user_seq.nextval into :new.user_id from dual;  /*触发器主题内容，即触发后执行的动作，在此是取得序列t_user_seq的下一个值插入到表t_user中的user_id字段中,注意：new.user_id 是new.加上原表的主键*/
    end;
```

到此oracle中自增的实现已经完成了，可以试着插入一条记录看一看是否成功。
