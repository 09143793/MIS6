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
![用户test1可以查看到的页面](https://github.com/09143793/MIS6/blob/master/6.1.png)
####伪代码：
1.	根据用户的登录名test1在用户表里查对应的userId  
2.	根据userId去权限表里查对应的访问人类型为user、访问对象类型为Menu的对应的MenuId  
3.	根据MenuId去菜单（页面）表里查对应的菜单（页面）名称MenuName</br>
4.	 根据用户的登录名test1在用户表里查对应的userId</br>
5.	根据userId去用户角色表里查对应的roleId</br>
6.	根据roleId去权限表里查对应的访问人类型为role、访问对象类型为Menu的对应的MenuId</br>
7.	根据MenuId去菜单（页面）表里查对应的菜单（页面）名称MenuName</br>
8.	将第三步得到的菜单（页面）名称与第七步得到的菜单（页面）名称取并集</br>
###查询test1 可以对order页面进行的操作
####查询语句：
SELECT * FROM sys_button
where MenuNo=
(select MenuNo
               from
               (
    SELECT * 
    FROM sys_menu 
           where MenuID in
           (SELECT PrivilegeAccessKey 
            FROM cf_privilege 
            where PrivilegeMasterKey in
               (SELECT RoleID 
                FROM cf_role 
                where RoleID in                
                    (SELECT RoleID                 
                      FROM cf_userrole                
                      where UserID in
                         (SELECT UserID 
                           FROM cf_user                  
                           where LoginName='test1')))
               and PrivilegeAccess='Sys_Menu')) temp
 where MenuName='订单');
 ####查询结果
 ![用户test1 可对order页面进行的操作](https://github.com/09143793/MIS6/blob/master/6.2.png)<br/>
 ####伪代码：
 1.根据用户的登录名test1在用户表里查对应的userID.<br/>
 2.根据userID在权限表里查对应的访问人类性是user，访问对象类型是button的对应的buttonId<br/><br/>
 3.根据页面名称order去菜单表里查找对应的meunNo.<br/>
 4.根据menuNo和buttonId在按钮表里查对应的操作名称buttonName<br/>
 5.根据用户的登录名test1在用户表里查对应的userID。<br/>
 6.根据userID去角色表里查找对应的roleId。<br/>
 7.根据roleId在权限表里查对应的访问人类型role，访问对象类型为button的所对应的buttonId<br/>
 8.根据页面名称order在菜单表里查找对应的menuNo。<br/>
 9.根据menuNo和buttonId在按钮表里查对应的操作名称buttonName<br/>
 10.将第4步得到的操作名称和第九步得到的操作名称合并取并集。
