- **数据库登录：**
  ```
   mysql -h域名 -P端口 -u用户名 -p 密码 数据库名 --default-character-set=utf8;
  ```

- **数据库备份：**
  ```
  mysql -h域名 -P端口 -u用户名 -p密码 数据库名 --default-character-set=utf8 -e "select id, create_time, update_time from table_test where id =1;" > /home/xxx/sql/xxx.csv
  mysqldump -h域名 -P端口 -u用户名 -p密码 数据库名 > /home/xxx/sql/xxx.sql
  mysqldump -h域名 -P端口 -u用户名 -p 密码 --databases 数据库名 --tables table1 table2  >xxx.sql
  ```
  
- **备份：**
  -  INSERT INTO ... SELECT ... (table_name_new必须存在)
  ```
    INSERT INTO table_name_new ( c1, c2, c3 ) SELECT c1, c2, '固定值' FROM table_name_old WHERE 各种条件;
    INSERT INTO table_name_new SELECT * FROM table_name_old WHERE 各种条件;
  ```
- **查看表结构：**
  ```
   SELECT
    COLUMN_NAME 列名,
    COLUMN_TYPE 数据类型,
    DATA_TYPE 字段类型,
    CHARACTER_MAXIMUM_LENGTH 长度,
    IS_NULLABLE 是否为空,
    COLUMN_DEFAULT 默认值,
    COLUMN_COMMENT 备注
  FROM
    INFORMATION_SCHEMA. COLUMNS
  WHERE
    -- 数据库名，只需要修改成你要导出表结构的数据库即可
    table_schema = '数据库名'
  AND -- sx_jc_car为表名，到时候换成你要导出的表的名称
  -- 表名，只需要修改成你要导出表结构的表即可，如果不写的话，默认会查询出所有表中的数据，这样可能就分不清到底哪些字段是哪张表中的了，所以还是建议写上要导出的名名称
  table_name = '表名'
  
  ```
  
 - **msyql查询：**

```
# 各个视频的平均完播率

# 方式1
# select l.video_id,
# round(sum(
#         case when timestampdiff(second,l.start_time,l.end_time)>=i.duration then 1 else 0 end
#     )/count(*),3) as avg_comp_play_rate 
# from tb_user_video_log l
# left join tb_video_info i on l.video_id = i.video_id
# where year(l.start_time)=2021
# group by l.video_id
# order by avg_comp_play_rate desc;

# # 方式2
# select l.video_id,
# round(avg(
#         case when timestampdiff(second,l.start_time,l.end_time)>=i.duration then 1 else 0 end
#     ),3) as avg_comp_play_rate 
# from tb_user_video_log l
# left join tb_video_info i on l.video_id = i.video_id
# where year(l.start_time)=2021
# group by l.video_id
# order by avg_comp_play_rate desc;

# 方式3
# select l.video_id,
# round(avg(if(timestampdiff(second,l.start_time,l.end_time)>=i.duration,1,0)),3) as avg_comp_play_rate 
# from tb_user_video_log l
# left join tb_video_info i on l.video_id = i.video_id
# where year(l.start_time)=2021
# group by l.video_id
# order by avg_comp_play_rate desc;

# 方式4
# 函数：
#     1、round(XXX,3) 保留三位小数
#     2、case when XXX then 1 else 0 end
#     3、timestampdiff(second,XXX,XXX)
#     4、year(XXX)
#     5、avg(XXX)
#     6、sum(XXX)
#     2、if(XXX,1,0)

# 方式5
# select l.video_id,
# round(count(if(timestampdiff(second,l.start_time,l.end_time)>=i.duration,1,null))/count(*),3) as avg_comp_play_rate 
# from tb_user_video_log l
# left join tb_video_info i on l.video_id = i.video_id
# where year(l.start_time)=2021
# group by l.video_id
# order by avg_comp_play_rate desc;


# select l.video_id,
# round(count(
#         case when timestampdiff(second,l.start_time,l.end_time)>=i.duration then 1 else null end
#     )/count(*),3) as avg_comp_play_rate 
# from tb_user_video_log l
# left join tb_video_info i on l.video_id = i.video_id
# where year(l.start_time)=2021
# group by l.video_id
# order by avg_comp_play_rate desc;


# 函数：
#     1、round(XXX,3) 保留三位小数
#     2、case when XXX then 1 else 0 end
#     3、timestampdiff(second,XXX,XXX)
#     4、year(XXX)
#     5、avg(XXX)
#     6、sum(XXX)
#     7、count(XXX)
#     8、if(XXX,1,0)
```
<img width="887" alt="image" src="https://user-images.githubusercontent.com/38684698/160087357-78411271-8a41-4985-bf8d-4d699bc11d3a.png">
