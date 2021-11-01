# MyBatis

## 传参方式

1. 一个简单类型（基本类型和String）参数：直接按照普通函数传参即可，无特殊要求

2. 多个参数

   1. **使用对象传参（使用比较多）**

      ```java
      Student student = new Student();
      student.setName("张丽");
      student.setAge(19);
      List<Student> stuList = studentDao.selectUseResultMap(student);
      ```

      ```xml
      <select id="selectUseResultMap" resultMap="studentMap">
          select id, name, email, age
          from su.student
          <!--在mapper文件之中，使用#{属性名}进行参数引用-->
          where name = #{name}
             or age = #{age}
      </select>
      ```

   2. 使用@Param()标签来传，在标签后面指明数据类型及形参名 （使用比较少）

      ```java
      // @Param("在mapper文件之中使用的别名") String name
      List<Student> selectMultiParam(@Param("personName") String name, @Param("personAge") int age);
      ```

      ```xml
      <select id="selectMultiParam" resultType="com.fightersu.domain.Student">
          select id, name, email, age
          from su.student
          <!--personName是在DAO接口里面的@Param()标签的别名-->
          where name = #{personName}
             or age = #{personAge}
      </select>
      ```

   3. **涉及表名，列名，不同列排序等操作时，必须使用@Param标签，并使用\${列名、表名}来引用**

      ```java
      Student findByDiffField(@Param("col") String columnName, @Param("val") Object value);
      ```

      ```xml
      <select id="findByDiffField" resultType="com.fightersu.domain.Student">
      select *
      from su.student
      where ${col} = #{val}
      </select>
      ```

   4. 使用位置来传参，通过#{arg0},#{arg1}。。来引用

      ```java
      List<Student> selectByNameAndAge(String name, int age);
      ```

      ```xml
      <select id="selectByNameAndAge" resultType="com.fightersu.domain.Student">
           select id, name, email, age
           from su.student
           where name = #{arg0}
              or age = #{arg1}
       </select>
      ```

      

   5. 使用Map来传参，其key为参数名，在在mapper文件之中，通过#{key}引用传进去的参数

      ```java
      List<Student> selectMultiMap(Map<String, Object> map);
      ```

      



## 实体类的属性名和数据库的列名不一致

```xml
<!-- 在查询语句之中使用别名，让实际查询结果和属性名一致 -->
<select id="selectUseFieldAlias" resultType="com.fightersu.entity.PrimaryStudent">
    select id as stuId, name as stuName, age as stuAge                            
    from su.student                                                               
    where name = #{stuName}                                                       
       or age = #{stuAge}                                                         
</select>                                                                         

<!-- 自定义返回resultMap，将对应列的属性改为属性名 -->
<!-- 需要指定id,在下面引用时会用到 -->
<resultMap id="primaryStudentMap"                                                 
           type="com.fightersu.entity.PrimaryStudent">                            
    <id column="id" property="stuId"/>                                            
    <result column="name" property="stuName"/>                                    
    <result column="age" property="stuAge"/>                                      
</resultMap>                                                                      
<!-- 在这里也需要指定resultMap，而不是之前的resultType -->           
<select id="selectUseDiffResultMap" resultMap="primaryStudentMap">                
    select id, name, age                                                          
    from su.student                                                               
    where name like '%${stuName}'                                                 
       or age = #{stuAge}                                                         
</select>                                                                         
```



## 模糊搜索

### Mybatis之中的模糊查询

- 将%，\_包含在参数里面传进来
- 在mapper文件里面进行设置

> mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.fightersu.dao.PrimaryStudentDao">
    <select id="selectUseFieldAlias" resultType="com.fightersu.entity.PrimaryStudent">
        select id as stuId, name as stuName, age as stuAge
        from su.student
        where name like #{stuName}
           or age = #{stuAge}
    </select>

    <resultMap id="primaryStudentMap"
               type="com.fightersu.entity.PrimaryStudent">
        <id column="id" property="stuId"/>
        <result column="name" property="stuName"/>
        <result column="age" property="stuAge"/>
    </resultMap>

    <select id="selectUseDiffResultMap" resultMap="primaryStudentMap">
        select id, name, age
        from su.student
        where name like '%${stuName}'
           or age = #{stuAge}
    </select>
</mapper>

```

> 测试类

```java
package com.fightersu;

import com.fightersu.dao.PrimaryStudentDao;
import com.fightersu.entity.PrimaryStudent;
import com.fightersu.utils.MyBatisUtil;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

/**
 * @author fighterSu
 * @date 2021/11/1 12:29
 */
public class TestMyBatis {
    @Test
    public void testSolveWithRename() {
        SqlSession session = MyBatisUtil.getSqlSession();
        if (session != null) {
            PrimaryStudentDao dao = session.getMapper(PrimaryStudentDao.class);
            PrimaryStudent stu = new PrimaryStudent();
            stu.setStuName("张丽");
            stu.setStuAge(21);
            // where name like #{stuName}，使用对象传参
            List<PrimaryStudent> list = dao.selectUseFieldAlias(stu);
            list.forEach(System.out::println);
        }
    }

    @Test
    public void testSolveWithResultMap() {
        SqlSession session = MyBatisUtil.getSqlSession();
        if (session != null) {
            PrimaryStudentDao dao = session.getMapper(PrimaryStudentDao.class);
            PrimaryStudent stu = new PrimaryStudent();
            stu.setStuName("丽");
            stu.setStuAge(19);
            //where name like '%${stuName}'，注意需要使用$而不是#
            List<PrimaryStudent> list = dao.selectUseDiffResultMap(stu);
            list.forEach(System.out::println);
        }
    }
}

```



## 动态SQL

### if标签

**需要注意的是：使用if标签必须在where子句后面，if标签前面1=1或者其他的永真式，因为如果所有if标签全部不满足，那么where子句就会是空，就会报错，我们应当避免**

```xml
<select id="selectStudentIf" resultType="com.fightersu.domain.Student">
    select id,name,email,age from su.student
    where 1=1
    <!-- 使用if标签必须在这里使用1=1或者其他的永真式，因为如果所有if标签全部不满足，那么where子句就会是空，查询不到任何结果 -->
    <if test="name!=null and name!=''">
        and name = #{name}
    </if>
    <if test="age > 0">
        and age &gt; #{age}
    </if>
</select>
```



### where标签

**为了弥补if标签的缺点，使用了where标签，如果里面的if标签全部不满足，那么不会加上where子句**

```xml
<select id="selectStudentWhere" resultType="com.fightersu.domain.Student">
    select id,name,email,age from su.student
    <!--使用where标签，但里面的if标签全部为假时不会添加where子句-->
    <where>
        <if test="name != null and name != ''">
            name like '%${name}'
        </if>
        <if test="age > 0">
            and age &gt;#{age}
        </if>
    </where>
</select>
```



### foreach标签

标签用于实现对于数组与集合的遍历。对其使用，需要注意： 

➢ collection 表示要遍历的集合类型, list ，array 等。 

➢ open、close、separator 为对遍历内容的 SQL 拼接。 语法：  #{item 的值}

语法：  

\<foreach collection="集合类型" open="开始的字符" close="结束的字符" item="集合中的成员" separator="集合成员之间的分隔符">

#{item 的值}

\</foreach>



> 普通数据类型的遍历

```xml
<select id="selectStudentById" resultType="com.fightersu.domain.Student">
    select id,name,email,age from su.student
    <if test="list != null and list.size() != 0">
        where id in
        <!--普通类型直接遍历就好了-->
        <!--实际上foreach标签就是在进行拼接，得到sql语句片段，一般会用在in 后面()，-->
        <!--所以开始和结束分别是左右小括号，中间分隔符为逗号，最后得到结果就是in ( , , , )-->
        <foreach collection="list" open="(" close=")" item="stuId" separator=",">
            #{stuId}
        </foreach>
    </if>
</select>
```

<br>

> 对象类型的遍历

```xml
<select id="selectStudentByList" resultType="com.fightersu.domain.Student">
    select id,name,email,age from su.student
    <if test="list != null and list.size() != 0">
        where id in
        <foreach collection="list" open="(" close=")" item="stuObject" separator=",">
            <!--对象类型的遍历，在 {} 里面取属性 {item.属性}-->
            #{stuObject.id}
        </foreach>
    </if>
</select>
```



### sql标签

**作用：方便代码复用，使代码简洁**

```xml
<!--方便代码复用-->
<!--定义sql片段 id：片段的名称，引用时需要使用-->
<sql id="studentSql">
    select id, name, email, age
    from su.student
</sql>

<select id="selectStudentSqlFragment"
        resultType="com.fightersu.domain.Student">
    <!--引用片段-->
    <include refid="studentSql"/>
    <if test="list != null and list.size() != 0">
        where id in
        <foreach collection="list" open="(" close=")" item="stuObject" separator=",">
            <!--对象类型的遍历，在 {} 里面取属性 {item.属性}-->
            #{stuObject.id}
        </foreach>
    </if>
</select>
```





## 配置文件

### 将数据库连接四要素放到独立的文件

> jdbc.properties

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/su
jdbc.username=root
jdbc.password=4869
```

> config.xml里面的配置

```xml
<properties resource="jdbc.properties"/>
<environments default="mysql">
    <!--id:数据源的名称-->
    <environment id="mysql">
        <!--配置事务类型：使用 JDBC 事务（使用 Connection 的提交和回滚）-->
        <transactionManager type="JDBC"/>
        <!--数据源 dataSource：创建数据库 Connection 对象
        type: POOLED 使用数据库的连接池
        -->
        <dataSource type="POOLED">
            <!--使用 ${key}来引用对应的值-->
            <property name="driver" value="${jdbc.driver}"/>
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
        </dataSource>
    </environment>
</environments>
```



### 给类取别名

```xml
<typeAliases>
    <!--
    定义单个类型的别名
    type:类型的全限定名称
    alias:自定义别名
    -->
    <typeAlias type="com.fightersu.domain.Student" alias="myStudent"/>
    <!--
    批量定义别名，扫描整个包下的类，别名为类名（首字母大写或小写都可以）
    name:包名
    -->
    <package name="com.fightersu.domain"/>
    <!--通过上面两个操作，com.fightersu.domain.Student有了两个别名：myStudent，Student-->
    <!--在对应的mapper文件之中的类似，resultType的值就可以直接使用别名-->
</typeAliases>
```



### 设置mapper文件位置

```xml
<mappers>
    <!--相对于类路径的资源,从 classpath 路径查找文件,即相对（java目录或者resources目录）-->
    <!--告诉 mybatis 要执行的 sql 语句的位置-->
    <!--        <mapper resource="com/fightersu/dao/StudentDao.xml"/>-->
    <!--此种方法要求 Dao 接口名称和 mapper 映射文件名称相同，且在同一个目录中-->
    <package name="com.fightersu.dao"/>
</mappers>
```





## 分页扩展pagehelper

**引用：**

> pom.xml配置引入插件

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.3.0</version>
</dependency>
```

> mybatis配置文件

```xml
<!--在<environments>之前加入-->
<plugins>
 <plugin interceptor="com.github.pagehelper.PageInterceptor" />
</plugins>
```



**在你需要进行分页的 MyBatis 查询方法前调用 PageHelper.startPage 静态方法即可，紧跟在这个方法后的第一个 MyBatis 查询方法会被进行分页。**

```java
@Test
public void testStart() {
    SqlSession session = MyBatisUtil.getSqlSession();
    List<Student> studentList;
    if (session != null) {
        // 每页3条记录，查询第二页
        // PageHelper.startPage(2, 3);
        // 等价于MySQL之中的limit 2,3,前面是其开始坐标，后面是查询记录条数
        PageHelper.offsetPage(2, 3);
        studentList = 
            session.selectList("com.fightersu.dao.StudentDao.selectStudents");
        studentList.forEach(System.out::println);
        session.close();
    } else {
        System.out.println("获取SqlSession失败！");
    }
}
```





























