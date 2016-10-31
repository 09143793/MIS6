# MIS6
### 查询用户test1可以查看的页面（Sys_menu）
####查询语句：
select d.MenuNo,d.MenuName  
from sys_menu d  
where d.MenuID in  
    (select c.PrivilegeAccessKey  
from cf_privilege c  
where c.PrivilegeAccess='Sys_Menu'   
and c.PrivilegeMaster='CF_User'  
and c.PrivilegeMasterKey=  
            (select distinct a.UserID  
        from cf_user a  
        where a.LoginName='test1'))  
union   
select d.MenuNo,d.MenuName  
from sys_menu d  
where d.MenuID in  
    (select c.PrivilegeAccessKey  
from cf_privilege c  
where c.PrivilegeAccess='Sys_Menu'   
and c.PrivilegeMaster='CF_Role'  
and c.PrivilegeMasterKey=  
            (select distinct b.RoleID  
        from cf_userrole b  
        where b.UserID=  
                    (select distinct a.UserID  
                from cf_user a  
                where a.LoginName='test1')))
####查询结果：
![用户test1可以查看到的页面]（）
####伪代码：
1.	根据用户的登录名test1在用户表里查对应的userId  
2.	根据userId去权限表里查对应的访问人类型为user、访问对象类型为Menu的对应的MenuId  
3.	根据MenuId去菜单（页面）表里查对应的菜单（页面）名称MenuName</br>
4.	 根据用户的登录名test1在用户表里查对应的userId</br>
5.	根据userId去用户角色表里查对应的roleId</br>
6.	根据roleId去权限表里查对应的访问人类型为role、访问对象类型为Menu的对应的MenuId</br>
7.	根据MenuId去菜单（页面）表里查对应的菜单（页面）名称MenuName</br>
8.	将第三步得到的菜单（页面）名称与第七步得到的菜单（页面）名称取并集</br>
