---
title: MyBatis学习笔记(1)
date: 2018-10-30 15:37:39
tags:
    - mybatis
    - java
categories: 
    - java
    - note
---

## 配置环境

* 基础 jar 包 : MyBatis 自带所有 jar 包
* 编写数据库的 JAVA 类并提供 GET / Set 方法

### 配置文件编写

``` python
<configuration>
    // 导入 properties 文件
    <properties resource="jdbc.properties" ></properties>
    <!-- 和 spring 整合后 environments 配置将废除 -->
    <environments default="development">
        <environment id="development">
            <!-- 使用 JDBC 事务管理 -->
            <transactionManager type="JDBC" />
            <!-- 数据库连接池 -->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url" value="${jdbc.url}" />
                <property name="username" value="${jdbc.user}" />
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 加载映射文件 -->
    <mappers>
        <mapper resource="url/mapper.xml"/>
    </mappers>
</configuration>
```

### 映射文件编写

``` python
<!-- namespace 用于隔离 sql 语句 -->
<mapper namespace="student">
    // id -> sql 语句标识符  parameterType -> 入参的数据类型  resultType -> 返回结果数据类型
    <select id="getStudentById" parameterType="int" resultType="com.uerax.mybatis.domain.Student">
        //  #{} 点位符号, 类似 JDBC 的 ?
        //  ${} 字符串拼接指令, 如果入参为普通类型 {} 只能写 value 如 : '%${value}%'
    </select>

    // 多个查询
    <select id="getStudentByStudentName" parameterType="string" resultType="com.uerax.mybatis.domain.Student">
        // sql
    </select>

    // 插入
    <insert id="insertStudent" parameterType="com.uerax.mybatis.domain.Student">
        // sql
    </insert>

    // 修改
    <update id="updateStudent" parameterType="com.uerax.mybatis.domain.Student">
        // sql
    </update>

    // 删除
    <delete id="deleteStudent" parameterType="int">
        // sql
    </delete>
</mapper>
```

### 调用方法

``` python
// 创建 SqlSeesionFactoryBuilder 对象
SqlSessionFactoryBuilder ssfb = new SqlSessionFactoryBuilder();

// 创建核心配置文件输入流
InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");

// 通过输入流创建 SqlSessionFactory 对象
SqlSessionFactory ssf = ssfb.build(is);

// 创建SqlSession 对象
SqlSession ss = ssf.openSession();

// 执行查询 (单个)
Student stu = ss.selectOne("student.getStudentById", 2);
// 查找多个, 映射的 resultType 直接用 List 内部数据类型
List<Student> list = selectList();

// 执行插入
Student student = new Student();
ss.insert("student.insertStudent", student);
// 必须执行 commit 提交才能成功插入
ss.commit();

// 执行修改, 删除
// 如果参数包含多个 Student 类的属性则将需要的属性赋值即可
Student stu = new Student();
ss.update("student.updateStudent", stu);
ss.delete("student.deleteStudent", 1);
// 必须执行 commit 提交才能成功插入
ss.commit();
ss.close();
```