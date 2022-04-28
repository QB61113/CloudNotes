# Servlet中编写JDBC程序连接数据库

## 一、怎么做？

- 在Servlet中直接编写Java代码即可（JDBC）
- 需要将驱动jar包拷贝到webapps\crm\WEB-INF\lib目录下

## 二、实现连接数据库

```java
package Servlet;

/**
 * @Author: 小雷学长
 * @Date: 2022/3/16 - 10:06
 * @Version: 1.8
 */

import jakarta.servlet.Servlet;
import jakarta.servlet.ServletException;
import jakarta.servlet.*;

import java.io.IOException;


import java.io.IOException;
import java.io.PrintWriter;
import java.sql.*;


public class StudentServlet implements Servlet {


    public void init(ServletConfig config)
            throws ServletException {

    }

    public void service(ServletRequest request, ServletResponse response)
            throws ServletException, IOException {

        /*
        打印到浏览器上
         */
        response.setContentType("text/html");
        //返回out
        PrintWriter out = response.getWriter();

        /*
        编写JDBC代码，连接数据，查询所有信息
         */
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            //注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");

            //获取连接
            String url = "jdbc:mysql://localhost:3306/practice";
            String user = "root";
            String password = "123456";
            conn = DriverManager.getConnection(url, user, password);

            //获取预编译的数据库操作对象
            String sql = "select id,informant_name from t_student";
            ps = conn.prepareStatement(sql);

            //执行SQL
            rs = ps.executeQuery();

            //处理查询结果集
            while (rs.next()) {
                String id = rs.getString("id");
                String informant_name = rs.getString("informant_name");

                //打印out
                out.print(id + "," + informant_name + "<br>");

            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if (ps != null) {
                try {
                    ps.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public void destroy() {

    }

    public String getServletInfo() {
        return "";
    }

    public ServletConfig getServletConfig() {
        return null;
    }


}

```
