---

---

# JEECG平台权限设计

> # 一、权限的设计概念

JEECG封装了完善的用户权限模块，支持菜单权限，列表权限，表单权限，数据权限。数据权限功能已实现极致： 支持行级、列级、字段级控制，实现不同人看不同数据，不同人对同一个页面操作不同字段。系统按钮权限和表单权限原来是正控制，只有授权的人才有权限，未授权看不到对应按钮;（admin拥有超级权限，不受控制）

用户权限模块：指用户信息及用户权限信息数据。一个用户可以分配多个角色；也可以隶属于多个组织结构；多个组织机构情况下，登录需要选择机构登录，方便数据权限控制。多个组织登录情况如下：

![1](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/1.png)

管理员可以对用户进行管理，如果用户已经失效可以直接冻结该用户禁止其登录，新开的用户需要激活后才能登录

![2](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/2.png)



> # 二、权限的对象

JEECG权限设计，采用用户、角色、菜单、组织机构来进行组建，角色为主要授权对象，组织机构可以分配角色。 菜单类型分两种：权限类型和菜单类型

- 菜单类型： 菜单类型的菜单，在首页菜单展示和访问使用
- 权限类型： 权限类型的菜单，在首页菜单不做展示，只做权限控制使用

### 1.用户管理

用户管理，用户可以分配多个角色；也可以隶属于多个组织机构，多个组织机构情况下，登录需要选择机构登录，方便数据权限控制；

![3](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/3.png)



### 2.角色管理

角色是权限组单位，通过角色管理菜单权限。

![4](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/4.png)



### 3.菜单管理

菜单管理，用来做首页菜单管理和权限管理，权限包括：菜单访问权限、按钮权限、表单权限、数据权限。

![5](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/5.png)



### 4.组织机构管理

组织机构管理，支持集团模式多个分公司，第一级默认是公司类型，从二级开始可设置部门和岗位，部门和岗位通过类型区分；组织机构可以单独设置角色；

![6](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/6.png)



> # 三、数据权限规则

### 一、功能说明

列表数据权限，主要通过数据权限控制行数据，让不同的人有不同的查看数据规则； 比如： 销售人员只能看自己的数据；销售经理可以看所有下级销售人员的数据；财务只看金额大于5000的数据等等；   

### 二、数据权限分两大类型

| 序号 | 类型       | 规则字段区别                                | 说明                               |
| ---- | ---------- | ------------------------------------------- | ---------------------------------- |
| 1    | 编码方式   | 规则字段是驼峰写法，对应hibernate实体的字段 | 编码模式（通过代码生成器生成代码） |
| 2    | Online方式 | 规则字段是下划线写法，对应表的字段          | Online模式（在线表单模式，无代码） |

### 三、数据权限规则篇

#### 1.当前用户上下文变量

注意：数据权限配置，规则值可以填写系统上下文变量（当前登录人信息），从而根据当前登录人信息进行权限控制。

| 编码             | 描述                 |
| ---------------- | -------------------- |
| sys_user_code    | 当前登录用户登录账号 |
| sys_user_name    | 当前登录用户真实名称 |
| sys_date         | 当前系统日期         |
| sys_time         | 当前系统时间         |
| sys_company_code | 当前登录用户公司编号 |
| sys_org_code     | 当前登录用户部门编号 |

规则值，配置写法如下：#{sys_user_code}   



> # 四、如何生成代码

> > #### 首先登录网站===>在线开发===>Online表单开发===>创建表单
> >
> > ![7](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/7.png)

> > 创建表单后需要先同步到数据库，然后点击<u>**代码生成**</u> 。可以根据弹出的对话框选择代码生成的存放位置。然后copy到项目中。
> >
> > 另外一种生成方案是使用Java中的GUI生成代码。『官方建议使用Online生成』。直接run As test包下的JeecgOneGUI.java 文件然后弹出录入界面
> >
> > ![8](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/8.png)
>
> 如果是基础功能不建议选择生成代码。直接在线生成就可以，如果有需要做开发API接口给前台使用的则需要生成代码放到项目中以便增加注解【API采用Swagger】，只有在方法上加入`@ApiOperation`注解才能在Swagger中显示。

> # 五、功能测试

创建表单后并且同步到数据库后可以在表单后面看到一个***功能测试***按钮。点击功能测试按钮看到的界面就是我们需要的生成的界面，创建表的时候风格一般都是默认不做选择

![9](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/9.png)

![10](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/10.png)

> # 六、具体的数据权限控制

> > #### 1.按钮权限规则控制
> >
> > 列表按钮权限控制，主要是控制列表上按钮、操作链接的隐藏；按钮权限配置后，默认未授权用户都看不到，具体配置在菜单中配置如图
> >
> > ![11](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/11.png)
> >
> > 页面控件编码`operationCode`：
> >
> > ```
> > <t:dgToolBar title="填报日志" icon="icon-add" url="tBussDailyLogController.do?goAdd" operationCode="#db_generate_input" funname="add"></t:dgToolBar>
> > 	<t:dgToolBar title="日志点评" icon="icon-edit" url="tBussDailyLogController.do?goUpdate" operationCode="#db_generate_form" funname="update"></t:dgToolBar>
> >    <t:dgToolBar title="查看" icon="icon-search" url="tBussDailyLogController.do?goUpdate" funname="detail"></t:dgToolBar>
> > ```
> >
> > 页面控件编码可以自定义，把定义好的编码填写到 【菜单栏目的】-【页面控件】
> >
> > 填写完毕后点击【角色管理】可以根据角色来选择是否隐藏按钮【隐藏不大勾，打勾不隐藏】
> >
> > ![12](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/12.png)
> >
> >



> > #### 2.列表数据权限用法
> >
> > ##### 数据权限菜单的配置
> >
> > 创建数据权限类型菜单（注意：列表访问链接和数据请求链接不是一个，此为加载数据请求）
> > **注意**： 这里很容易配置错误，一定注意【datagrid针对列表数据控制？后面要跟datagrid】
> > 用户数据请求地址：userController.do?datagrid
> >
> > ![13](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/13.png)
> >
> > ##### 列数据权限控制
> >
> > 如果想隐藏某一列的值，不想让某类用户看到该列的值可以使用如下方法
> >
> > 请看操作步骤
> >
> > ![15](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/15.png)
> >
> > 然后给角色分配权限
> >
> > ![16](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/16.png)
> >
> > 使用角色为 经理的用户进行登录看到的效果如下：
> >
> > ![17](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/17.png)
> >
> > 然后给经理角色【页面权限规则】 工作日志字段隐藏 打勾
> >
> > ![18](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/18.png)
> >
> > 打勾完毕后，再次使用角色为 经理的用户进行登录看到的效果如下：
> >
> > ![19](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/19.png)
> >
> > 如果只想让登录用户只能看到自己的数据可以进行如下操作
> >
> > 首先需要在菜单栏目中进行设置
> >
> > ![20](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/20.png)
> >
> > 设置完毕后进入角色管理，给角色分配权限
> >
> > ![21](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/21.png)
> >
> > 使用员工角色登录然后查看显示效果如下
> >
> > ![22](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/22.png)
> >
> > 再次给员工角色分配权限，打上勾
> >
> > ![23](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/23.png)
> >
> > 然后再次使用员工角色登录然后查看显示效果如下
> >
> > ![24](https://github.com/PlayTaoist/jeecg-lession/blob/master/%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86-lession/images/24.png)
> >
> >

