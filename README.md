# MIS4

 查询用户test1可以查看的页面（Sys_menu）

## SQL语句：

SELECT MenuName 

FROM sys_menu 

WHERE MenuID IN

(SELECT PrivilegeAccessKey

FROM cf_privilege

WHERE ((PrivilegeMaster='CF_Role' AND PrivilegeMasterKey IN

(SELECT RoleID

FROM cf_userrole

WHERE UserID=

(SELECT UserID

FROM cf_user

WHERE LoginName='test1')

)

)

OR (PrivilegeMaster='CF_User' AND PrivilegeMasterKey=

(SELECT UserID

FROM cf_user

WHERE LoginName='test1')

)

)

AND PrivilegeAccess='Sys_Menu'

AND PrivilegeOperation='Permit'

) 

## 伪代码
1.从用户表cf_user中通过用户名LoginName查找用户ID UserID

2.通过UserID从用户角色表cf_userrole中查找该用户对应角色的集合

3.从权限表中通过用户ID或用户对应角色集合，查找对象名为CF_Role 和 访问权限为permit的对应MenuID

4.从sys_menu表中通过MenuID，查找对应MenuName

查询结果：
![](/)


查询用户test1对订单(order)页面中的操作权限(sys_button)
## SQL语句
SELECT PrivilegeMaster,PrivilegeAccess,BtnName

FROM cf_privilege AS cp LEFT JOIN sys_button AS sb ON cp.PrivilegeAccessKey=sb.BtnID

WHERE((PrivilegeMaster='CF_Role' AND PrivilegeMasterKey IN

(SELECT RoleID

FROM cf_userrole

WHERE UserID=

(SELECT UserID

FROM cf_user

WHERE LoginName='test1')

)

)

OR (PrivilegeMaster='CF_User' AND PrivilegeMasterKey=

(SELECT UserID

FROM cf_user

WHERE LoginName='test1')

)

)

AND PrivilegeAccess='Sys_Button'

AND PrivilegeOperation='Permit'

AND PrivilegeAccessKey IN 

(SELECT BtnID

FROM sys_button

WHERE MenuNo=

(SELECT MenuNo

FROM sys_menu

WHERE MenuName='订单')

)

##伪代码
1.从用户表cf_user中通过用户名LoginName查找用户ID UserID
2.通过UserID从用户角色表cf_userrole中查找该用户对应角色的集合
3.从权限表中通过用户ID或用户对应角色集合，查找对象名为Sys_Button 和 访问权限为permit的对应MenuID
4.从sys_menu表中找到名称为‘订单’的MenuID
5.从sys_button表中找出MenuName为订单的MenuNo
6.再从第5步中找到的MenuNo中找出属于第3步中MenuID的记录
7.通过MenuID连接PrivilegeAccessKey与Sys_Button
8.选择出所选记录对应的权限类型，对象类型，对象名称
