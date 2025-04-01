---
title: "Mybatis-Plus"
date: 2025-04-01
teaser: "https://mpimg.cn/view.php/3423bfdc1d93d7dc87346a88813e55dd.png"
categories: [数据库]
tags: [mybatis-Plus, 数据库]
---



# Mybatis-Plus



首先需要先引入依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.3.1</version>
</dependency>
```



### BaseMapper

使用方法:

```java
/*
 * Copyright (c) 2011-2022, baomidou (jobob@qq.com).
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
package com.baomidou.mybatisplus.core.mapper;

import com.baomidou.mybatisplus.core.conditions.Wrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.core.toolkit.Constants;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.exceptions.TooManyResultsException;

import java.io.Serializable;
import java.util.Collection;
import java.util.List;
import java.util.Map;

/*

               :`
                    .:,
                     :::,,.
             ::      `::::::
             ::`    `,:,` .:`
             `:: `::::::::.:`      `:';,`
              ::::,     .:::`   `@++++++++:
               ``        :::`  @+++++++++++#
                         :::, #++++++++++++++`
                 ,:      `::::::;'##++++++++++
                 .@#@;`   ::::::::::::::::::::;
                  #@####@, :::::::::::::::+#;::.
                  @@######+@:::::::::::::.  #@:;
           ,      @@########':::::::::::: .#''':`
           ;##@@@+:##########@::::::::::: @#;.,:.
            #@@@######++++#####'::::::::: .##+,:#`
            @@@@@#####+++++'#####+::::::::` ,`::@#:`
            `@@@@#####++++++'#####+#':::::::::::@.
             @@@@######+++++''#######+##';::::;':,`
              @@@@#####+++++'''#######++++++++++`
               #@@#####++++++''########++++++++'
               `#@######+++++''+########+++++++;
                `@@#####+++++''##########++++++,
                 @@######+++++'##########+++++#`
                @@@@#####+++++############++++;
              ;#@@@@@####++++##############+++,
             @@@@@@@@@@@###@###############++'
           @#@@@@@@@@@@@@###################+:
        `@#@@@@@@@@@@@@@@###################'`
      :@#@@@@@@@@@@@@@@@@@##################,
      ,@@@@@@@@@@@@@@@@@@@@################;
       ,#@@@@@@@@@@@@@@@@@@@##############+`
        .#@@@@@@@@@@@@@@@@@@#############@,
          @@@@@@@@@@@@@@@@@@@###########@,
           :#@@@@@@@@@@@@@@@@##########@,
            `##@@@@@@@@@@@@@@@########+,
              `+@@@@@@@@@@@@@@@#####@:`
                `:@@@@@@@@@@@@@@##@;.
                   `,'@@@@##@@@+;,`
                        ``...``

 _ _     /_ _ _/_. ____  /    _
/ / //_//_//_|/ /_\  /_///_/_\      Talk is cheap. Show me the code.
     _/             /
 */

/**
 * Mapper 继承该接口后，无需编写 mapper.xml 文件，即可获得CRUD功能
 * <p>这个 Mapper 支持 id 泛型</p>
 *
 * BaseMapper<T>:这个泛型代表需要操作的数据库的DTO
 * 
 */
public interface BaseMapper<T> extends Mapper<T> {

    /**
     * 插入一条记录
     *
     * @param entity 实体对象
     */
    int insert(T entity);

    /**
     * 根据 ID 删除
     *
     * @param id 主键ID
     */
    int deleteById(Serializable id);

    /**
     * 根据实体(ID)删除
     *
     * @param entity 实体对象
     * @since 3.4.4
     */
    int deleteById(T entity);

    /**
     * 根据 columnMap 条件，删除记录
     *
     * @param columnMap 表字段 map 对象
     */
    int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

    /**
     * 根据 entity 条件，删除记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
     */
    int delete(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 删除（根据ID或实体 批量删除）
     *
     * @param idList 主键ID列表或实体列表(不能为 null 以及 empty)
     */
    int deleteBatchIds(@Param(Constants.COLL) Collection<?> idList);

    /**
     * 根据 ID 修改
     *
     * @param entity 实体对象
     */
    int updateById(@Param(Constants.ENTITY) T entity);

    /**
     * 根据 whereEntity 条件，更新记录
     *
     * @param entity        实体对象 (set 条件值,可以为 null)
     * @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
     */
    int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);

    /**
     * 根据 ID 查询
     *
     * @param id 主键ID
     */
    T selectById(Serializable id);

    /**
     * 查询（根据ID 批量查询）
     *
     * @param idList 主键ID列表(不能为 null 以及 empty)
     */
    List<T> selectBatchIds(@Param(Constants.COLL) Collection<? extends Serializable> idList);

    /**
     * 查询（根据 columnMap 条件）
     *
     * @param columnMap 表字段 map 对象
     */
    List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

    /**
     * 根据 entity 条件，查询一条记录
     * <p>查询一条记录，例如 qw.last("limit 1") 限制取一条记录, 注意：多条数据会报异常</p>
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    default T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper) {
        List<T> list = this.selectList(queryWrapper);
        // 抄自 DefaultSqlSession#selectOne
        if (list.size() == 1) {
            return list.get(0);
        } else if (list.size() > 1) {
            throw new TooManyResultsException("Expected one result (or null) to be returned by selectOne(), but found: " + list.size());
        } else {
            return null;
        }
    }

    /**
     * 根据 Wrapper 条件，判断是否存在记录
     *
     * @param queryWrapper 实体对象封装操作类
     * @return 是否存在记录
     */
    default boolean exists(Wrapper<T> queryWrapper) {
        Long count = this.selectCount(queryWrapper);
        return null != count && count > 0;
    }

    /**
     * 根据 Wrapper 条件，查询总记录数
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    Long selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 entity 条件，查询全部记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录
     * <p>注意： 只返回第一个字段的值</p>
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 entity 条件，查询全部记录（并翻页）
     *
     * @param page         分页查询条件（可以为 RowBounds.DEFAULT）
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    <P extends IPage<T>> P selectPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录（并翻页）
     *
     * @param page         分页查询条件
     * @param queryWrapper 实体对象封装操作类
     */
    <P extends IPage<Map<String, Object>>> P selectMapsPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
}

```



常见方法:

![image-20250401152334872](C:\Users\XiaoXin\AppData\Roaming\Typora\typora-user-images\image-20250401152334872.png)

常见注解

```java
mybatis-plus默认
    1.表名首字母小写进行找
    2.名字为ID会作为主键
    3.字段名为转驼峰名，进行查找 create_time


@TableName("数据库表名")
//如果当前的DTO名与数据库名不一致，可以用这个修改
@TableId("主键ID")
//如果当前主键ID与数据库中的主键ID不一致，可以用这个修改,如果主键没有为空，他默认策略为雪花算法,进行添加
TableField("字段名")
//如果字段名与数据库中字段名不一致，可以用这个修改 exist 这个默认为false 这个可以指定改字段是否为数据库中的字段，如果为true他就忽略这个字段
```

Yaml配置

```yaml
mybatis-plus:
  configuration:
    default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler # 枚举处理器
  global-config:
    db-config:
      update-strategy: not_null # 只更新非空字段
      id-type: auto # 主键自增
```

### 枚举处理器

配置枚举处理器

```yaml
mybatis-plus:
  configuration:
    default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler
```

使用方法

```java
@EnumValue:这个是代表数据库中的对应的字段
@JsonValue:这个是代表json返回值对应的字段
```

