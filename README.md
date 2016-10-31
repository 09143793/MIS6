# MIS6
### 查询用户test1可以查看的页面（Sys_menu）
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
