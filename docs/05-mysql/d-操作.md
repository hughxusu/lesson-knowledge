# 操作数据表

本课程以音乐歌曲库为例来介绍数据库表的设计与操作，根据前面的内容首先创建一个歌曲表

```sql
create table songs (
    id bigint unsigned not null auto_increment primary key comment '歌曲ID',
    title varchar(255) not null comment '歌曲标题',
    duration int unsigned not null default 0 comment '时长(秒)',
    file_url varchar(512) not null comment '文件路径',
    audio_format enum('mp3', 'flac', 'wav', 'aac', 'ogg', 'm4a') not null default 'mp3' comment '音频格式',
    file_size bigint unsigned default 0 comment '文件大小(字节)',
    created_at timestamp default current_timestamp comment '入库时间',
    update_at timestamp default current_timestamp on update current_timestamp comment '更新时间'
) comment = '歌曲表';
```

* `CURRENT_TIMESTAMP`设置时间为写入记录的当前时间。
* `ON UPDATE`当记录更新时触发的操作。

## 数据操作

DML主要包括数据的增、删、改操作，对应的动词关键字是`INSERT`、`UPDATE`、`DELETE`。

### 添加数据

#### 1. 给指定字段添加数据

```sql
insert into songs (title, duration, file_url, file_size)
values ('七里香', 299, 'https://cdn.music.com/tracks/qilixing.mp3', 5242880);
```

* `(title, duration, file_url, file_size)`添加数据的字段，如果字段非空必须添加数据，不是非空或有默认值可以选填。
* `values`值为对应的数据。

> [!alert]
>
> 缺失非空字段，添加数据会报错。
>
> ```sql
> insert into songs (title) values ('勇气');
> ```

在DataGrip中查看添加成功的数据

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-20_17-15-37.jpg" style="zoom:40%;" />

#### 2. 给全部字段添加数据

```sql
insert into songs
values (
    null,
    '勇气',
    299,
    'https://cdn.music.com/tracks/yongqi_v1.mp3',
    'mp3',
    5247896,
    current_timestamp,
    current_timestamp
);
```

* 给全部字段添加不能省略数据，自动增长可以填`null`。

#### 3. 批量添加数据

```sql
insert into songs (title, duration, file_url, audio_format, file_size) values
('夜曲', 239, 'https://cdn.music.com/tracks/yequ.mp3', 'mp3', 4194304),
('以父之名', 342, 'https://cdn.music.com/tracks/yifuzhiming.flac', 'flac', 31457280),
('晴天', 269, 'https://cdn.music.com/tracks/qingtian.m4a', 'm4a', 5242880);
```

* 批量添加数据中字段只需要写一次。

> [!warning]
>
> 1. 插入数据时，指定的字段顺序需要与值的顺序是一一对应的。
> 2. 字符串和日期型数据应该包含在引号中。
> 3. 插入的数据大小，应该在字段的规定范围内。

### 修改数据

`update`关键字用于数据的更新

```sql
update songs set duration = 310, file_url = 'https://cdn.music.com/tracks/qilixing_v2.mp3' where id = 4;
```

* `set`表示更新数据字段和值。
* 更新值采用键值对形式`duration = 310`。
* 可以同时更新多个字段。
* `where`关键字，用于控制数据过滤条件；`id = 4`表示第几条数据。

更新数据后应该查看更新结果

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-20_17-58-18.jpg" style="zoom:40%;" />

修改全部数据

```sql
update songs set file_size = 3000000;
```

> [!alert]
>
> 如果不设置条件条件，将会修改整张表的所有数据。

### 删除数据

`delete`关键字用于数据的删除

```sql
delete from songs where id = 9;
```

* `from`关键字用于确定数据源

删除全部数据

```sql
delete from songs;
```

> [!warning]
>
> 1. `delete`语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。
>
> 2. `delete`语句不能删除某一个字段的值，可以使用`update`，将该字段值置为`null`。

## DataGrip操作表

前面的表和数据操作，均可以使用DataGrip完成。

### 创建表

创建新的表

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-20_19-15-07.jpg" style="zoom:40%;" />

编辑表和相相关列

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-20_19-24-43.jpg" style="zoom:40%;" />

### 数据操作

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-20_20-06-28.jpg" style="zoom:40%;" />
