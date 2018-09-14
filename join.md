# 广东省所有市、县的查询构想及实现代码

**先查询出广东省所有的市，然后再关联查询出广东省所有的区/县，包括含有null值得那一列，最后用 union all 将两个查询的结果关联显示在同一张表中**


    select a.id,c.cityName,b.cityName,a.cityName from s_provinces a
    	join s_provinces b on a.parentId=b.id
    	join s_provinces c on b.parentId=c.id
    where c.cityName='广东省'
    
    union all
    
    select d.id,e.cityName,d.cityName,null from s_provinces d
    	join s_provinces e on d.parentId=e.id
    where e.cityName='广东省';

## union 和 union all 的区别

### Union：对两个结果集进行并集操作，不包括重复行，同时进行默认规则的排序；
### Union All：对两个结果集进行并集操作，包括重复行，不进行排序；
### union和union all的区别是,union会自动压缩多个结果集中的重复结果，而union all则将所有的结果全部显示出来，不管是不是重复。

