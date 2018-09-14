#广东省所有市、县的查询构想及实现代码

**先查询出广东省所有的市，然后再关联查询出广东省所有的区/县，包括含有null值得那一列，最后用 union all 将两个查询的结果关联显示在同一张表中**


    select a.id,c.cityName,b.cityName,a.cityName from s_provinces a
    	join s_provinces b on a.parentId=b.id
    	join s_provinces c on b.parentId=c.id
    where c.cityName='广东省'
    
    union all
    
    select d.id,e.cityName,d.cityName,null from s_provinces d
    	join s_provinces e on d.parentId=e.id
    where e.cityName='广东省';