## Web开发模式【Mode I 和Mode II的介绍、应用案例】

原创 Java3y [Java3y](javascript:void(0);) *2018-02-18*





# 开发模式的介绍

**在Web开发模式中，有两个主要的开发结构，称为模式一（Mode I）和模式二（Mode II）.**

**首先我们来理清一些概念吧：**

- **DAO(Data Access Object)：主要对数据的操作，增加、修改、删除等原子性操作。**
- **Web层：界面+控制器，也就是说JSP【界面】+Servlet【控制器】**
- **Service业务层：将多个原子性的DAO操作进行组合，组合成一个完整的业务逻辑**
- **控制层：主要使用Servlet进行控制**
- **数据访问层：使用DAO、Hibernate、JDBC技术实现对数据的增删改查**
- **JavaBean用于封装数据，处理部分核心逻辑，每一层中都用到！**

# 模式一

模式一指的就是**在开发中将显示层、控制层、数据层的操作统一交给JSP或者JavaBean来进行处理**！

**模式一有两种情况：**

- **完全使用JSP做开发**

- - 优点：
    1. **开发速度贼快，只要写JSP就行了，JavaBean和Servlet都不用设计！**
    2. **小幅度修改代码方便，直接修改JSP页面交给WEB容器就行了，不像Servlet还要编译成.class文件再交给服务器！【当然了，在ide下开发这个也不算是事】**
  - 缺点：

- 1. **程序的可读性差、复用性低、代码复杂！什么jsp代码、html代码都往上面写，这肯定很难阅读，很难重用！**

- **使用JSP+JavaBean做开发**

- - 优点：
    1. **程序的可读性较高，大部分的代码都写在JavaBean上，不会和HTML代码混合在一起，可读性还行的**。
    2. **可重复利用高，核心的代码都由JavaBean开发了，JavaBean的设计就是用来重用、封装，大大减少编写重复代码的工作！**
  - 缺点：

- 1. **没有流程控制，程序中的JSP页面都需要检查请求的参数是否正确，异常发生时的处理。显示操作和业务逻辑代码工作会紧密耦合在一起的！日后维护会困难**

## 应用例子：

**我们使用JavaBean+JSP开发一个简易的计算器吧，效果如图下**：

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib12LjwAzLDiaWS0z7DVGCLx0x6276AYjEPXkgFPP6yYulhsBf9BYE7DRp48P1fVH3SaxPxoTbQehxA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib12LjwAzLDiaWS0z7DVGCLx0x4Tzpw47xQaJopibMQ28uZm1RGAWicNK31UHdkMRZukQC61X0b1jDt8w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 首先开发JavaBean对象

```
    public class Calculator {
    
        private double firstNum;
        private double secondNum;
        private char Operator = '+';
        private double result;
    
    
        //JavaBean提供了计算的功能
        public void calculate() {
    
            switch (this.Operator) {
                case '+':
                    this.result = this.firstNum + this.secondNum;
                    break;
    
                case '-':
                    this.result = this.firstNum - this.secondNum;
                    break;
    
                case '*':
                    this.result = this.firstNum * this.secondNum;
                    break;
    
                case '/':
                    if (this.secondNum == 0) {
                        throw new RuntimeException("除数不能为0");
                    }
                    this.result = this.firstNum / this.secondNum;
                    break;
    
                default:
                    throw new RuntimeException("传入的字符非法！");
            }
        }
    
    
        public double getFirstNum() {
            return firstNum;
        }
    
        public void setFirstNum(double firstNum) {
            this.firstNum = firstNum;
        }
    
        public double getSecondNum() {
            return secondNum;
        }
    
        public void setSecondNum(double secondNum) {
            this.secondNum = secondNum;
        }
    
        public char getOperator() {
            return Operator;
        }
    
        public void setOperator(char operator) {
            Operator = operator;
        }
    
        public double getResult() {
            return result;
        }
    
        public void setResult(double result) {
            this.result = result;
        }
    }
```

- 再开发显示页面

```
    <%--开发用户界面--%>
    <form action="/zhongfucheng/1.jsp" method="post">
        <table border="1">
            <tr>
                <td colspan="2">简单计数器</td>
                <td></td>
            </tr>
            <tr>
                <td>第一个参数：</td>
                <td><input type="text" name="firstNum"></td>
            </tr>
            <tr>
                <td>运算符</td>
                <td>
                    <select name="operator">
                        <option value="+">+</option>
                        <option value="-">-</option>
                        <option value="*">*</option>
                        <option value="/">/</option>
                    </select>
                </td>
            </tr>
            <tr>
                <td>第二个参数：</td>
                <td><input type="text " name="secondNum"></td>
            </tr>
            <tr>
                <td colspan="2"><input type="submit" value="提交"></td>
                <td></td>
            </tr>
        </table>
    </form>
```

- 效果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib12LjwAzLDiaWS0z7DVGCLx0N4BdISgFhaJsUPMxf3mm1ZSrtYaC965u5iaR2w0bbEx1Wo1xJzjNDzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- **获取得到显示页面提交的参数，调用JavaBean的方法，最后输出结果！**

```
    <%--获取得到Bean对象--%>
    <jsp:useBean id="calculator" class="domain.Calculator" scope="page"/>
    
    <%--设置Bean对象的数据--%>
    <jsp:setProperty name="calculator" property="*"/>
    
    <%--调用Caculator的方法计算出值--%>
    <jsp:scriptlet>
        calculator.calculate();
    </jsp:scriptlet>
    
    
    <%--得出的结果：--%>
    
    <c:out value="计算得出的结果是："/>
    <jsp:getProperty name="calculator" property="firstNum"/>
    <jsp:getProperty name="calculator" property="operator"/>
    <jsp:getProperty name="calculator" property="secondNum"/>
    <c:out value="="/>
    <jsp:getProperty name="calculator" property="result"/>
```

- 效果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib12LjwAzLDiaWS0z7DVGCLx0R9BukpSibNvPuED9OHBgcWicChic9ZYicQEp47iaEQkTyicibbEGQibTUnUQFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

------

**开发这个简易的计算器，只用了一个JSP页面和一个JavaBean完成！**

**总的来说，Mode I 适合小型的开发，复杂程序低的开发，因为Mode I 的特点就是开发速度快，但在进行维护的时候就要付出更大的代价！**

------

# 模式二

**Mode II 中所有的开发都是以Servlet为主体展开的，由Servlet接收所有的客户端请求，然后根据请求调用相对应的JavaBean，并所有的显示结果交给JSP完成！，也就是俗称的MVC设计模式！**

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib12LjwAzLDiaWS0z7DVGCLx0bhvibC9akDkXTVja4umibAAyIty8mIbEibYtpaycibjfhiaDOMMoIy6L91w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**MVC设计模式：**

- **显示层（View）：主要负责接受Servlet传递的内容，调用JavaBean，将内容显示给用户**
- **控制层（Controller）：主要负责所有用户的请求参数，判断请求参数是否合法，根据请求的类型调用JavaBean，将最终的处理结果交给显示层显示！**
- **模型层（Mode）：模型层包括了业务层，DAO层。**

## 应用例子：

我们使用MVC模式开发一个简单的用户登陆注册的案例吧！作为一个简单的用户登陆注册，这里就**直接使用XML文档当作小型数据库吧**！

### **①搭建开发环境**

- **导入相对应的开发包**

  

  

- **创建程序的包名**

- **创建xml文件，当做小型的数据库**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### **②开发实体User**

```
    private int id;
    private String username;
    private String password;
    private String email;
    private Date birthday;

    //....各种setter、getter
```

### **③开发dao**

- 这个根据业务来开发，我们是登陆注册，那应该提供什么功能呢？**注册（外界传递一个User对象进来，我可以在XML文档多一条信息）。登陆（外界传递用户名和密码过来，我就在XML文档中查找有没该用户名和密码，如果有就返回一个User对象）**
- **3.1登陆功能**：

```
    //外界传递用户名和密码进来，我要在XML文档中查找是否有该条记录
    public User find(String username, String password) {

        //得到XML文档的流对象
        InputStream inputStream = UserImplXML.class.getClassLoader().getResourceAsStream("user.xml");

        //得到dom4j的解析器对象
        SAXReader saxReader = new SAXReader();


        try {

            //解析XML文档
            Document document = saxReader.read(path);

            //使用XPATH技术，查找XML文档中是否有传递进来的username和password
            Element element = (Element) document.selectSingleNode("//user[@username='" + username + "' and@password='" + password + "']");

            if (element == null) {
                return null;
            }

            //如果有，就把XML查出来的节点信息封装到User对象，返回出去
            User user = new User();
            user.setId(Integer.parseInt(element.attributeValue("id")));
            user.setUsername(element.attributeValue("username"));
            user.setPassword(element.attributeValue("password"));
            user.setEmail(element.attributeValue("email"));

            //生日就需要转换一下了，XML文档保存的是字符串，User对象需要的是Date类型
            SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd");
            Date birthday = simpleDateFormat.parse(element.attributeValue("birthday"));
            user.setBirthday(birthday);

            //返回User对象出去
            return user;

        } catch (DocumentException e) {
            e.printStackTrace();
            throw new RuntimeException("初始化时候出错啦！");
        } catch (ParseException e) {
            e.printStackTrace();
            throw new RuntimeException("查询的时候出错啦！");
        }

    }
```

- **做完一个功能，最好就测试一下，看有没有错误再继续往下写！**

```
    private String username = "zhongfucheng";
    private String password = "123";

    @Test
    public void testLogin() {

        UserImplXML userImplXML = new UserImplXML();
        User user = userImplXML.find(username, password);

        System.out.println(user.getBirthday());
        System.out.println(user.getEmail());
        System.out.println(user.getId());
        System.out.println(user.getUsername());
        System.out.println(user.getPassword());


    }
```

- 效果：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

**3.2注册功能**

```
    //注册功能，外界传递一个User对象进来。我就在XML文档中添加一条信息
    public void register(User user) {

    //获取XML文档路径！
    String path = UserImplXML.class.getClassLoader().getResource("user.xml").getPath();
        

        try {
            //获取dom4j的解析器，解析XML文档
            SAXReader saxReader = new SAXReader();
            Document document = saxReader.read(path);

            //在XML文档中创建新的节点
            Element newElement = DocumentHelper.createElement("user");
            newElement.addAttribute("id", String.valueOf(user.getId()));
            newElement.addAttribute("username", user.getUsername());
            newElement.addAttribute("email", user.getEmail());
            newElement.addAttribute("password", user.getPassword());

            //日期返回的是指定格式的日期
            SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd");
            String date = simpleDateFormat.format(user.getBirthday());
            newElement.addAttribute("birthday",date);

            //把新创建的节点增加到父节点上
            document.getRootElement().add(newElement);

            //把XML内容中文档的内容写到硬盘文件上
            OutputFormat outputFormat = OutputFormat.createPrettyPrint();
            outputFormat.setEncoding("UTF-8");
            XMLWriter xmlWriter = new XMLWriter(new FileWriter(path),outputFormat);
            xmlWriter.write(document);
            xmlWriter.close();

        } catch (DocumentException e) {
            e.printStackTrace();
            throw new RuntimeException("注册的时候出错了！！！");
        } catch (IOException e) {
            e.printStackTrace();
            throw new RuntimeException("注册的时候出错了！！！");
        }
    }
```

- 我们也测试一下有没有错误！

```
    @Test
    public void testRegister() {

        UserImplXML userImplXML = new UserImplXML();

        //这里我为了测试的方便，就添加一个带5个参数的构造函数了！
        User user = new User(10, "nihao", "123", "sina@qq.com", new Date());

        userImplXML.register(user);
    }
```

- 注意！**测试的结果是在classes目录下的user.xml文件查询的**！因为**我们是用Test来测试代码，读取XML文件时使用的是类装载器的方法，在编译后，按照WEB的结构目录，XML文件的读写是在WEB-INF的classes目录下的！**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

- **DAO的实现已经开发完成了，接下来我们就对DAO的实现进行抽取。【当然了，也可以先写DAO再写DAO的实现】**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

### **④开发service层**

service层的开发就非常简单了！上面已经说了，service层就是：**将多个原子性的DAO操作进行组合，组合成一个完整的业务逻辑**。简单来说：**对web层提供所有的业务服务的**！

**在逻辑代码不是非常复杂的情况下，我们可以没有service层的**，这里还是演示一下吧！

```
    public class UserServiceXML {
    
        //Service层就是调用Dao层的方法，我们就直接在类中创建Dao层的对象了
        UserDao userImplXML = new UserImplXML();
    
        public void register(User user) {
            userImplXML.register(user);
        }
    
        public void login(String username, String password) {
    
            userImplXML.find(username, password);
        }
    }
```

- 当然了，**为了更好的解耦，也把它抽取成接口**！

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

### **⑤开发web层**

#### 5.1我们来先做注册的界面吧！

- **提供注册界面的Servlet**

```
    public class RegisterUIServlet extends javax.servlet.http.HttpServlet {
        protected void doPost(javax.servlet.http.HttpServletRequest request, javax.servlet.http.HttpServletResponse response) throws javax.servlet.ServletException, IOException {
    
            //直接跳转到显示注册界面的JSP
            request.getRequestDispatcher("/WEB-INF/register.jsp").forward(request, response);
    
        }
    
        protected void doGet(javax.servlet.http.HttpServletRequest request, javax.servlet.http.HttpServletResponse response) throws javax.servlet.ServletException, IOException {
    
            this.doPost(request, response);
        }
    }
```

- **开发注册界面的JSP**

```
<h1>欢迎来到注册界面！</h1>

<%--提交给处理注册的处理Servlet--%>

<form method="post" action="${pageContext.request.contextPath}/RegisterServlet">

    <table>
        <%--对于id来讲，是服务器分配的！不需要用户自己输入--%>
        <tr>
            <td>用户名</td>
            <td>
                <input type="text " name="username">
            </td>
        </tr>
        <tr>
            <td>密码</td>
            <td>
                <input type="text" name="password">
            </td>
        </tr>
        <tr>
            <td>确认密码</td>
            <td>
                <input type="text" name="password">
            </td>
        </tr>
        <tr>
            <td>邮箱</td>
            <td>
                <input type="text" name="email">
            </td>
        </tr>
        <tr>
            <td>生日</td>
            <td>
                <input type="text " name="birethday">
            </td>
        </tr>
        <tr>
            <td>
                <input type="submit" value="提交">
            </td>
            <td>
                <input type="reset" value="重置！">
            </td>
        </tr>
    </table>
</form>
```

- JSP页面是这样子的

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 接下来，**我们要开发处理用户注册提交的Servlet**

```
        //首先要接受Parameter的参数，封装到User里面去
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        //......如果参数过多，我们就要写好多好多类似的代码了...
```

- 此时，我们应该想起**反射机制中的BeanUtils开发包..为了更好地重用，我就将它写成一个工具类**！

```
    /*
    * 将Parameter参数的数据封装到Bean中，为了外边不用强转，这里就使用泛型了！
    *
    * @request   由于要获取的是Parameter参数的信息，所以需要有request对象
    * @tClass    本身是不知道封装什么对象的，所以用class
    *
    * */

    public static <T> T request2Bean(HttpServletRequest httpServletRequest, Class<T> tClass) {

        try {

            //创建tClass的对象
            T bean = tClass.newInstance();

            //获取得到Parameter中全部的参数的名字
            Enumeration enumeration = httpServletRequest.getParameterNames();

            //遍历上边获取得到的集合
            while (enumeration.hasMoreElements()) {

                //获取得到每一个带过来参数的名字
                String name = (String) enumeration.nextElement();

                //获取得到值
                String value = httpServletRequest.getParameter(name);

                //把数据封装到Bean对象中
                BeanUtils.setProperty(bean, name, value);
            }
            return bean;
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException("封装数据到Bean对象中出错了！");
        }
    }
```

- 经过我们测试，**日期不能直接封装到Bean对象中，会直接报出异常**！

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 对于日期而言，需要一个日期转换器。**当BeanUtils的setProperty()方法检测到日期时，会自动调用日期转换器对日期进行转换，从而实现封装！**
- 于是乎，就在上面的方法中添加以下一句代码

```
            //日期转换器
            ConvertUtils.register(new DateLocaleConverter(), Date.class);
```

------

- 还有一个问题，**用户的id不是自己输入的，是由程序生成的。我们避免id的重复，就使用UUID生成用户的id吧！**为了更好的重用，我们也把它封装成一个方法！

```
    /*生成ID*/
    public static int makeId() {
        return Integer.parseInt(UUID.randomUUID().toString());
    }
```

- 好的，我们来测试一下吧！**以下是RegisterServlet的代码**

```
        User user = WebUtils.request2Bean(request, User.class);
        user.setId(WebUtils.makeId());

        //调用service层的注册方法，实现注册
        ServiceBussiness serviceBussiness = new UserServiceXML();
        serviceBussiness.register(user);
```

- 效果：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

**上面的代码是不够完善的（没有校验用户输入的信息、注册成功或失败都没有给出提示..等等）**

- 下面，**我们来校验用户输入的信息吧，如果用户输入的信息不合法，就直接跳转回注册的界面**。
- 刚才我们是用BeanUtils把Parameter的信息全部直接封装到User对象中，但**现在我想要验证用户提交表单的数据，也应该把表单的数据用一个对象保存着【面向对象的思想、封装、重用】**
- 流程是这样子的：**当用户提交表单数据的时候，就把表单数据封装到我们设计的表单对象上，调用表单对象的方法，验证数据是否合法**！
- 好了，**我们来开发一个表单的对象吧，最重要的是怎么填写validate()方法！**！

```
public class FormBean {

    //表单提交过来的数据全都是String类型的，birthday也不例外！
    private String username;
    private String password;
    private String password2;
    private String email;
    private String birthday;
    
    /*用于判断表单提交过来的数据是否合法*/
    public boolean validate() {
        
        return false;
        
    }
    
    //......各种setter、getter方法
}
```

- 以下是我定下的规则：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 方法的代码如下：

```
    public boolean validate() {

        //用户名不能为空，并且要是3-8的字符 abcdABcd
        if (this.username == null || this.username.trim().equals("")) {
            return false;

        } else {
            if (!this.username.matches("[a-zA-Z]{3,8}")) {
                return false;
            }
        }

        //密码不能为空，并且要是3-8的数字
        if (this.password == null || this.password.trim().equals("")) {
            return false;
        } else {
            if (!this.password.matches("\\d{3,8}")) {
                return false;
            }
        }

        //两次密码要一致
        if (this.password2 != null && !this.password2.trim().equals("")) {
            if (!this.password2.equals(this.password)) {
                return false;
            }
        }

        //邮箱可以为空，如果为空就必须合法
            if (this.email != null && !this.email.trim().equals("")) {
                if (!this.email.matches("\\w+@\\w+(\\.\\w+)+")) {

                    System.out.println("邮箱错误了！");
                    return false;
                }
        }

        //日期可以为空，如果为空就必须合法
        if (this.birthday != null && !this.birthday.trim().equals("")) {

            try {
                DateLocaleConverter dateLocaleConverter = new DateLocaleConverter();
                dateLocaleConverter.convert(this.birthday);
            } catch (Exception e) {

                System.out.println("日期错误了！");
                return false;
            }
        }

        
        //如果上面都没有执行，那么就是合法的了，返回true
        return true;
    }
```

- **处理表单数据的Servlet，代码是这样子的**：

```
        //将表单的数据封装到formBean中
        FormBean formBean = WebUtils.request2Bean(request, FormBean.class);

        //验证表单的数据是否合法，如果不合法就跳转回去注册的页面
        if(formBean.validate()==false){
            request.getRequestDispatcher("/WEB-INF/register.jsp").forward(request, response);
            return;
        }
        try {
            
            //将表单的数据封装到User对象中
            User user = WebUtils.request2Bean(request, User.class);
            user.setId(WebUtils.makeId());

            //调用service层的注册方法，实现注册
            ServiceBussiness serviceBussiness = new UserServiceXML();
            serviceBussiness.register(user);

        } catch (Exception e) {
            e.printStackTrace();
        }
```

- 接下来我们测试一下吧！**将所有的信息都按照规定的输入！**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 没有问题！已经将记录写到XML文件上了！

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 但是，**如果我没有输入日期呢**？

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

它抛出了错误！原因也非常简单：**表单数据提交给Servlet，Servlet将表单的数据（Parameter中的数据）用BeanUtils封装到User对象中，当封装到日期的时候，发现日期为null，无法转换成日期对象！**

那我们现在要怎么解决呢？

首先我们要明确：因为**我们在设定的时候，已经允许了email和birthday可以为空，那么在DAO层就应该有相应的逻辑判断email和birthday是否为空**！

```
            if (user.getEmail() == null) {
                newElement.addAttribute("email", "");
            } else {
                newElement.addAttribute("email", user.getEmail());

            }

            //如果不是空才格式化信息
            if (user.getBirthday() != null) {

                //日期返回的是指定格式的日期
                SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
                String date = simpleDateFormat.format(user.getBirthday());
                
                newElement.addAttribute("birthday", date);
            } else {
                newElement.addAttribute("birthday", "");
            }
```

**解决办法：**

- **Parameter中的数据如果是"",我就不把数据封装到User对象中,执行下一次循环！**

```
    public static <T> T request2Bean(HttpServletRequest httpServletRequest, Class<T> tClass) {

        try {

            //创建tClass的对象
            T bean = tClass.newInstance();

            //获取得到Parameter中全部的参数的名字
            Enumeration enumeration = httpServletRequest.getParameterNames();

            //日期转换器
            ConvertUtils.register(new DateLocaleConverter(), Date.class);

            //遍历上边获取得到的集合
            while (enumeration.hasMoreElements()) {

                //获取得到每一个带过来参数的名字
                String name = (String) enumeration.nextElement();

                //获取得到值
                String value = httpServletRequest.getParameter(name);

                //如果Parameter中的数据为""，那么我就不封装到User对象里边去！执行下一次循环
                if (value == "") {
                    continue;
                } else {
                    //把数据封装到Bean对象中
                    BeanUtils.setProperty(bean, name, value);
                }

            }
            return bean;
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException("封装数据到Bean对象中出错了！");
        }
    }
```

- 效果：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**将数据封装到User对象中还有另外一个办法：**

- 我们知道BeanUtils有个copyProperties()方法，**可以将某个对象的成员数据拷贝到另外一个对象的成员变量数据上（前提是成员变量的名称相同！）**我们**FormBean对象的成员变量名称和User对象的成员变量的名称是一致的**！并且，前面在验证的时候，**我们已经把Parameter中带过来的数据封装到了FormBean对象中了，所以我们可以使用copyProperties()方法！**
- 使用该方法时，值得注意的是：**第一个参数是拷贝到哪一个对象上（也就是User对象），第二个参数是被拷贝的对象（也就是formbean对象）,口诀：后拷前....不要搞混了！！！！！(我就是搞混了，弄了很久...)**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

- **处理表单的Servlet完整代码如下**：

```
        //将表单的数据封装到formBean中
        FormBean formBean = WebUtils.request2Bean(request, FormBean.class);

        //验证表单的数据是否合法，如果不合法就跳转回去注册的页面
        if(formBean.validate()==false){
            request.getRequestDispatcher("/WEB-INF/register.jsp").forward(request, response);
            return;
        }
        try {

            //这是第一种--------------------------
          /*User user = new User();
            user.setId(WebUtils.makeId());
            BeanUtils.copyProperties(user,formBean);*/
            //------------------------------------------

            //这是第二种
            User user1 = WebUtils.request2Bean(request,User.class);
            user1.setId(WebUtils.makeId());
            //-----------------------------------


            //调用service层的注册方法，实现注册
            ServiceBussiness serviceBussiness = new UserServiceXML();
            serviceBussiness.register(user1);

        } catch (Exception e) {
            e.printStackTrace();
        }
```

------

现在还有问题，**如果我填写信息不合法，提交给服务器验证以后，服务器应该告诉用户哪个信息不合法，而不是直接把跳转回注册界面，把所有的信息全部清空，让用户重新填写！**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们应该这样做：**当发现用户输入的信息不合法时，把错误的信息记录下来，等到返回注册页面，就提示用户哪里出错了！**

- **在FormBean对象中添加一个HashMap集合（因为等会还要根据关键字把错误信息显示给用户！）**
- FormBean的全部代码如下：

```
    //表单提交过来的数据全都是String类型的，birthday也不例外！
    private String username;
    private String password;
    private String password2;
    private String email;
    private String birthday;

    //记录错误的信息
    private HashMap<String, String> error = new HashMap<>();
    
    
    /*用于判断表单提交过来的数据是否合法*/
    public boolean validate() {

        //用户名不能为空，并且要是3-8的字符 abcdABcd
        if (this.username == null || this.username.trim().equals("")) {

            error.put("username", "用户名不能为空，并且要是3-8的字符");
            return false;

        } else {
            if (!this.username.matches("[a-zA-Z]{3,8}")) {
                error.put("username", "用户名不能为空，并且要是3-8的字符");
                return false;
            }
        }

        //密码不能为空，并且要是3-8的数字
        if (this.password == null || this.password.trim().equals("")) {
            error.put("password", "密码不能为空，并且要是3-8的数字");
            return false;
        } else {
            if (!this.password.matches("\\d{3,8}")) {
                error.put("password", "密码不能为空，并且要是3-8的数字");
                return false;
            }
        }

        //两次密码要一致
        if (this.password2 != null && !this.password2.trim().equals("")) {
            if (!this.password2.equals(this.password)) {
                error.put("password2", "两次密码要一致");
                return false;
            }
        }

        //邮箱可以为空，如果为空就必须合法
            if (this.email != null && !this.email.trim().equals("")) {
                if (!this.email.matches("\\w+@\\w+(\\.\\w+)+")) {

                    error.put("email", "邮箱不合法！");
                    return false;
                }
        }

        //日期可以为空，如果为空就必须合法
        if (this.birthday != null && !this.birthday.trim().equals("")) {

            try {
                DateLocaleConverter dateLocaleConverter = new DateLocaleConverter();
                dateLocaleConverter.convert(this.birthday);
            } catch (Exception e) {

                error.put("birthday", "日期不合法！");
                return false;
            }
        }

        //如果上面都没有执行，那么就是合法的了，返回true
        return true;
    }

    //.....各种的setter和getter
```

- **在跳转到注册页面之前，把formbean对象存到request域中。在注册页面就可以把错误的信息取出来(使用EL表达式)！**
- **处理表单的Servlet的部分代码**

```
        //验证表单的数据是否合法，如果不合法就跳转回去注册的页面
        if(formBean.validate()==false){

            //在跳转之前，把formbean对象传递给注册页面
            request.setAttribute("formbean", formBean);
            request.getRequestDispatcher("/WEB-INF/register.jsp").forward(request, response);
            return;
        }
```

- **在注册页面中，使用EL表达式把错误的信息写出来**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 测试：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 效果：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

做到这里，还是有丢丢的问题，我们不应该把用户输入的数据全部清空的！你想想，**如果用户注册需要输入多个信息，仅仅一个出错了，就把全部信息清空，要他重新填写，这样是不合理的！**

- **我们在各个的输入项中使用EL表达式回显数据就行了**！

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 效果：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

还没有完善，**细心的朋友可以发现，上面图的日期也是错误的，但是没一次性标记出来给用户！要改也十分简单：在验证的时候，不要先急着return false 用一个布尔型变量记住，最后返回布尔型的变量即可**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

**无论注册成功还是失败都需要给用户一个友好界面的！**

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

#### 5.2登陆界面

登陆和注册是类似的，我们按着注册的步骤来写就对了！

**首先写一个提供登陆界面的Servlet**

```
        //直接跳转到登陆界面
        request.getRequestDispatcher("/WEB-INF/login.jsp").forward(request, response);
```

- 写登陆界面

```
<h1>这是登陆界面</h1>

<form action="${pageContext.request.contextPath}/LoginServlet" method="post">
    <table>
        <tr>
            <td>用户名</td>
            <td><input type="text" name="username"></td>
        </tr>
        <tr>
            <td>密码</td>
            <td><input type="password" name="password"></td>
        </tr>
        <tr>
            <td><input type="submit" value="提交"></td>
            <td><input type="reset" name="重置"></td>
        </tr>
    </table>
</form>
```

- 写处理登陆表单的Servlet

```
        //获取提交过来的数据
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        //调用service层的方法，去查询数据库（XML）是否有该条记录
        try {
            ServiceBussiness serviceBussiness = new UserServiceXML();
            User user = serviceBussiness.login(username, password);

            if (user == null) {
                request.setAttribute("message", "用户名或密码是错的");
            } else {
                request.setAttribute("message","登陆成功");
            }
        } catch (Exception e) {
            e.printStackTrace();
            request.setAttribute("message","登陆失败咯");
        }
        request.getRequestDispatcher("/message.jsp").forward(request, response);
```

- 效果：

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

------

#### 5.3把注册和登陆都挂在首页上

```
  <h1>这是首页！</h1>

  <a href="${pageContext.request.contextPath}/LoginUIServlet">登陆</a>
  <a href="${pageContext.request.contextPath}/RegisterUIServlet">注册</a>
  </body>
```

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# 总结

- 使用JSP+JavaBean开发一个简单计算器，是非常容易的，显示页面和请求都是交由JSP来做。没有什么新的知识点，用一些JSP行为就能完成了。
- MVC模式开发使用Servlet来做处理请求，代码量略大，但层次清晰
- 使用BeanUtils开发组件可以将request请求的参数封装到JavaBean对象中，Date属性要另外处理
- 校验的功能也是使用一个JavaBean来完成，目的就是为了可重用性，职责分工。同时，我们可以在该JavaBean设置一个Map集合来保存错误的信息，以便在前台上展示错误信息。