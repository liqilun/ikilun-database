NOW()取的是语句开始执行的时间，SYSDATE()取的是动态的实时时间。

SELECT concat_ws(',',citycode,g.company_name) as t from gym g;


group_concat()
group_concat([DISTINCT] 要连接的字段 [Order BY ASC/DESC 排序字段] [Separator '分隔符'])

　　对下面的一组数据使用 group_concat()

　　| id |name

　　|1 | 10|
　　|1 | 20|
　　|1 | 20|
　　|2 | 20|
　　|3 | 200   |
　　|3 | 500   |

　　1、select id,group_concat(name) from aa group by id;

　　|1 | 10,20,20|
　　|2 | 20 |
　　|3 | 200,500|

　　2、select id,group_concat(name separator ';') from aa group by id;

　　|1 | 10;20;20 |
　　|2 | 20|
　　|3 | 200;500   |

　　3、select id,group_concat(name order by name desc) from aa group by id;

　　|1 | 20,20,10   |
　　|2 | 20|
　　|3 | 500,200|

　　4、select id,group_concat(distinct name) from aa group by id;

　　|1 | 10,20|
　　|2 | 20   |
　　|3 | 200,500 |