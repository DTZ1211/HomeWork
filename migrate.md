#分割表的方法总结#

##城市表的分割##

#####首先，先列出所有和城市有关的字段，然后用 as 和 union all 方法创建新的 lagou_city 表，在插入数据的同时还要将 district 字段里值为 null 的数据也一起插入#####

##公司表的分割##

#####公司表和城市表的分割步骤也是差不多的，也是先将与公司表有关的字段和值先查询出来，在用相关的语句进行表的创建和插入数据#####

##lagou_position表的分割##

#####lagou_position表，在上面两个表的基础上进行字段的删减，做到表中字段与表主题内容的相关和不冲突，同时需要创建一个 id 字段可以与公司表和城市表进行相关的查询操作#####