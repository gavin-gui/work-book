## EL表达式学习

### 1. EL特点

- (1) 对象的简明访问
- (2) 对象属性的简单表示
- (3) 对集合元素的简单访问
- (4) 对请求参数、cookie和其它请求参数的简洁访问
- (5) EL定义了一个运算符的集合
- (6) 支持条件输出
- (7) 自动类型转换
- (8) 空值代替异常

---

### 2. EL语法
一般形式：`${expression}`<br/>
2.1 EL变量范围
(1) 如果没有指定变量的有效范围，JSP容器会默认依次在`pageScope,requestScope,sessionScope和applicationScope`所对应的有效范围中查找，找到就返回变量的值，否则返回null.<br/>
(2) 也可以为变量指定查找范围，如`${sessionScope.userName}`

2.2 EL关键字
EL保留的关键字共有16个：
`and(&&),eq(==),gt(>),true,instanceof,null,div(/),or(||),ne(!=),lt(<),not(!),false,le(<=),empty,mod(%).`

2.3访问运算符.和[]
(1)`${expressionb}`<br/>
(2)`${person['name']}`<br/>
   `${list[0]}` //数组或者List可以使用下标取值<br/>
(3)`${list[0].name}` //.和[]可以结合使用

2.4运算
(1)算术运算符：+,-,*,/,%(mod),-(负号),()(括号运算)。<br/>
(2)关系运算符：<,>,<=,>=,==,!=<br/>
(3)逻辑运算符：!(not),&&(and),||(or).<br/>
(4)empty运算：如果操作数值为null或操作数本身是一个空容器，空数组或长度为0的字符串，则返回true.如：
>  
    //下面都返回true
    ${empty ""}
    ${empty name}
    ${empty null}

2.5自动类型转换
EL表达式中自动类型转换规则如下：
(1)Object转换为String
- Object为数值时，依其值直接转换，如100.12转换为"100.12".
- Object为null时，返回空字符串""
- Object.toString()产生异常时，出错，否则返回Object.toString()

(2)Object转换为数值
- Object为boolean型时出错
- Object为null时，返回0
- Object为空字符串时，返回0
- Object为字符串时，若字符串可以转换为数值，则返回数值，否则出错

---

### 3. EL隐含对象
EL共有11个隐含对象：
`pageContext,param,paramValue,header,headerValues,cookie,initParam,pageScope,requestScope,sessionScope,aplicationScope` 
