```
drop table tt;

create table emp(
    id  int auto_increment comment 'ID' primary key,
    name varchar(50) not null comment '姓名',
    age  int comment '年龄',
    job varchar(20) comment '职位',
    salary int comment '薪资',
    entrydate date comment '入职时间',
    managerid int comment '直属领导ID',
    dept_id int comment '部门ID'
)comment '员工表';

alter table emp add xb char(1) comment '性别';

INSERT INTO emp (id, name, age, xb, job, salary, entrydate, managerid, dept_id) VALUES
            (1, '金庸', 66, '男','总裁',20000, '2000-01-01', null,5),

            (2, '张无忌', 20, '男','项目经理',12500, '2005-12-05', 1,1),
            (3, '杨逍', 33, '男','开发', 8400,'2000-11-03', 2,1),
            (4, '韦一笑', 48,'男', '开发',11000, '2002-02-05', 2,1),
            (5, '常遇春', 43, '女','开发',10500, '2004-09-07', 3,1),
            (6, '小昭', 19, '女','程序员鼓励师',6600, '2004-10-12', 2,1),

            (7, '灭绝', 60, '男','财务总监',8500, '2002-09-12', 1,3),
            (8, '周芷若', 19, '女','会计',48000, '2006-06-02', 7,3),
            (9, '丁敏君', 23, '女','出纳',5250, '2009-05-13', 7,3),

            (10, '赵敏', 20, '女','市场部总监',12500, '2004-10-12', 1,2),
            (11, '鹿杖客',56,'男',  '职员',3750, '2006-10-03', 10,2),
            (12, '鹤笔翁', 19,'男', '职员',3750, '2007-05-09', 10,2),
            (13, '方东白', 19,'男', '职员',5500, '2009-02-12', 10,2),

            (14, '张三丰', 88,'男', '销售总监',14000, '2004-10-12', 1,4),
            (15, '俞莲舟', 38, '女','销售',4600, '2004-10-12', 14,4),
            (16, '宋远桥', 40, '女','销售',4600, '2004-10-12', 14,4),
            (17, '陈友谅', 42, '男' , null,2000, '2011-10-12', 1,null);


select name,job,age from emp;
select id, name, age, job,salary, entrydate, managerid, dept_id from emp;
select * from emp;

select dept_id as deptid from emp;

select distinct dept_id from emp;

desc emp;

show create table emp;

select * from emp;

-- 条件查询
select * from emp where age <25;
select * from emp where dept_id is not null;

select * from emp where dept_id = 3 and age < 30;

select * from emp where  age in(20, 35, 45);

select * from emp where name like '__';
select * from emp where age like '%9';

-- 聚合函数
select count(age) from emp;
select avg(age) from emp;
select sum(age) from emp where dept_id = 2;

-- 分组查询
select age, count(*) from emp group by age;

select dept_id, count(*) from emp where age < 50 group by dept_id having  count(*) > 1;

-- 排序查询
select * from emp order by age asc;
select * from emp order by age asc , entrydate desc ;

-- 分页查询
select * from emp limit 8, 10;

-- 练习
select * from emp where age = 20 or age = 21 or age = 22;
select * from emp where age in(20,21,22,23);

select * from emp where xb = '男' and (age between 20 and 40 )and name like '___';

select age,count(*) from emp where(age < 40 and age > 20) group by age;

select name,age from emp where age <= 35 order by age asc , entrydate desc ;

select * from emp where age between 20 and  40 and xb = '男'  order by age asc ,entrydate desc limit 5;
```