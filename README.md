# MIS4

# 查询用户test1可以查看的页面（Sys_menu）和对订单(order)页面中的操作权限(sys_button)

# SQL语句：

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

# 伪代码
1.从用户表cf_user中通过用户名LoginName查找用户ID UserID
2.通过UserID从用户角色表cf_userrole中查找该用户对应角色的集合
3.从权限表中通过用户ID或用户对应角色集合，查找对象名为CF_Role 和 访问权限为permit的对应MenuID
4.从sys_menu表中通过MenuID，查找对应MenuName

查询结果：
![](/)

