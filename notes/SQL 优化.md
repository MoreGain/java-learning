# SQL 优化

1. 查询优化，避免全表扫描，对 where 和 order by 涉及的列上建立索引
2. 避免在 where 子句中进行 null 值判断，会导致搜索引擎放弃使用索引进行全表扫描，可以将 null 值设置默认值为 0
3. 避免 where 子句中使用 != 或 <> 操作符，否则搜索引擎将放弃使用索引
4. 避免 where 子句中使用 or 连接条件，否则搜索引擎将放弃使用索引，可以通过 union all 来代替 
5. in 和 not in 慎用，也会导致全表扫描，连续数值尽量用 between and
6. 不使用模糊查询，会导致全表扫描，特别将通配符作为头
7. 避免对字段进行表达式操作，如 `where num/2 = 100` ，会导致放弃使用索引
8. 应尽量避免在 where 子句中对字段进行函数操作，这将导致引擎放弃使用索引而进行全表扫描
9. 不要在 where 子句中的“=”左边进行函数、算术运算或其他表达式运算，否则系统将可能无法正确使用索引
10. 在使用索引字段作为条件时，如果该索引是复合索引，那么必须使用到该索引中的第一个字段作为条件时才能保证系统使用该索引，否则该索引将不会被使用，并且应尽可能的让字段顺序与索引顺序相一致
11. 并不是所有索引对查询都有效，SQL 是根据表中数据来进行查询优化的，当索引列有大量数据重复时，SQL 查询可能不会去利用索引 
12. 索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，    
    因为 insert 或 update 时有可能会重建索引，一个表的索引数最好不要超过6个
13. 尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这会降低查询和连接的性能，并会增加存储开销。这是因为引擎在处理查询和连接时会逐个比较字符串中每一个字符，而对于数字型而言只需要比较一次就够了
14. 尽可能的使用 varchar 代替 char ，因为首先变长字段存储空间小，可以节省存储空间，其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些
15. 不要使用 `select *`

*参考资料：[sql优化的几种方式](https://blog.csdn.net/qq_38789941/article/details/83744271)*

