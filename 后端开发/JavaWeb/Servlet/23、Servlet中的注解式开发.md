#  Servlet中的注解式开发

## 一、Servlet注解，简化配置

- [x] oa项目中的web.xml文件
   - 当开发项目过大时，web.xml中需要配置的文件会非常庞大，可能会有几十兆
   - 在web.xml文件中进行Servlet信息的配置，显然开发效率较低，每一个都需要配置、
   - 而且在web.xml文件中的配置是很少需要修改的
- [x] Servlet3.0版本之后，退出了各种Servlet基于注解式开发
   - 优点
      - 开发效率高，不需要编写大量的配置信息，直接写在Java类上使用注解进行标注
      - web.xml文件体积变小了
   - 并不是有了注解之后，web.xml文件就不需要了
      - 有的一些需要变化的信息，还是需要配置到web.xml文件中，一般都是`注解+配置文件`的开发模式
      - 一些不会经常变化修改的配置建议使用注解，一些可能会被修改的建议写到配置文件中

​	

## 二、注解的使用

- [x] 第一个注解

  ```java
  import jakarta.servlet.annotation.WebServlet;
  ```

  

![20220424174658](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424174658.png)

- [x] Servlet类上使用：`@WebServlet`
- [x] Servlet注解的属性
   - `name`属性L：用来指定Servlet的名字，等同于`<servlet-name>`
   - `urlPatterns`属性：用来指定Servlet的映射路径，可以指定多个字符串，等同于`url-pattern`
   - `loadOnStartUp`属性：用来指定服务器在启动阶段是否加载Servlet，等同于`<load-on-startup>`
   - `value`属性
      - ⚠️如果注解的属性名是value的话，属性名可以省略`@WebServlet(value = "/a")`可以写为`@WebServlet("/a")`
- [x] 注解对象的使用格式`initPatams`
   - `@注解名称(属性名=属性值,属性名=属性值,属性名=属性值)`

![20220424174739](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424174739.png)

- [x] 不需要将所有的属性都写上，只需要提供需要的（需要什么用什么）

⚠️当注解的属性是一个数组，并且数字中只有一个元素，大括号可以省略

​	

## 三、模板方法设计模式解决类爆炸

- [x] 如果一个复杂的业务系统，类太多，会导致类爆炸（类的数量急剧增多）
- [x] 可以使用模板设计模式解决类爆炸

❓如何解决类爆炸问题

   - 一个请求对应一个方法
   - 一个业务对应一个Servlet类
   - 处理部门的业务的对应一个DeptServlet
   - 处理用户相关的业务对应一个UserServlet
   - 处理银行卡片业务的对应一个CardServlet
- [x] 通配写法

![20220424174748](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424174748.png)
```java
package oa.action;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import oa.untils.DBUtil;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * @Author: 小雷学长
 * @Date: 2022/3/23 - 21:31
 * @Version: 1.8
 */

//模板类
@WebServlet({"/dept/list", "/dept/save", "/dept/edit", "/dept/detail", "/dept/delete", "/dept/update"})
//通配写法
//只要是路径已"/dept"开始的，都走Servlet
//@WebServlet("/dept/*")
public class DeptServlet extends HttpServlet {

    //模板方法
    //重写Servlet方法（并没有重写doGet或者doPost


    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        //获取ServletPath
        String servletPath = request.getServletPath();

        if ("/dept/list".equals(servletPath)) {
            doList(request, response);
        } else if ("/dept/save".equals(servletPath)) {
            doSave(request, response);
        } else if ("/dept/edit".equals(servletPath)) {
            doEdit(request, response);
        } else if ("/dept/detail".equals(servletPath)) {
            doDeail(request, response);
        } else if ("/dept/delete".equals(servletPath)) {
            doDelete(request, response);
        } else if ("/dept/update".equals(servletPath)) {
            doUpdate(request, response);
        }
    }

    private void doList(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        {
            //获取应用的根路径
            String contextPath = request.getContextPath();

            //设置响应的内容类型
            response.setContentType("text/html");
            PrintWriter out = response.getWriter();

            out.print("        <!DOCTYPE html>");
            out.print("<html lang='en'>");
            out.print("<head>");
            out.print("	<meta charset='UTF-8'>");
            out.print("	<title>部门列表</title>");
            out.print("</head>");

            out.print("<body>");
            out.print("<script type='text/javascript'>");
            out.print("        function del(dno) {");
            out.print("    if(window.confirm('亲，删了不可恢复哦！')){");
            out.print("        document.location.href = '" + contextPath + "/dept/delete?deptno=' + dno");
            out.print("    }");
            out.print("}");
            out.print("</script>");
            out.print("<h1 align='center'>部门列表</h1>");
            out.print("<hr>");
            out.print("<table border='1px' align='center' width='50%'>");
            out.print("	<tr>");
            out.print("		<th>序号</th>");
            out.print("		<th>部门编号</th>");
            out.print("		<th>部门名称</th>");
            out.print("		<th>操作</th>");
            out.print("	</tr>");
            out.print("	<!--以上是固定的-->");
        /*
        上面一部分是死的
         */

            /*
             * 连接数据库
             */
            Connection conn = null;
            PreparedStatement ps = null;
            ResultSet rs = null;

            try {

                //获取连接
                conn = DBUtil.getConnection();

                //获取预编译的数据库操作对象
                String sql = "select deptno,dname ,loc from dept";
                ps = conn.prepareStatement(sql);

                //执行SQL语句
                rs = ps.executeQuery();

                //处理结果集
                int i = 0;
                while (rs.next()) {
                    String deptno = rs.getString("deptno");
                    String dname = rs.getString("dname");
                    String loc = rs.getString("loc");

                    out.print("	<tr>");
                    out.print("		<td>" + (++i) + "</td>");
                    out.print("		<td>" + deptno + "</td>");
                    out.print("		<td>" + dname + "</td>");
                    out.print("		<td>");
                    out.print("<a href='javascript:void(0)' onclick='del(" + deptno + ")'>删除</a>");
                    out.print("			<a href='/oa/dept/edit?deptno=" + deptno + "'>修改</a>");
                    out.print("			<a href='" + contextPath + "/dept/detail?deptno=" + deptno + "'>详情</a>");
                    out.print("		</td>");
                    out.print("	</tr>");

                }
            } catch (SQLException e) {
                e.printStackTrace();
            } finally {

                //释放资源
                DBUtil.close(conn, ps, rs);
            }

        /*
        下面一部分是死的
         */
            out.print("	<!--一下是固定的-->");
            out.print("</table>");
            out.print("<hr>");
            out.print("<a href='" + contextPath + "/add.html'>新增部门</a>");
            out.print("</body>");
            out.print("</html>");
        }
    }


    private void doSave(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        {


            //获取部门信息
            String deptno = request.getParameter("deptno");
            String dname = request.getParameter("dname");
            String loc = request.getParameter("loc");

            //连接数据库执行insert语句
            Connection conn = null;
            PreparedStatement ps = null;
            int count = 0;

            try {
                conn = DBUtil.getConnection();

                String sql = "insert into dept(deptno,dname,loc) values(?,?,?)";
                ps = conn.prepareStatement(sql);
                ps.setString(1, deptno);
                ps.setString(2, dname);
                ps.setString(3, loc);

                count = ps.executeUpdate();

            } catch (SQLException e) {
                e.printStackTrace();

            } finally {
                DBUtil.close(conn, ps, null);
            }

            //判断新增成功还是失败
            if (count == 1) {
                //新增成功
                //仍然跳回到部门列表页面
                //部门列表页面需要执行另一个Servlet，利用转发机制
                //转发一次请求
//            request.getRequestDispatcher("/dept/list").forward(request, response);

                //建议使用重定向,因为重定向是一次全新的请求，不会受dopost、doget方法影响
                response.sendRedirect(request.getContextPath() + "/dept/list");
            } else {
                //新增失败
//            request.getRequestDispatcher("/error.html").forward(request, response);

                //建议使用重定向,因为重定向是一次全新的请求，不会受dopost、doget方法影响
                response.sendRedirect(request.getContextPath() + "/error.hmtl");
            }


        }
    }

    private void doEdit(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        {

            response.setContentType("text/html");
            PrintWriter out = response.getWriter();

            String contextPath = request.getContextPath();


            out.print("<html lang='en'>");
            out.print("<head>");
            out.print("	<meta charset='UTF-8'>");
            out.print("	<title>修改部门</title>");
            out.print("</head>");
            out.print("<body>");
            out.print("<h1>修改部门</h1>");
            out.print("<hr>");
            out.print("<form action='" + contextPath + "/dept/update' method='post'>");


            //获取部门编号
            String deptno = request.getParameter("deptno");

            //连接数据库执行insert语句
            Connection conn = null;
            PreparedStatement ps = null;
            ResultSet rs = null;

            try {
                conn = DBUtil.getConnection();

                String sql = "select dname,loc from dept where deptno=?";
                ps = conn.prepareStatement(sql);
                ps.setString(1, deptno);

                rs = ps.executeQuery();

                if (rs.next()) {
                    String dname = rs.getString("dname");
                    String loc = rs.getString("loc");

                    //输出动态网页
                    out.print("                部门编号<input type='text' name='deptno' value='" + deptno + "' readonly><br><!--readonly只读-->");
                    out.print("                部门名称<input type='text' name='dname' value='" + dname + "'><br>");
                    out.print("                部门位置<input type='text' name='loc' value='" + loc + "'><br>");

                }

            } catch (SQLException e) {
                e.printStackTrace();

            } finally {
                DBUtil.close(conn, ps, rs);
            }

            //判断新增成功还是失败

            out.print("	<input type='submit' value='修改'><br>");
            out.print("");
            out.print("</form>");
            out.print("");
            out.print("</body>");
            out.print("</html>");

        }
    }

    private void doDeail(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        {

            response.setContentType("text/html");
            PrintWriter out = response.getWriter();


            out.print("<!DOCTYPE html>");
            out.print("<html lang='en'>");
            out.print("<head>");
            out.print("	<meta charset='UTF-8'>");
            out.print("	<title>部门详情</title>");
            out.print("</head>");
            out.print("<body>");
            out.print("<h1>部门详情</h1>");
            out.print("<hr>");


            //获取部门编号
            //虽然提交的是30，但服务器获取的是30这个字符串
            String deptno = request.getParameter("deptno");


            /*
             * 连接数据库
             */
            Connection conn = null;
            PreparedStatement ps = null;
            ResultSet rs = null;

            try {

                //获取连接
                conn = DBUtil.getConnection();

                //获取预编译的数据库操作对象
                String sql = "select deptno,dname,loc from dept where deptno=?";
                ps = conn.prepareStatement(sql);

                ps.setString(1, deptno);
                //这个结果集一定只有一个元素


                //执行SQL语句
                rs = ps.executeQuery();

                //处理结果集
                if (rs.next()) {
                    String dname = rs.getString("dname");
                    String loc = rs.getString("loc");

                    out.print("部门编号" + deptno + "<br>");
                    out.print("部门名称" + dname + "<br>");
                    out.print("部门位置" + loc + "<br>");


                }
            } catch (SQLException e) {
                e.printStackTrace();
            } finally {

                //释放资源
                DBUtil.close(conn, ps, rs);
            }

            out.print("");
            out.print("<form action='list.html'>");
            out.print("	<input type='button' value='后退' onclick='window.history.back()'>");
            out.print("");
            out.print("</form>");
            out.print("");
            out.print("</body>");
            out.print("</html>");
        }
    }

    public void doDelete(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        {

            //根据部门编号，删除部门
            //获取部门编号
            String deptno = request.getParameter("deptno");

            //连接数据库删除数据
            Connection conn = null;
            PreparedStatement ps = null;

            int count = 0;

            try {
                conn = DBUtil.getConnection();

                //开启事务（自动提交机制关闭）（非必要）
//            conn.setAutoCommit(false);

                String sql = "delete from dept where deptno=?";
                ps = conn.prepareStatement(sql);
                ps.setString(1, deptno);

                //返回值是：影响了数据库表当中多少条记录
                count = ps.executeUpdate();

                //事务提交（非必要）
//            conn.commit();
            } catch (SQLException e) {
                e.printStackTrace();

            } finally {
                DBUtil.close(conn, ps, null);
            }

            //判断删除成功还是失败
            if (count == 1) {
                //删除成功
                //仍然跳回到部门列表页面
                //部门列表页面需要执行另一个Servlet，利用转发机制
                //request.getRequestDispatcher("/dept/list").forward(request, response);

                //建议使用重定向,因为重定向是一次全新的请求，不会受dopost、doget方法影响
                response.sendRedirect(request.getContextPath() + "/dept/list");

            } else {
                //删除失败
                //建议使用重定向,因为重定向是一次全新的请求，不会受dopost、doget方法影响
                response.sendRedirect(request.getContextPath() + "/error.html");
            }

        }

    }

    private void doUpdate(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        {


            response.setContentType("text/html");
            PrintWriter out = response.getWriter();

            String deptno = request.getParameter("deptno");
            String dname = request.getParameter("dname");
            String loc = request.getParameter("loc");

            //连接数据库执行insert语句
            Connection conn = null;
            PreparedStatement ps = null;
            ResultSet rs = null;
            int count = 0;

            try {
                conn = DBUtil.getConnection();

                String sql = "update dept set dname = ?,loc = ? where deptno = ?";
                ps = conn.prepareStatement(sql);
                ps.setString(1, dname);
                ps.setString(2, loc);
                ps.setString(3, deptno);

                count = ps.executeUpdate();

            } catch (SQLException e) {
                e.printStackTrace();

            } finally {
                DBUtil.close(conn, ps, rs);
            }

            if (count == 1) {
                //更新成功
                //跳转到部门列表页面，转发机制
//    request.getRequestDispatcher("/dept/list").forward(request,response);

//建议使用重定向,因为重定向是一次全新的请求，不会受dopost、doget方法影响
                response.sendRedirect(request.getContextPath() + "/dept/list");
            } else {
                //更新失败
//    request.getRequestDispatcher("/error.html").forward(request,response);

                //建议使用重定向,因为重定向是一次全新的请求，不会受dopost、doget方法影响
                response.sendRedirect(request.getContextPath() + "/error.html");
            }
        }
    }
}


```
