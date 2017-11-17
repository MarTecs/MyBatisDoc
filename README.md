# MyBatisDoc
To describe how to use MyBatis first!<BR />
![](https://camo.githubusercontent.com/3ba433e9aaff8fb8749c3fa980ff5839515057f8/687474703a2f2f70726f677265737365642e696f2f6261722f32383f7469746c653d70726f6772657373)&nbsp;![](https://camo.githubusercontent.com/5d1e2146f2113a6c55a81d131ba112462f24f23c/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f736976616e5775303232322f48696265726e617465446f632e737667)



## 一个基本的SQL脚本
```SQL
SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for `orders`
-- ----------------------------
DROP TABLE IF EXISTS `orders`;
CREATE TABLE `orders` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) NOT NULL COMMENT '下单用户id',
  `number` varchar(32) NOT NULL COMMENT '订单号',
  `createtime` datetime NOT NULL COMMENT '创建订单时间',
  `note` varchar(100) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  KEY `FK_orders_1` (`user_id`),
  CONSTRAINT `FK_orders_id` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of orders
-- ----------------------------
INSERT INTO `orders` VALUES ('3', '1', '1000010', '2015-02-04 13:22:35', null);
INSERT INTO `orders` VALUES ('4', '1', '1000011', '2015-02-03 13:22:41', null);
INSERT INTO `orders` VALUES ('5', '10', '1000012', '2015-02-12 16:13:23', null);

-- ----------------------------
-- Table structure for `user`
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(32) NOT NULL COMMENT '用户名称',
  `birthday` date DEFAULT NULL COMMENT '生日',
  `sex` char(1) DEFAULT NULL COMMENT '性别',
  `address` varchar(256) DEFAULT NULL COMMENT '地址',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=27 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('1', '王五', null, '2', null);
INSERT INTO `user` VALUES ('10', '张三', '2014-07-10', '1', '北京市');
INSERT INTO `user` VALUES ('16', '张小明', null, '1', '河南郑州');
INSERT INTO `user` VALUES ('22', '陈小明', null, '1', '河南郑州');
INSERT INTO `user` VALUES ('24', '张三丰', null, '1', '河南郑州');
INSERT INTO `user` VALUES ('25', '陈小明', null, '1', '河南郑州');
INSERT INTO `user` VALUES ('26', '王五', null, null, null);

```



-----
## 一个基本的JavaBean
```Java
package cn.itheima.pojo;

import java.io.Serializable;
import java.util.Date;

public class User implements Serializable {
	private int id;
	private String username;
	private String sex;
	private Date birthday;
	private String address;

	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	public Date getBirthday() {
		return birthday;
	}
	public void setBirthday(Date birthday) {
		this.birthday = birthday;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	@Override
	public String toString() {
		return "User [id=" + id + ", username=" + username + ", sex=" + sex
				+ ", birthday=" + birthday + ", address=" + address + "]";
	}

	
	

}

```
------


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

----

## 持久化类的映射文件
```XML
<?xml version="1.0" encoding="UTF-8" ?> 
<!DOCTYPE mapper  PUBLIC 
"-//mybatis.org//DTD Mapper 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- namespace:命名空间，用来做SQL隔离 -->
<mapper namespace="test">
	<!-- id:sql语句唯一表示
		parameterType：指定传入参数类型
		resultType：返回结果类型
		#{}：占位符，起到占位作用，如果返回的结果是基本类型(string int long ....),则#{}中的变量可以任意写
		 简单的根据ID查询一个用户
		%{}：拼接符，字符串原样拼接，如果传入的参数是基本类型，那么${}中的变量必须是value
		 -->
 	<select id="findUserById" parameterType="java.lang.Integer" resultType="cn.itheima.pojo.User">
 		select * from user where id = #{id}
 	</select>
 	
 	<!-- 如果返回结果为集合，可以调用selectList方法，这个方法返回的结果就是一个集合，所以映射文件中应该配置成集合泛型的类型 -->
 	<select id="findUserByUserName" parameterType="java.lang.String" resultType="cn.itheima.pojo.User">
 		<!-- /*如果这样写的话，SQL语句将会是一个参数，而不是通配符查找*/ 
 			这里使用了拼接符进行使用，有SQL注入的风险，所以谨慎使用！ 	
 			select * from user where username like ${}
 		-->
 		select * from user where username like '%{value}%'
 	</select>
</mapper>
```
