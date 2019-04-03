1.用户登录后，建立一个Session会话，保存重要信息，

``` java
@Controller
public class LoginController {
    @PostMapping(value = "/user/login")
    public String login(@RequestParam("username") String username,
                        @RequestParam("password") String password,
                        Map<String,Object> map,
                        HttpSession session) {
        if (!StringUtils.isEmpty(username) && "123456".equals(password)) {
            //防止表单重复提交，重定向
            session.setAttribute("loginUser",username);
            return "redirect:/home";
        } else {
            map.put("msg","用户名密码错误");
            return "index";
        }
    }
}
```

2.数据前后端传输
**后端到前端：**
![后端](./images/1553673655737.png)
![前端](./images/1553673730881.png)
**前端到后端：**
首先表单action标识表单交给谁负责：
![enter description here](./images/1553673920239.png)

``` java
//来到员工添加页面
    @GetMapping("/emp")
    public String toAddPage(Model model){
        //来到添加页面,查出所有的部门，在页面显示
        Collection<Department> departments = departmentDao.getDepartments();
        model.addAttribute("depts",departments);
        return "emp/add";
    }

    //员工添加
    //SpringMVC自动将请求参数和入参对象的属性进行一一绑定；要求请求参数的名字和javaBean入参的对象里面的属性名是一样的
    @PostMapping("/emp")
    public String addEmp(Employee employee){
        //来到员工列表页面

        System.out.println("保存的员工信息："+employee);
        //保存员工
        employeeDao.save(employee);
        // redirect: 表示重定向到一个地址  /代表当前项目路径
        // forward: 表示转发到一个地址
        return "redirect:/emps";
    }
```html
<form th:action="@{/emp}" method="post">
						<!--发送put请求修改员工数据-->
						<!--
						1、SpringMVC中配置HiddenHttpMethodFilter;（SpringBoot自动配置好的）
						2、页面创建一个post表单
						3、创建一个input项，name="_method";值就是我们指定的请求方式
						-->
						<input type="hidden" name="_method" value="put" th:if="${emp!=null}"/>
						<input type="hidden" name="id" th:if="${emp!=null}" th:value="${emp.id}">
						<div class="form-group">
							<label>LastName</label>
							<input name="lastName" type="text" class="form-control" placeholder="zhangsan" th:value="${emp!=null}?${emp.lastName}">
						</div>
						<div class="form-group">
							<label>Email</label>
							<input name="email" type="email" class="form-control" placeholder="zhangsan@atguigu.com" th:value="${emp!=null}?${emp.email}">
						</div>
						<div class="form-group">
							<label>Gender</label><br/>
							<div class="form-check form-check-inline">
								<input class="form-check-input" type="radio" name="gender" value="1" th:checked="${emp!=null}?${emp.gender==1}">
								<label class="form-check-label">男</label>
							</div>
							<div class="form-check form-check-inline">
								<input class="form-check-input" type="radio" name="gender" value="0" th:checked="${emp!=null}?${emp.gender==0}">
								<label class="form-check-label">女</label>
							</div>
						</div>
						<div class="form-group">
							<label>department</label>
							<!--提交的是部门的id-->
							<select class="form-control" name="department.id">
								<option th:selected="${emp!=null}?${dept.id == emp.department.id}" th:value="${dept.id}" th:each="dept:${depts}" th:text="${dept.departmentName}">1</option>
							</select>
						</div>
						<div class="form-group">
							<label>Birth</label>
							<input name="birth" type="text" class="form-control" placeholder="zhangsan" th:value="${emp!=null}?${#dates.format(emp.birth, 'yyyy-MM-dd HH:mm')}">
						</div>
						<button type="submit" class="btn btn-primary" th:text="${emp!=null}?'修改':'添加'">添加</button>
					</form>
									

**name属性必须和实体类属性字段相同，才会自动封装：**
```



**3.修改**
![enter description here](./images/TIM截图20190328171629.png)
![enter description here](./images/1553764732169.png)

``` java
//来到修改页面，查出当前员工，在页面回显
    @GetMapping("/emp/{id}")
    public String toEditPage(@PathVariable("id") Integer id,Model model){
        Employee employee = employeeDao.get(id);
        model.addAttribute("emp",employee);

        //页面要显示所有的部门列表
        Collection<Department> departments = departmentDao.getDepartments();
        model.addAttribute("depts",departments);
        //回到修改页面(add是一个修改添加二合一的页面);
        return "emp/add";
    }

    //员工修改；需要提交员工id；
    @PutMapping("/emp")
    public String updateEmployee(Employee employee){
        System.out.println("修改的员工数据："+employee);
        employeeDao.save(employee);
        return "redirect:/emps";
    }
```
**4.删除**

![enter description here](./images/1553782482738.png)
![enter description here](./images/1553782646270.png)