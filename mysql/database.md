<div style="text-align: center;font-size: 40px;">MySQL</div>

## 建库

```shell
USE
`mysql`;

CREATE DATABASE `dbName` CHARACTER SET 'utf8mb4.' COLLATE 'utf8mb4_general_ci';

USE `dbName`;
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;
```

## 创建用户 & 赋权

```shell
CREATE USER IF NOT EXISTS 'lotus'@'%' IDENTIFIED BY 'lotus';
GRANT ALL PRIVILEGES ON `dbName`.* TO 'lotus'@'%';
FLUSH PRIVILEGES;
```

## 导出数据库

- 导出sql文件库 `mysqldump -u DBUser -p databaseName > dbName.sql`
- 导出sql文件表 `mysqldump -u DBUser -p databaseName tableName > tableName.sql`
    - -d 不导出数据 `mysqldump -u DBUser -p -d tableName > databaseName.sql`
    - 在create语句前加drop table `mysqldump -u DBUser -p --add-drop-table databaseName tableName > databaseName.sql`

## 导入数据库

```shell
mysql -u root -p
USE mysql;
CREATE DATABASE `dbName` CHARACTER SET 'utf8mb4.' COLLATE 'utf8mb4_general_ci';
USE dbName;
SET NAMES utf8mb4;
source /root/dbName.sql;
# 或者
mysql -u root -p dbName < dbName.sql
```

## example

> table
> > 清空表 `truncate TABLE table_name;` \
> > 添加主键 `alter table t_name add primary key(id);` \
> > 添加字段 `alter table t_name add column del_flag tinyint(1) NULL default 0 COMMENT '注释';` \
> > 修改id字段位置到最前 `alter table t_name modify id int first;` \
> > 修改id位置到name之后 `alter table t_name modify id int after name;` \
> > 修改字段名字及属性:把id改成new_id且字段类型为varchar(10) `alter table t_name change id new_id varchar(10) COMMENT '注释'';` \
> > 删除id字段 `alter table t_name drop column id;` \
> > 重命名表 `alter table t_name rename new_name;`
> 
> data
> > 增 `insert into t_name values('','','','');` \
> > 删 `delete from t_name where id=1111;` \
> > 改 `update t_name set id=110;` \
> > 查 `select * from t_name where time<'12:00:00'and date='2016-05-13';` \
> > 查 as 别名 `select name as '姓名' from student where id=1;` 