## 数据库笔记

### sql的执行顺序

- 第一步：执行FROM
- 第二步：WHERE条件过滤
- 第三步：GROUP BY分组
- 第四步：执行SELECT投影列
- 第五步：HAVING条件过滤
- 第六步：执行ORDER BY 排序

### 聚合函数中使用 when case

场景根据一个字段的值，分别统计不同值的个数
```
<select id="dome" resultMap="com.dome" parameterType="java.util.List">
    SELECT count(*) as count,
    sum(case when status = 1 then 1 else 0 end) as success,
    sum(case when status = 2 then 1 else 0 end) as fail
    FROM `test`
    
    GROUP BY code

</select>
```
