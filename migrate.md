# 分割表的方法总结

## 城市表的分割

##### 首先，先列出所有和城市有关的字段，然后用 as 和 union all 方法创建新的 lagou_city 表，在插入数据的同时还要将 district 字段里值为 null 的数据也一起插入

        create table lagou_city as select * from (
    	select 
    	d.id as id,
    	b.cityName as province,
    	c.cityName as city,
    	d.cityName as district from s_provinces a
    	join s_provinces b on a.id = b.parentId
    	join s_provinces c on b.id = c.parentId
    	join s_provinces d on c.id = d.parentId
    	where a.cityName = '中国'
    	
    	union all
    	
    	select c.id, b.cityName, c.cityName, null from s_provinces a
    	 join s_provinces b on a.id = b.parentId
    	 join s_provinces c on b.id = c.parentId
    	where a.cityName = '中国'
      ) lc;


## 公司表的分割

##### 公司表和城市表的分割步骤也是差不多的，也是先将与公司表有关的字段和值先查询出来，在用相关的语句进行表的创建和插入数据
    
    create table lagou_company
    select * from(
    	select 
    	distinct(company_id) as cid,
    	company_short_name as short_name,
    	company_full_name as full_name,
    	company_size as size,
    	financestage 
    from lagou_position) cr;



## lagou_position表的分割

##### lagou_position表，在上面两个表的基础上进行字段的删减，做到表中字段与表主题内容的相关和不冲突，同时需要创建一个 id 字段可以与公司表和城市表进行相关的查询操作

    create table lagou_position as
    select pid, id as city, company_id as company, `position`, `field`, salary_min, salary_max, workyear, education, ptype, pnature, advantage, published_at, updated_at
    from (select * from lagou_position_first where district is null) p
    join lagou_city c on c.city like concat(p.city, '%') and c.district is null;

##### 表结构创建完了之后，将相关字段的数据插入到表中

    insert into lagou_position
    select pid, id as city, company_id as company, `position`, `field`, salary_min, salary_max, workyear, education, ptype, pnature, advantage, published_at, updated_at
    from (select * from lagou_position_first where district is not null) p
      join lagou_city c on c.city like concat(p.city, '%') and c.district like concat(p.district, '%');