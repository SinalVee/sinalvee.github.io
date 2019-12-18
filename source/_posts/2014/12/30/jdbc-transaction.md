title: JDBC事务处理
date: 2014-12-30 16:45:02
updated: 2014-12-30 16:45:02
categories:
  - Java
tags:
  - java
  - jdbc
---

在JDBC中，事务操作默认是自动提交。也就是说，一条对数据库的更新表达式代表一项事务操作。操作成功后，系统将自动调用`commit()`来提交，否则将调用`rollback()`来回退。
可以通过调用`setAutoCommit(false)`来禁止自动提交。之后就可以把多个数据库操作的表达式作为一个事务，在操作完成后调用`commit()`来进行整体提交。  
倘若其中一个表达式操作失败，都不会执行到`commit()`，并且将产生响应的异常。此时就可以在异常捕获时调用`rollback()`进行回退。  
这样做可以保持多次更新操作后，相关数据的一致性。
```java
/*
 * 2014年12月30日16:24:04
 */

Connection conn;
Statement stat;
ResultSet rs;
try {
    Class.forName("oracle.jdbc.driver.OracleDriver");
    conn = DriverManager.getConnection("jdbc:oracle:thin:@127.0.0.1:1521:orcl", "test", "test");
    conn.setAutoCommit(false);
    stat = conn.createStatement();

    stat.executeUpdate("INSERT INTO student VALUES('00001', '老大', '男', '22', 'AA')");
    stat.executeUpdate("INSERT INTO student VALUES('00002', '老二', '女', '42', 'BB')");

    conn.commit();
    conn.setAutoCommit(true);
} catch (ClassNotFoundException e) {
    e.printStackTrace();
} catch (SQLException e) {
    e.printStackTrace();
    try {
        conn.rollback();
        System.out.println("回滚成功");
        conn.setAutoCommit(true);
    } catch (SQLException e1) {
        e1.printStackTrace();
    }
} finally {
    try {
        if (rs != null) {
            rs.close();
        }
    } catch (SQLException e) {
        e.printStackTrace();
    } finally {
        try {
            if (stat != null) {
                stat.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```
