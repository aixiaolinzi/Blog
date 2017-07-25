Android数据库，第三篇。（主要就是研究Sqlite数据库真正的实战）
##前言
前两篇主要讲解了一些理论，今天我们举一些简单的例子。全部上实例。
##任务
学会举一反三。
##Data Query Language 数据查询语言
#####情况一：先上表格，这是学生成绩表格。表的名称是STUDENT。
ID  |        SHUXUE   |   YUWEN   |    YINGYU   |   WULI  |      HUAXUE
----------|  ----------  ----------  ----------  ----------  ----------
2006001  |   108    |     119      |   98        |  127   |      136
2006002  |   149   |      105     |    110      |   142   |      129
2006003  |   139    |     125     |    110     |    120   |      104
2006004  |   90     |     135     |    130      |   145   |      114

注意：数据库的查询语句是不区分大小写的。
查询所有学生的信息：
`select * from STUDENT`
查询学生的学号和对应的英语成绩：
`select ID,YINGYU from student`
过滤表中的重复数据：
` select distinct YINGYU from student`
在所有学生数学分数上加10分特长分
` select shuxue+10  from student;`
统计每个学生的总分:
` select shuxue+yuwen+yingyu+wuli+huaxue from student;`
或者
` SELECT (SHUXUE +YUWEN+YINGYU+WULI+HUAXUE)as total from student;`
使用别名表示学生分数：
`select id, shuxue+yuwen+yingyu+wuli+huaxue 别名 from student;`
> 下面是对比
sqlite> select id, shuxue+yuwen+yingyu+wuli+huaxue **zongfen** from student;
ID    |      zongfen
-----|-----  ----------
2006001 |    588
2006002  |   635
2006003   |  598
2006004   |  614
sqlite> select id, shuxue+yuwen+yingyu+wuli+huaxue  from  student;
ID         | shuxue+yuwen+yingyu+wuli+huaxue
---------|-  -------------------------------
2006001 |    588
2006002  |   635
2006003  |   598
2006004  |   614

查询总分大于600分的所有同学 :
` select * from student where(yuwen+shuxue+yingyu+wuli+huaxue)>600;`
查询数学成绩在100-120之间的同学 :
 ` select * from student where shuxue between 100 and 120;`
对总分进行排序后输出，然后按照从高到底的顺序输出 :
`SELECT (SHUXUE +YUWEN+YINGYU+WULI+HUAXUE )  from student order BY SHUXUE
+YUWEN+YINGYU+WULI+HUAXUE DESC;`
或者是：
`SELECT (SHUXUE +YUWEN+YINGYU+WULI+HUAXUE ) AS TOTAL from student order BY TOTAL DESC;`
#####情况二：表格如下所示：表格的名称是information
sqlite> select * from information;
ID    |      PROJECT   |  FRACTION
-----|-----  ----------  ----------
2001 |       yingyu  |    80
2001  |      shuxue  |    130
2001   |     wuli       | 130
2001   |     yuwen    |   110
2001   |     huaxue    |  123
2002   |     yingyu   |   123
2002   |     yuwen    |   110
2002   |     huaxue    |  123
2002   |     shuxue    |  123
2002   |     wuli     |   123

下面的操作主要使用  `group by `;
给出说有学生的总分：
` select id,SUM(FRACTION)from information group by ID;`
计算平均科目的平均分：
`select id,sum(FRACTION)/count(*) from information  group by ID ;`
查询总分大于600的学生ID：
` select id from information  group by ID having SUM(FRACTION)>600;`
查询平均成绩大于110的学生ID：
`select id from information  group by ID having SUM(FRACTION)/count(*) >115;`
或
` select * from information group by ID having avg(FRACTION)>115;`
给出成绩全部在90分及以上的学生信息（包含ID、课程、分数）：
> 思路：上面可以查询平均成绩大于110的学生的ID，那么我们根据上面的条件可以得到某个ID的所有成绩的最小值是否大于90，这样就可以得到所有满足条件的ID，然后根据ID作为条件，就能得出上面的值。
> sqlite> select * from information where ID in (select ID from information group by ID having min(FRACTION)>=90);
> 思路2：得到成绩小于90的学生的ID，得到了学生的ID，我们就利用not in 语法。得到满足条件的ID，根据ID作为条件得到上面的值。
> sqlite> select * from information where ID not in(select distinct ID from information where FRACTION < 90);

查询出该班级里不同科目人数的总数：
` select PROJECT ,count(id) from information group by PROJECT;`
查询出ID是“20”开头学生中平均成绩大于115分的学生信息
` select * from information where ID IN (select ID from information where ID like '20%' group by ID having avg(FRACTION)>115);`


