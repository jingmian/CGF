
# CGF(Comment Generate Form) 不写一行代码，实现增删改查

这是一种表字段注释的格式，一种思想，不限于任何编程语言，只要各语言实现了这种方式，就能自动实现CURD。  

根据表字段注释，自动实现添加,修改，列表，搜索基本功能。    
只要定义好字段的注释，就能实现增删改查。大大提高开发效率，让你真正飞起来！   

希望大家可以一起来完善这种格式，让其变成一种标准或协议。  


# demo 样例(基于thinkphp实现的)  
http://cgf.rrbrr.com/




# 格式说明  

以 |-，之类的做分隔  
注释标题 - htm控件类型 - 提示 |展现页面 | 校验类型 |  选项  

## 第一部分
注释标题: 一般是字段的中文标题，form表单的label  
html控件类型: select,checkbox,input,textare,datepicker,editor等,更多的控件需要你自己来实现。  
提示:一般是此字段的填写规范，如：允许字母或数字  


## 第二部分
展现页面:用位表示   
1       1          1       1  
添加  修改    列表   搜索项  
8       4           2      1  
可以用每1位的10进制数相加的和表示，也可以直接用二进制表示  
1011(二进制)  等价于 11(十进制)  

例:  
添加,修改，列表,搜索全显示 1111 = 15  
添加,修改，列表都要显示则是  1110 = 14  
添加，修改显示，列表不显示    1100 = 10  
添加，修改不显示，列表显示，一般像创建时间就是这样  0010 = 1  


## 第三部分
校验类型:reqiure,email,username,mobile等,用于后台校验,对应thinkphp的校验格式  ，
也支持前台验证，目前使用validform验证格式，也可使用正则表达式。


## 第四部分
选项： 选项1:选项1值，选项2：缺项2值  
   

# 样例写法
### 用户名--用户名为字符|1111|require:用户名必须填写-unique-<</\w{3,6}/i>>:用户名不合法    

#### [ 用户名 ] 控件label   
#### [ - ] 光有1个[ - ],没有内容，表示省略了内容，也就是省略了个控件类型，使用默认的控件类型
### [ 用户名为字符 ]，表示tip提示信息    
#### [ 1111 ] 表示用户名在增加，修改，列表，搜索页面都显示    
#### [ require ] 表示必填, unique表示为惟一,   
#### [ <</\w{3,6}/i>>:用户名不合法 ]   正则验证用户名，此正则意思是，用户名为3到6位的字符，并且不符合此正则的则报错 “用户名不合法”  



### 状态-select-禁用则不能访问 | 15| reqiure:必须填写  | 0:禁用,1:正常,2:审核中  
#### [ 状态 ]  控件label名为   
#### [ select ] 表示使用select控件  
####  [ 禁用则不能访问 ] 表示提示内容为   
#### [ 15 ] 为1111的10进制，等同于1111  
#### [ reqiure:必须填写 ] 表示必填，错误提示为：必须填写  
#### [ 0:禁用,1:正常,2:审核中 ]   表示状态这个select控件有3个选项，0,1,2表示key  


# 表定义的参考格式
```sql

CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '标题-hidden|0111',
  `username` varchar(255) NOT NULL DEFAULT '' COMMENT '用户名--用户名为字符|1111|require:用户名必须填写-unique-<</\\w{3,6}/i>>:用户名不合法',
  `password` varchar(255) DEFAULT '' COMMENT '密码-password|1110|require:密码必须填写',
  `email` varchar(255) DEFAULT NULL COMMENT '邮箱|1111|require:邮箱必须填写-email:邮箱格式不正确',
  `birthday` date DEFAULT NULL COMMENT '生日|1111|require:密码必须填写',
  `status` tinyint(1) NOT NULL DEFAULT '0' COMMENT '状态-select-禁用则不显示|1111|require|0:禁用,1:正常,2:审核中',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间|0010',
  `flag` varchar(255) NOT NULL DEFAULT '' COMMENT '标记-select|1100|require|0:推荐,1:置顶,2:广告',
  `intro` text COMMENT '用户介绍-editor|1100',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=34 DEFAULT CHARSET=utf8;



```



# 截图

![添加页面](https://github.com/caoygx/CGF/blob/master/screenshot/add.jpg)
![列表页面](https://github.com/caoygx/CGF/blob/master/screenshot/lists.jpg)



 