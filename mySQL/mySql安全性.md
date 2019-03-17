# sql注入

sql注入，简单地说就是执行恶意的sql语句，或者说是构造恶意的sql语句。

**如何进行sql注入**
进行sql注入的方式可谓是千奇百怪了，虽然这也从侧面证明了sql的功能是多么的强大。
常见的恶意sql都是怎么构造的。

 - 1、OR 1 =1
 - 2、利用注释符 mysql中有两种注释符 #  和  -- 
 

``` sql
SELECT * FROM user WHERE username ='user'#'AND password = '111' 
 SELECT * FROM user WHERE username = 'user'-- 'AND password = '111' 
```

 这样一注释，后面的语句就失效了。相当于执行了： 
 

``` sql
SELECT * FROM user WHERE username = 'user' 
```

 达到巧妙避开密码验证的效果
 

 - 3、利用union

union会把结果拼拼到一起，所有要让union前面的查询返回一个空值，一般采用类似于id=-1的方式。
UNION 需要两个被select的集合拥有相同的列数。

``` sql
// 获取数据库名列表  
select name from students where id = -1 union select schema_name from information_schema.schemata;
```

**如何防范sql注入？**

 - 不要信任用户的输入，对用户的输入进行校验。如用mysqli_real_escape_string()函数
 - 开发时尽量用安全函数代替不安全函数，编写安全代码。危险函数，常见的执行命令函数，动态访问函数，如C语言中的system(),PHP的eval()，JSP的include()导致的代码越权执行，都是注入。
 - 不要使用动态拼装sql，使用参数化的sql。预编译语句，绑定变量。使用预编译的SQL语句，SQL的语意不会变化，攻击者无法改变SQL的结构
 - 限制mysql客户端用户的权限
 - 不要把机密信息直接存放，加密或者hash掉密码和敏感的信息。
 - 应用的异常信息，对外应该给出尽可能少的提示。