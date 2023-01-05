### Idea-JavaFX+maven连接数据库

首先要连接数据库

即要写一个`DBUtil类`

```java
package com.example.fifthjob;

import java.sql.*;

public class DBUtil {

    private static String jdbcUrl = "jdbc:mysql://localhost:3306/javapractice?useSSL=false&characterEncoding=UTF-8";
    private static String dbUserName = "yourUserName";
    private static String dbPwd = "yourPwd";
    
    // 建议使用com.mysql.cj.jdbc.Driver, 而不是com.mysql.jdbc.Driver
    private static String jdbcName = "com.mysql.cj.jdbc.Driver";

    /**
     * 获取数据库连接
     * @return
     * @throws Exception
     */
    public static Connection getConnection() throws Exception{
        Class.forName(jdbcName);
        Connection conn = DriverManager.getConnection(jdbcUrl, dbUserName, dbPwd);
        return conn;
    }

    /**
     * 关闭数据库连接
     * @param conn
     * @throws Exception
     */
    public void closeConn(Connection conn) throws Exception{
        if(conn != null){
            conn.close();
        }
    }

    // 测试连接状态
    public static void main(String[] args) {
        DBUtil dbUtil = new DBUtil();
        try {
            dbUtil.getConnection();
            System.out.println("数据库连接成功！");
        } catch (Exception e) {
            //TODO: handle exception
            e.printStackTrace();
            System.out.println("数据库连接失败");
        }

    }
} 
```

