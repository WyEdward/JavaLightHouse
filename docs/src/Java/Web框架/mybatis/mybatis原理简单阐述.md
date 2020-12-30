#### mybatis其实就是对jdbc的整合简化
##### JDBC的步骤
1、注册驱动
2、获取数据库连接对象
3、定义sql语句
4、加载预处理对象
5、获取结果集 将结果集封装在某个对象
6、遍历结果集
7、释放资源


#### mybatis   
利用构建者模式创建工厂
利用工厂模式创建session
利用代理模式 找到接口的代理对象

**主配置文件干了什么？**
1、数据库的注册信息
2、相关mapper的映射

**mapper的映射文件干了什么？**
namespace表明该映射文件对应的接口全限定类名 
id 对应方法名 
sql语句 
resulttype 返回值类型

所以mybatis其实就是把namespace+id作为key  sql+resultype作为value

**测试类干了什么？**
利用xml解析工具 解析了mybatis主配置文件
代理对象.方法  其实记录了 这个接口的全限定类名+方法名
然后找到这个 接口的全限定类名+方法名(key) 对应的value
然后获取预处理对象 封装结果集 

![mybatis原理](https://gitee.com/WyEdward/images/raw/master/img/mybatis原理.png)