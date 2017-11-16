# MyBatisDoc
To describe how to use MyBatis first!<BR />
![](https://camo.githubusercontent.com/3ba433e9aaff8fb8749c3fa980ff5839515057f8/687474703a2f2f70726f677265737365642e696f2f6261722f32383f7469746c653d70726f6772657373)&nbsp;![](https://camo.githubusercontent.com/5d1e2146f2113a6c55a81d131ba112462f24f23c/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f736976616e5775303232322f48696265726e617465446f632e737667)


## MyBatis总配置文件
```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration  PUBLIC 
"-//mybatis.org//DTD Config 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC"/>
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver"/>
				<property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8"/>
				<property name="username" value="root"/>
				<property name="password" value="1018222wxw"/>
			</dataSource>
		</environment>
	</environments>
</configuration>
```
