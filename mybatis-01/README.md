

## Ŀ¼����
1. depend��mybatis��������jar��
2. conf�����Mybatis���������ļ��Լ��־û����ӳ���ļ��淶
3. junit��mybatisʹ��junit���԰���ȷ���޳�ͻ
4. required��mybatis����������İ�

-----

## ��������
1. �½�һ��web project
2. ���������jar����mybatis���İ���mybatis��������junit����
3.

-----


## һ��������SQL�ű�
```SQL
SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for `orders`
-- ----------------------------
DROP TABLE IF EXISTS `orders`;
CREATE TABLE `orders` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) NOT NULL COMMENT '�µ��û�id',
  `number` varchar(32) NOT NULL COMMENT '������',
  `createtime` datetime NOT NULL COMMENT '��������ʱ��',
  `note` varchar(100) DEFAULT NULL COMMENT '��ע',
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
  `username` varchar(32) NOT NULL COMMENT '�û�����',
  `birthday` date DEFAULT NULL COMMENT '����',
  `sex` char(1) DEFAULT NULL COMMENT '�Ա�',
  `address` varchar(256) DEFAULT NULL COMMENT '��ַ',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=27 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('1', '����', null, '2', null);
INSERT INTO `user` VALUES ('10', '����', '2014-07-10', '1', '������');
INSERT INTO `user` VALUES ('16', '��С��', null, '1', '����֣��');
INSERT INTO `user` VALUES ('22', '��С��', null, '1', '����֣��');
INSERT INTO `user` VALUES ('24', '������', null, '1', '����֣��');
INSERT INTO `user` VALUES ('25', '��С��', null, '1', '����֣��');
INSERT INTO `user` VALUES ('26', '����', null, null, null);

```



-----
## һ��������JavaBean
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


## MyBatis�������ļ�
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

## �־û����ӳ���ļ�
```XML
<?xml version="1.0" encoding="UTF-8" ?> 
<!DOCTYPE mapper  PUBLIC 
"-//mybatis.org//DTD Mapper 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- namespace:�����ռ䣬������SQL���� -->
<mapper namespace="test">
	<!-- id:sql���Ψһ��ʾ
		parameterType��ָ�������������
		resultType�����ؽ������
		#{}��ռλ������ռλ���ã�������صĽ���ǻ�������(string int long ....),��#{}�еı�����������д
		 �򵥵ĸ���ID��ѯһ���û�
		%{}��ƴ�ӷ����ַ���ԭ��ƴ�ӣ��������Ĳ����ǻ������ͣ���ô${}�еı���������value
		 -->
 	<select id="findUserById" parameterType="java.lang.Integer" resultType="cn.itheima.pojo.User">
 		select * from user where id = #{id}
 	</select>
 	
 	<!-- ������ؽ��Ϊ���ϣ����Ե���selectList����������������صĽ������һ�����ϣ�����ӳ���ļ���Ӧ�����óɼ��Ϸ��͵����� -->
 	<select id="findUserByUserName" parameterType="java.lang.String" resultType="cn.itheima.pojo.User">
 		<!-- /*�������д�Ļ���SQL��佫����һ��������������ͨ�������*/ 
 			����ʹ����ƴ�ӷ�����ʹ�ã���SQLע��ķ��գ����Խ���ʹ�ã� 	
 			select * from user where username like ${}
 		-->
 		select * from user where username like '%{value}%'
 	</select>
</mapper>
```
----

## һ�������Ĳ�������

```Java
package cn.itheima.test;

import java.io.InputStream;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import cn.itheima.pojo.User;

public class UserTest {

	@Test
	public void testFindUserById() throws Exception {

		/* ͨ���������������ļ���ȡ������Ȼ��ͨ�����������ļ��������������Ự���� */
		String resource = "SqlMapConfig.xml";
		InputStream inputstream = Resources.getResourceAsStream(resource);
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(inputstream);

		/* ͨ�����������Ự */
		SqlSession session = factory.openSession();

		/*
		 * ��1��������sql���(���� namespace + . + sql����id) ��2����������Ҫ���ҵı��
		 */
		User user = session.selectOne("test.findUserById", 1);
		System.out.println(user);
	}

	@Test
	public void findUserByUsername() throws Exception {

		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("SqlMapConfig.xml"));
		SqlSession session = factory.openSession();
		
		/*�������д�Ļ���SQL��佫����һ��������������ͨ�������*/
		List<User> list = session.selectList("test.findUserByUserName", "%��%");
		for (User user : list) {
			System.out.println(user.getUsername());
		}

	}

}

```
