### 携程团队Apollo配置中心 新增项目功能.Net版本实现，及使用Demo

###### 因Apollo官方Rest Api未提供新增应用接口，在进行自动化运维工作时，配置管理使用不方便，故参照Apollo官方源码（Java）翻译实现.Net版本


关于Apollo数据表结构，不再进行赘述，请查阅
- [Apollo部署文档](https://ctripcorp.github.io/apollo/#/zh/deployment/quick-start)
- [Apollo开发文档](https://ctripcorp.github.io/apollo/#/zh/development/apollo-development-guide)


该Demo实现思路：
- 模拟Apollo官方源码，首先对`apolloconfigdb`库下的`app, appnamespace,namespace,cluster`表进行数据写入,示例如下:
```
// declare variables
set @appId='ApolloCreateAppTest';
set @user = 'admin';
set @userEmail = 'admin@xx.com';
set @cluster = 'default';
set @namespace = 'application';

// insert apollo app record
INSERT INTO `apolloconfigdb`.`app`(`AppId`, `Name`, `OrgId`, `OrgName`, `OwnerName`, `OwnerEmail`, `IsDeleted`, `DataChange_CreatedBy`, `DataChange_CreatedTime`, `DataChange_LastModifiedBy`, `DataChange_LastTime`) VALUES (@appId, '测试创建Apollo项目', 'test', '测试部门', @user, @userEmail, b'0', @user, '2021-04-07 11:09:27', @user, NULL);

// insert apollo app cluster record
INSERT INTO `apolloconfigdb`.`cluster`(`Name`, `AppId`, `ParentClusterId`, `IsDeleted`, `DataChange_CreatedBy`, `DataChange_CreatedTime`, `DataChange_LastModifiedBy`, `DataChange_LastTime`) VALUES (@cluster, @appId, 0, b'0', @user, '2021-04-07 11:09:27', @user, NULL);

// insert apollo app namespace record
INSERT INTO `apolloconfigdb`.`namespace`(`AppId`, `ClusterName`, `NamespaceName`, `IsDeleted`, `DataChange_CreatedBy`, `DataChange_CreatedTime`, `DataChange_LastModifiedBy`, `DataChange_LastTime`) VALUES (@appId, @cluster, @namespace, b'0', @user, '2021-04-07 11:09:27', @user, NULL);

// insert apollo app-namespace relation record
INSERT INTO `apolloconfigdb`.`appnamespace`(`Name`, `AppId`, `Format`, `IsPublic`, `Comment`, `IsDeleted`, `DataChange_CreatedBy`, `DataChange_CreatedTime`, `DataChange_LastModifiedBy`, `DataChange_LastTime`) VALUES (@namespace, @appId, 'properties', b'0', 'default app namespace created by devops', b'0', @user, '2021-04-07 11:09:27', @user, NULL);
```
- 再对`apolloportaldb`库下的`app,appnamespace`表进行数据写入，示例如下：
```
// insert apollo app record
INSERT INTO `apolloportaldb`.`app`(`AppId`, `Name`, `OrgId`, `OrgName`, `OwnerName`, `OwnerEmail`, `IsDeleted`, `DataChange_CreatedBy`, `DataChange_CreatedTime`, `DataChange_LastModifiedBy`, `DataChange_LastTime`) VALUES (@appId, '测试创建Apollo项目', 'test', '测试部门', @user, @userEmail, b'0', @user, '2021-04-07 11:09:27', @user, NULL);

// insert apollo app cluster record
INSERT INTO `apolloportaldb`.`cluster`(`Name`, `AppId`, `ParentClusterId`, `IsDeleted`, `DataChange_CreatedBy`, `DataChange_CreatedTime`, `DataChange_LastModifiedBy`, `DataChange_LastTime`) VALUES (@cluster, @appId, 0, b'0', @user, '2021-04-07 11:09:27', @user, NULL);
```
- 完成以上基础数据写入后，需要对项目权限进行处理，即该项目中提供的方法
```
Task InitAppRoles(App app);
```
- 完成权限数据的生成与写入后，若存在调用Rest Api的使用场景，则需添加接口权限
```
//TODO:添加权限方法
```