Mybatis就是基于JDBC的基础做的。而 JDBC 用的就是JDK API中 java.sql 包下的类！所以具体可以参考API。



JDBC基本步骤如下：

```java
// 1 和 3 步骤中会有异常，IDE加上try/catch就好

//1、加载JDBC驱动，不写 DriverManager.registerDriver(Class.forName(driverClass).new Instance()) 的原因是：
//因为com.mysql.jdbc.Driver 类中有 注册驱动的静态代码块，而使用 Class.forName 会引起类的初始化，执行静态代码块，因此不用重复写。

Class.forName("com.mysql.jdbc.Driver");

//2、建立数据库连接

String url = "jdbc:mysql://localhost:3306/testdb?user=root&password=root&useUnicode=true&characterEncoding=UTF-8";

Connection conn = DriverManager.getConnection(url); 

//3、创建一个statement.

//要执行SQL语句必须先获得一个java.sql.statement实例。statement有3种类型：

  //(1)执行静态sql语句，用Statement对象

  Statement stmt = con.createStatement();

  //(2)执行动态sql语句，用PreparedStatement对象

  PreparedStatement pstmt = con.prepareStatement(sql);

  //(3)执行数据库存储过程，用CallableStatement对象  

  CallableStatement cstmt = con.prepareCall("{CALL demoSp(? , ?)}"); 

//4、执行Sql语句获得结果

  Statement接口提供了三种执行SQL语句的方法：executeQuery() 、executeUpdate()和execute()

  //(1)ResultSet executeQuery(String sqlString) 返回一个结果集对象

  ResultSet rs = stmt.executeQuery(sqlString);

  //(2)int executeUpdate(String sqlString)：返回更新计数。用于执行INSERT、UPDATE、DELETE语句。以及SQL DDL语句，如create/drop table

  int rows = stmt.executeUpdate(sqlString);

  //(3)execute(sqlString) 返回多个结果集，多个更新计数或二者组合的语句

  Boolean flag = stmt.execute(sqlString);

//5、处理结果：

  //若返回的是更新记录数就看情况了。

  //返回结果集：

	while(rs.next()){
		rs.getXXX("name"); //提供了各种类型的获取方法
		rs.getXXX(1); //从左到右编号，从1开始。这种更高效
	}

//6、关闭JDBC

  //1、关闭结果集

  //2、关闭声明  

  //3、关闭连接对象  

	if(rs != null){  // 关闭结果集
        try{  
            rs.close() ;  
        }catch(SQLException e){  
            e.printStackTrace() ;  
        }  
	}  

	if(stmt != null){  // 关闭声明  
		try{  
			stmt.close() ;  
		}catch(SQLException e){  
			e.printStackTrace() ;  
		}  
	}  

	if(conn != null){ // 关闭连接对象  
		try{  
			conn.close() ;  
		}catch(SQLException e){  
			e.printStackTrace() ;  
		}  
	}
```

