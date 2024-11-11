# Cypher API
> 此文档主要介绍了TuGraph-Cypher的详细使用说明。
## 1. 操作符
### 1.1. 概述
Operators支持进度一览：

| 类别                                 | 支持                                                                                                        | 待支持                             |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| 通用操作符                           | `DISTINCT`, `.` 用于属性访问                                                                             | `[]` 用于动态属性访问             |
| 数学操作符                           | `+`, `-`, `*`, `/`, `%`, `^`                                                                               |                                    |
| 比较操作符                           | `=`, `<>`, `<`, `>`, `<=`, `>=`, `IS NULL`, `IS NOT NULL`                                                 |                                    |
| 字符串特定比较操作符                 | `STARTS WITH`, `ENDS WITH`, `CONTAINS`, `REGEXP`                                                        |                                    |
| 布尔操作符                           | `AND`, `OR`, `XOR`, `NOT`                                                                                 |                                    |
| 字符串操作符                         | `+` 用于连接                                                                                               |                                    |
| 列表操作符                           | `+` 用于连接，`IN` 用于检查元素在列表中的存在，`[]` 用于访问元素(s)                                     |                                    |
### 1.2. 通用操作符
- ✓ 使用 DISTINCT 操作符

```
MATCH (p:person) RETURN DISTINCT p.born
```

- ❏ 使用 `.` 操作符访问嵌套字面量映射的属性

```
WITH {person: {name: 'Anne', age: 25}} AS p
RETURN p.person.name
```

- ❏ 使用 `[]` 操作符对动态计算的属性键进行过滤

```
CREATE (a:Restaurant {name: 'Hungry Jo', rating_hygiene: 10, rating_food: 7}),
       (b:Restaurant {name: 'Buttercup Tea Rooms', rating_hygiene: 5, rating_food:6}),
       (c1:Category {name: 'hygiene'}), (c2:Category {name: 'food'})
```

```
MATCH (restaurant:Restaurant), (category:Category)
WHERE restaurant["rating_" + category.name] > 6
RETURN DISTINCT restaurant.name
```
### 1.3. 数学运算符
- ✓ 使用指数运算符 `^`

```
WITH 2 AS number, 3 AS exponent
RETURN number ^ exponent AS result
```

- ✓ 使用一元减法运算符 `-`

```
WITH -3 AS a, 4 AS b
RETURN b - a AS result
```

### 1.4. 比较运算符
- ✓ 比较两个数字

```
WITH 4 AS one, 3 AS two
RETURN one > two AS result
```
输出:
### 1.5.字符串特定比较运算符
- ✓ 使用 STARTS WITH 过滤名称

```
WITH ['John', 'Mark', 'Jonathan', 'Bill'] AS somenames
UNWIND somenames AS names
WITH names AS candidate
WHERE candidate STARTS WITH 'Jo'
RETURN candidate
```

- ✓ 使用 REGEXP 过滤名称

```
WITH ['John', 'Mark', 'Jonathan', 'Bill'] AS somenames
UNWIND somenames AS names
WITH names AS candidate
WHERE candidate REGEXP 'Jo.*n'
RETURN candidate
```
输出：
### 1.6.布尔运算符
- ✓ 使用布尔运算符过滤数字

```
WITH [2, 4, 7, 9, 12] AS numberlist
UNWIND numberlist AS number
WITH number
WHERE number = 4 OR (number > 6 AND number < 10)
RETURN number
```
输出：
### 1.7. 字符串操作符
字符串操作符包括：

- ✓ 连接字符串: `+`
输出:
### 1.8. 列表运算符
- ✓ 使用 + 连接两个列表

```
RETURN [1,2,3,4,5]+[6,7] AS myList
```

- ✓ 使用 IN 检查一个数字是否在列表中

```
WITH [2, 3, 4, 5] AS numberlist
UNWIND numberlist AS number
WITH number
WHERE number IN [2, 3, 8]
RETURN number
```

- ✓ 使用 [] 运算符访问列表中的元素

```
WITH ['Anne', 'John', 'Bill', 'Diane', 'Eve'] AS names
RETURN names[1..3] AS result
```
## 2.子句
### 2.1.摘要
Clauses支持进度一览：

| 类别                    | 语法                                  | 备注   |
| ----------------------- | ------------------------------------- | ------ |
| 阅读子句                | MATCH                                 | 支持   |
|                         | OPTIONAL MATCH                        | 支持   |
|                         | MANDATORY MATCH                       | 待支持 |
| 投影子句                | RETURN … [AS]                        | 支持   |
|                         | WITH … [AS]                          | 支持   |
|                         | UNWIND … [AS]                        | 支持   |
| 阅读子子句              | WHERE                                 | 支持   |
|                         | ORDER BY [ASC[ENDING] / DESC[ENDING]] | 支持   |
|                         | SKIP                                  | 支持   |
|                         | LIMIT                                 | 支持   |
| 写入子句                | CREATE                                | 支持   |
|                         | DELETE                                | 支持   |
|                         | DETACH DELETE                         | 支持   |
|                         | SET                                   | 支持   |
|                         | REMOVE                                | 支持   |
| 阅读/写入子句          | MERGE                                 | 支持   |
|                         | CALL […YIELD]                        | 支持   |
| 集合操作                | UNION                                 | 待支持 |
|                         | UNION ALL                             | 支持   |
### 2.2.匹配
- 基本节点查找

  - ✓ 获取所有节点

  ```
  MATCH (n)
  RETURN n
  ```

  - ✓ 获取具有标签的所有节点

  ```
  MATCH (movie:movie)
  RETURN movie.title
  ```

  - ✓ 相关节点

  ```
  MATCH (person {name: 'Laurence Fishburne'})-[]-(movie)
  RETURN movie.title
  ```

  - ✓ 带标签的匹配

  ```
  MATCH (:person {name: 'Laurence Fishburne'})-[]-(movie:movie)
  RETURN movie.title
  ```

- 关系基础

  - ✓ 输出关系

  ```
  MATCH (:person {name: 'Laurence Fishburne'})-[]->(movie)
  RETURN movie.title
  ```

  - ✓ 有向关系和变量

  ```
  MATCH (:person {name: 'Laurence Fishburne'})-[r]->(movie)
  RETURN type(r)
  ```

  - ✓ 根据关系类型匹配

  ```
  MATCH (matrix:movie {title: 'The Matrix'})<-[:acted_in]-(actor)
  RETURN actor.name
  ```

  - ✓ 根据多个关系类型匹配

  ```
  MATCH (matrix {title: 'The Matrix'})<-[:acted_in|:directed]-(person)
  RETURN person.name
  ```

  - ✓ 根据关系类型匹配并使用变量

  ```
  MATCH (matrix {title: 'The Matrix'})<-[r:acted_in]-(actor)
  RETURN r.role
  ```

- 深入了解关系

  - ❏ 关系类型包含非常规字符

  ```
  MATCH (n {name: 'Rob Reiner'})-[r:`TYPE WITH SPACE`]->()
  RETURN type(r)
  ```

  - ✓ 多重关系

  ```
  MATCH (laurence {name: 'Laurence Fishburne'})-[:acted_in]->(movie)<-[:directed]-(director)
  RETURN movie.title, director.name
  ```

  - ✓ 变长关系

  ```
  MATCH (laurence {name: 'Laurence Fishburne'})-[:acted_in*1..3]-(movie:movie)
  RETURN movie.title
  ```

  - ✓ 在变长关系中使用关系变量

  ```
  MATCH p = (laurence {name: 'Laurence Fishburne'})-[:acted_in*2]-(co_actor)
  RETURN p
  ```

  - ❏ 在变长路径上根据属性进行匹配

  ```
  MATCH p = (charlie:person)-[* {blocked:false}]-(martin:person)
  WHERE charlie.name = 'Charlie Sheen' AND martin.name = 'Martin Sheen'
  RETURN p
  ```

  - ✓ 零长度路径

  ```
  MATCH (matrix:movie {title: 'The Matrix'})-[*0..1]-(x)
  RETURN x
  ```

  - ✓ 命名路径

  ```
  MATCH p = (michael {name: 'Michael Douglas'})-[]->() RETURN p
  ```

  - ✓ 基于约束关系的匹配

  ```
  MATCH (a)-[r]->(b)
  WHERE euid(r)="0_3937_0_0_0"
  RETURN a,b
  ```

- 最短路径

  - ✓ 单个最短路径

  ```
  MATCH (martin:person {name: 'Carrie-Anne Moss'}), (laurence:person {name: 'Laurence Fishburne'})
  CALL algo.shortestPath(martin, laurence) YIELD nodeCount,totalCost,path RETURN nodeCount,totalCost,path
  ```

  - ✓ 所有最短路径

  ```
  MATCH (martin:person {name: 'Carrie-Anne Moss'}), (laurence:person {name: 'Laurence Fishburne'}) WITH martin, laurence
  CALL algo.allShortestPaths(martin, laurence) YIELD nodeIds,relationshipIds,cost RETURN nodeIds,relationshipIds,cost
  ```

- 根据 ID 获取节点或关系

  - ✓ 根据 ID 获取节点

  ```
  MATCH (n)
  WHERE id(n)= 0
  RETURN n
  ```

  - ✓ 根据 ID 获取关系

  ```
  MATCH ()-[r]->()
  WHERE euid(r) = "0_3937_0_0_0"
  RETURN r
  ```

  - ✓ 根据 ID 获取多个节点

  ```
  MATCH (n)
  WHERE id(n) IN [0, 3, 5]
  RETURN n
  ```
### 2.3.返回
- ✓ 返回节点

```
MATCH (n {name: 'Carrie-Anne Moss'}) RETURN n
```

- ✓ 返回关系

```
MATCH (n {name: 'Carrie-Anne Moss'})-[r:acted_in]->(c)
RETURN r
```

- ✓ 返回属性

```
MATCH (n {name: 'Carrie-Anne Moss'}) RETURN n.born
```

- ❏ 返回所有元素

```
MATCH p = (a {name: 'A'})-[r]->(b)
RETURN *
```

- ❏ 带有不常见字符的变量

```
MATCH (`This isn\'t a common variable`)
WHERE `This isn\'t a common variable`.name = 'A'
RETURN `This isn\'t a common variable`.happy
```

- ✓ 字段别名

```
MATCH (a {name: 'Carrie-Anne Moss'})
RETURN a.born AS SomethingTotallyDifferent
```

- ✓ Optional properties

```
MATCH (n)
RETURN n.age
```

- ❏ 其他表达式

```
MATCH (a {name: 'Carrie-Anne Moss'})
RETURN a.born > 1900, "I'm a literal", (a)-[]->()
```

`(a)-[]->()`  不支持。

- ✓ 唯一结果

```
MATCH (a {name: 'Carrie-Anne Moss'})-[]->(b)
RETURN DISTINCT b
```
### 2.4. 哪里
- 基本用法
  - ✓ 布尔运算

  ```
  MATCH (n)
  WHERE n.name = 'Laurence Fishburne' XOR (n.born > 1965 AND n.name = 'Carrie-Anne Moss')
  RETURN n.name, n.born
  ```

  - ✓ 按节点标签过滤

  ```
  MATCH (n)
  WHERE n:person
  RETURN n.name, n.born
  ```

  - ✓ 按节点属性过滤

  ```
  MATCH (n)
  WHERE n.born > 2000
  RETURN n.name, n.born
  ```

  - ✓ 按关系属性过滤

  ```
  MATCH (n)-[k:acted_in]->(f)
  WHERE k.role = "Trinity"
  RETURN f.title
  ```

  - ❏ 按动态计算的属性过滤

  ```
  WITH 'AGE' AS propname
  MATCH (n)
  WHERE n[toLower(propname)]< 30
  RETURN n.name, n.age
  ```

  - ✓ 属性存在性检查

  ```
  MATCH (n)
  WHERE exists(n.born)
  RETURN n.name, n.born
  ```

- 字符串匹配
  - ✓ 匹配字符串的开头

  ```
  MATCH (n)
  WHERE n.name STARTS WITH 'Pet'
  RETURN n.name, n.born
  ```

  - ✓ 匹配字符串的结尾

  ```
  MATCH (n)
  WHERE n.name ENDS WITH 'ter'
  RETURN n.name, n.born
  ```

  - ✓ 匹配字符串中的任意位置

  ```
  MATCH (n)
  WHERE n.name CONTAINS 'ete'
  RETURN n.name, n.born
  ```

  - ✓ 字符串匹配否定

  ```
  MATCH (n)
  WHERE NOT n.name ENDS WITH 's'
  RETURN n.name, n.born
  ```

- 在 `WHERE` 中使用路径模式
  - ❏ 按模式过滤

  ```
  MATCH (tobias {name: 'Tobias'}), (others)
  WHERE others.name IN ['Andres', 'Peter'] AND (tobias)<-[]-(others)
  RETURN others.name, others.age
  ```

  - ❏ 使用 NOT 按模式过滤

  ```
  MATCH (persons), (peter {name: 'Peter'})
  WHERE NOT (persons)-[]->(peter)
  RETURN persons.name, persons.age
  ```

  - ❏ 按含属性的模式过滤

  ```
  MATCH (n)
  WHERE (n)-[:KNOWS]-({name: 'Tobias'})
  RETURN n.name, n.age
  ```

  - ✓ 按关系类型过滤

  ```
  MATCH (n)-[r]->()
  WHERE n.name='Laurence Fishburne' AND type(r) STARTS WITH 'ac'
  RETURN type(r), r.role
  ```

- 列表
  - ✓ IN 操作符

  ```
  MATCH (a)
  WHERE a.name IN ['Laurence Fishburne', 'Tobias']
  RETURN a.name, a.born
  ```

- 缺失的属性和值
  - ✓ 如果缺失属性则默认为 false

  ```
  MATCH (n)
  WHERE n.belt = 'white'
  RETURN n.name, n.age, n.belt
  ```

  - ✓ 如果缺失属性则默认为 true

  ```
  MATCH (n)
  WHERE n.belt = 'white' OR n.belt IS NULL RETURN n.name, n.age, n.belt
  ORDER BY n.name
  ```

  - ✓ 按 null 过滤

  ```
  MATCH (person)
  WHERE person.name = 'Peter' AND person.belt IS NULL RETURN person.name, person.age,
  person.belt
  ```

- 使用范围
  - ✓ 简单范围

  ```
  MATCH (a)
  WHERE a.name >= 'Peter'
  RETURN a.name, a.born
  ```

  - ✓ 复合范围

  ```
  MATCH (a)
  WHERE a.name > 'Andres' AND a.name < 'Tobias'
  RETURN a.name, a.born
  ```
### 2.5.SKIP
- ✓ 跳过前三条记录

```
MATCH (n:person)
RETURN n.name
ORDER BY n.name
SKIP 3
```

- ✓ 返回中间两条记录

```
MATCH (n:person)
RETURN n.name
ORDER BY n.name
SKIP 1
LIMIT 2
```

- ❏ 使用带有 SKIP 的表达式来返回记录的子集

```
MATCH (n:person)
RETURN n.name
ORDER BY n.name
SKIP toInteger(3*rand())+ 1
```
Output:
### 2.6.限制
- ✓ 返回记录的一个子集

```
MATCH (n:person)
RETURN n.name
LIMIT 3
```

- ❏ 使用带LIMIT的表达式返回记录的一个子集

```
MATCH (n:person)
RETURN n.name
LIMIT toInteger(3 * rand()) + 1
```
输出：
### 2.7.CREATE
- 创建节点

> **注意**
> TuGraph不支持创建空的节点，也不支持多标签。

  - ☒ 创建单个节点

  ```
  CREATE (n)
  ```

  - ☒ 创建多个节点

  ```
  CREATE (n), (m)
  ```

  - ☒ 创建一个带标签的节点

  ```
  CREATE (n:person)
  ```

  - ☒ 创建一个带多个标签的节点

  ```
  CREATE (n:Person:Swedish)
  ```

  - ✓ 创建节点并添加标签和属性

  ```
  CREATE (n:person {id:2001, name: 'Andres'})
  ```

  - ✓ 返回创建的节点

  ```
  CREATE (n:person {id:2002, name: 'Andres'})
  RETURN n
  ```

- 创建关系
  - ✓ 在两个节点之间创建关系

  ```
  MATCH (n:person), (m:movie)
  WHERE n.name = 'Jada Pinkett Smith' AND m.title = 'The Matrix'
  CREATE (n)-[r:write]->(m)
  ```

  - ✓ 创建关系并设置属性

  ```
  MATCH (n:person), (m:movie)
  WHERE n.name = 'Jada Pinkett Smith' AND m.title = 'The Matrix'
  CREATE (n)-[r:acted_in{role: 'Trinity'}]->(m)
  ```

  - ❏ 创建一个完整路径

  ```
  CREATE p = (andres:person {id: 2005, name:'Andres'})-[:acted_in {role: 'Trinity'}]->
  (m:movie {id: 2006})<-[:acted_in {role: 'Trinity'}]-(michael {id: 2006, name:'Michael'})
  RETURN p
  ```

- 使用参数创建
  - ❏ 创建一个包含属性参数的节点

  ```
  CREATE (n:Person $props)
  RETURN n
  ```

  - ☒ 使用属性参数创建多个节点

  ```
  UNWIND $props AS map
  CREATE (n)
  SET n = map
  ```
  不能创建没有标签的顶点。
### 2.8.CALL[…YIELD]
- ✓ 使用 CALL 调用一个过程

```
CALL db.vertexLabels
```

- ✓ 查看一个过程的签名

```
CALL dbms.procedures() YIELD name, signature
RETURN signature
```

- ❏ 使用引用的命名空间和名称调用一个过程

```
CALL `db`.`vertexLabels`
```

- ✓ 以字面参数调用一个过程

```
CALL org.opencypher.procedure.example.addNodeToIndex('users', 0, 'name')
```

- ❏ 使用参数参数调用一个过程

```
CALL org.opencypher.procedure.example.addNodeToIndex($indexName,$node,$propKey)
```

- ❏ 使用混合字面和参数参数调用一个过程

```
CALL org.opencypher.procedure.example.addNodeToIndex('users', $node, 'name')
```

- ✓ 使用字面和默认参数调用一个过程

```
CALL org.opencypher.procedure.example.addNodeToIndex('users', 0)
```

- ✓ 在复杂查询中使用 CALL…YIELD 调用一个过程

```
CALL db.vertexLabels() YIELD label
RETURN count(label) AS numLabels
```

- ❏ 调用一个过程并过滤其结果

```
CALL db.vertexLabels() YIELD label
WHERE label CONTAINS 'User'
RETURN count(label) AS numLabels
```

- ❏ 在复杂查询中调用一个过程并重命名其输出

```
CALL db.propertyKeys() YIELD propertyKey AS prop
MATCH (n)
WHERE n[prop] IS NOT NULL RETURN prop, count(n) AS numNodes
```
### 2.9.联合
- ✓ 组合两个查询并保留重复项

```
MATCH (n:person)
RETURN n.name AS name
UNION ALL MATCH (n:movie)
RETURN n.title AS name
```

- ❏ 组合两个查询并移除重复项

```
MATCH (n:Actor)
RETURN n.name AS name
UNION
MATCH (n:Movie)
RETURN n.title AS name
```
## 3.功能
### 3.1.功能完整列表
| 种类                   | 功能               | 备注                      |
| ---------------------- |------------------| ------------------------- |
| 谓词函数              | exists()         |                           |
|                        | all()            | 不支持                    |
|                        | any()            | 不支持                    |
|                        | single()         | 不支持                    |
|                        | none()           | 不支持                    |
| 标量函数              | id()             |                           |
|                        | euid()           |                           |
|                        | properties()     |                           |
|                        | head()           |                           |
|                        | last()           |                           |
|                        | toBoolean()      |                           |
|                        | toFloat()        |                           |
|                        | toInteger()      |                           |
|                        | toString()       |                           |
|                        | type()           |                           |
|                        | startnode()      |                           |
|                        | endnode()        |                           |
|                        | size()           |                           |
|                        | length()         |                           |
|                        | substring()      |                           |
|                        | concat()         |                           |
|                        | label()          | OpenCypher扩展方法      |
| 聚合函数              | avg()            |                           |
|                        | collect()        |                           |
|                        | count()          |                           |
|                        | max()            |                           |
|                        | min()            |                           |
|                        | percentileCont() |                           |
|                        | percentileDisc() |                           |
|                        | stDev()          |                           |
|                        | stDevP()         |                           |
|                        | variance()       |                           |
|                        | varianceP()      |                           |
|                        | sum()            |                           |
| 列表函数              | keys()           |                           |
|                        | labels()         | 返回结果有且只有一个label |
|                        | nodes()          |                           |
|                        | range()          |                           |
|                        | subscript()      | 不支持                    |
| 数学函数              | abs()            |                           |
|                        | ceil()           |                           |
|                        | floor()          |                           |
|                        | rand()           |                           |
|                        | round()          |                           |
|                        | sign()           |                           |
| 字符串函数            | /                |                           |
### 3.2.谓词函数
- exists()
  判断一个顶点或边是否存在该字段。
  **范围：** 整个实例。
  **示例输入：**

```
MATCH (n)
WHERE exists(n.born)
RETURN n.name, n.born
```

**示例输出：**

| exists(name) |
| ------------ |
| true         |
输出：
### 3.3.标量函数
- id()
  获取顶点的ID。
  **作用域:** 整个实例。
  **示例输入:**

```
MATCH (a)
RETURN id(a)
```

**示例输出:**

| vid |
| --- |
| 1   |
| 2   |
| ... |

- properties()
  获取一个包含节点或关系所有属性的映射。
  **作用域:** 整个实例。
  **示例输入:**

```
MATCH (n:person {name: 'Laurence Fishburne'})
RETURN n
```

- head()
  获取列表的第一个元素。
  **作用域:** 整个实例。
  **示例输入:**

```
WITH ['one','two','three'] AS coll RETURN coll, head(coll)
```

**示例输出:**

| coll                  | head(coll)    |
| --------------------- | ------------- |
| ["one","two","three"] | "one"         |

- last()
  获取列表的最后一个元素。
  **作用域:** 整个实例。
  **示例输入:**

```
WITH ['one','two','three'] AS coll RETURN coll, last(coll)
```

**示例输出:**

| coll                  | last(coll)    |
| --------------------- | ------------- |
| ["one","two","three"] | "three"       |

- toFloat()
  将整数或字符串值转换为浮点数。
  **作用域:** 整个实例。
  **示例输入:**

```
RETURN toFloat('11.5')
```

**示例输出:**

| float |
| ----- |
| 11.5  |

- toInteger()
  将浮点或字符串值转换为整数值。
  **作用域:** 整个实例。
  **示例输入:**

```
RETURN toInteger('2.3') AS integer
```

**示例输出:**

| integer |
| ------- |
| 2       |

- toString()
  将整数、浮点数、布尔值转换为字符串。
  **作用域:** 整个实例。
  **示例输入:**

```
RETURN toString(2.3)
```

- type()
  获取关系类型的字符串表示。
  **作用域:** 整个实例。
  **示例输入:**

```
MATCH (n)-[r]->()
WHERE n.name = 'Laurence Fishburne'
RETURN type(r)
```

**示例输出:**

| type     |
| -------- |
| acted_in |
| acted_in |
### 3.4. 聚合函数
- avg()
  返回一组数值的平均值。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n:person)
RETURN avg(n.born)
```

**示例输出：**

| avg(n.born)        |
| ------------------ |
| 1869.2661654135338 |

- collect()
  返回一个包含表达式返回值的列表。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n:person)
RETURN collect(n.born)
```

**示例输出：**

| collect(n.born) |
| --------------- |
| [1967,...]      |

- count()
  返回值或记录的数量。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n {name: 'Laurence Fishburne'})-[]->(x)
RETURN labels(n), n.born, count(*)
```

**示例输出：**

| labels(n)  | n.born | count(*) |
| ---------- | ------ | -------- |
| ["person"] | 1961   | 3        |

- max()
  返回一组值中的最大值。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n:person)
RETURN max(n.born)
```

**示例输出：**

| max(n.born) |
| ----------- |
| 2003        |

- min()
  返回一组值中的最小值。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n:person)
RETURN min(n.born)
```

**示例输出：**

| min(n.born) |
| ----------- |
| 1000        |

- percentileCont()
  使用线性插值返回一组中的一个值的百分位。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n:person)
RETURN percentileCont(n.born, 0.4)
```

**示例输出：**

| percentileCont(n.born, 0.4) |
| --------------------------- |
| 1953                        |

- percentileDisc()
  使用四舍五入方法返回一组中给定百分位的最近值。
  **作用范围：** 整个实例。
  **输出：** 给定值在一组中的百分位。
  **示例输入：**

```
MATCH (n:person)
RETURN percentileDisc(n.born, 0.5)
```

**示例输出：**

| percentileDisc(n.age, 0.5) |
| -------------------------- |
| 1959                       |

- stDev()
  返回在一组对总体样本的给定值的标准差。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n)
RETURN stDev(n.born)
```

**示例输出：**

| stDev(n.born)      |
| ------------------ |
| 279.53117993401725 |

- stDevP()
  返回在一组对整个总体的给定值的标准差。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n)
RETURN stDevP(n.born)
```

**示例输出：**

| stDevP(n.born)     |
| ------------------ |
| 279.3209270423399  |

- variance()
  返回在一组对总体样本的给定值的方差。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n)
RETURN variance(n.born)
```

**示例输出：**

| variance(n.age)   |
| ----------------- |
| 78137.68055530392 |

- varianceP()
  返回在一组对整个总体的给定值的方差。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n)
RETURN varianceP(n.born)
```

**示例输出：**

| varianceP(n.age)  |
| ----------------- |
| 78020.18028379219 |

- sum()
  返回一组数值的总和。
  **作用范围：** 整个实例。
  **示例输入：**

```
MATCH (n:person)
RETURN sum(n.born)
```

**示例输出：**

| sum(n.born) |
| ----------- |
| 1243062     |
### 3.5. 列表函数:
- keys()
  获取某个顶点的字段名称。
  **范围:** 整个实例。
  **示例输入:**

```
MATCH (a)
RETURN keys(a) LIMIT 1
```

**示例输出:**

| keys(a)               |
| --------------------- |
| ["name","age","eyes"] |

- labels()/label()
  返回包含节点、关系或映射的所有属性名称的字符串表示的列表。
  **范围:** 整个实例。
  **示例输入:**

```
MATCH (a)
RETURN labels(a) LIMIT 1
```

**示例输出:**

| labels                 |
| ---------------------- |
| ["Person"]             |

- nodes()

  获取一个路径的顶点ID。

  **范围:** 整个实例。

  **示例输入:**

  ```
  MATCH p = (from {name: 'Bob'})-[*1..]->(to {name: 'Alice"})
  RETURN nodes(p)
  ```

  **示例输出:**

  | nodes(p)       |
  | -------------- |
  | [0, 1, 10, 12] |
### 3.6.数学函数
- abs()
  获取某些数据的绝对值。
  **范围：** 整个实例。
  **示例输入：**

```
MATCH (a:person {name: 'Laurence Fishburne'}),(e:person {name: 'Carrie-Anne Moss'})
RETURN a.born, e.born, abs(a.born-e.born)
```

**示例输出：**

  | a.born | e.born | abs(a.born - e.born) |
  |--------|--------|-----------------------|
  | 1961   | 1967   | 6                     |

- ceil()
  返回大于或等于给定数字并且等于数学整数的最小浮点数。
  **范围：** 整个实例。
  **示例输入：**

```
RETURN ceil(0.1)
```

**示例输出：**

| ceil(0.1) |
| --------- |
| 1.0       |

- floor()
  获取小于或等于给定数字并且等于数学整数的最大浮点数。
  **范围：** 整个实例。
  **示例输入：**

```
RETURN floor(0.9)
```

**示例输出：**

| floor(0.9) |
| ---------- |
| 0.0        |

- round()
  返回四舍五入到最接近的整数的数字值。
  **范围：** 整个实例。
  **示例输入：**

```
RETURN round(3.141592)
```

**示例输出：**

| round |
| ----- |
| 3     |

- rand()
  返回一个随机浮点数，范围从 0（包含）到 1（不包含）。
  **范围：** 整个实例。
  **示例输入：**

```
RETURN rand()
```

**示例输出：**

| rand()             |
| ------------------ |
| 0.9797131960534085 |

- sign()
  获取给定数字的符号：如果数字为 0，则为 0；如果是任何负数，则为 -1；如果是任何正数，则为 1。
  **范围：** 整个实例。
  **示例输入：**

```
RETURN sign(-17), sign(0.1)
```

**示例输出：**

| sign(-17) | sign(0.1) |
| --------- | --------- |
| -1        | 1         |

TuGraph 查询语言与 OpenCypher 的不同点如下：

- 标签数量
  - TuGraph: 每个节点/关系必须有且仅有一个标签。因此如果没有标签会出现错误，如果有多个标签将选择第一个标签作为标签。
  - OpenCypher: 一个节点/关系可以有 0 到多个标签。
- 模式。
  - TuGraph: TuGraph 具有强模式。
  - OpenCypher: 无模式。
## 4.附录1. 语法扩充及不同
TuGraph查询语言与OpenCypher的不同点如下：

- 标签数量
  - TuGraph: 每个节点/关系必须具有一个且仅有一个标签。因此当没有标签时会出现错误，如果有多个标签，将选择第一个标签作为标签。
  - OpenCypher: 一个节点/关系可以拥有0到多个标签。
- 模式。
  - TuGraph: TuGraph具有强模式
  - OpenCypher: 无模式
## 5.附录2. 内置procedures列表
### 5.1.procedures样例
* dbms.procedures()
列出所有可用的过程。

  **范围：** 整个实例。

  **输出：** 一个包含 {`signature`, `name`} 的列表。

  **示例输入：**

  ```
  CALL dbms.procedures()
  ```

  **示例输出：**

  | signature                        | name         |
  | ---------------------------------------------------- | -------------------- |
  | db.vertexLabels() :: (label::STRING)               | db.vertexLabels        |
  | db.edgeLabels() :: (edgeLabels::STRING) | db.edgeLabels |
  | db.indexes() :: (index::LIST)            | db.indexes       |
  | ...                          | ...          |
输出：
* db.subgraph()
**范围：** 整个实例。

  **参数：**

  | 参数       | 参数类型       | 描述                                                             |
  | ---------- | -------------- | ----------------------------------------------------------------- |
  | vids       | 列表           | 顶点 ID 的列表                                                    |

  **输出：**

  获取一个包含所有节点和关系属性的 JSON。

  **示例输入：**

  ```
  CALL db.subgraph([3937,4126,4066,4010])
  ```

  **示例输出**

  | 子图       |
  | ---------- |
  | {"nodes":[{"identity":3937,"label":"movie","properties":{"duration":136,"id":1,"poster_image":"http://image.tmdb.org/t/p/w185/gynBNzwyaHKtXqlEKKLioNkjKgN.jpg","rated":"R","summary":"Thomas A. Anderson 是一个过着双重生活的人。白天他是一名普通的计算机程序员，夜晚则是一个恶意黑客，名叫 Neo，当他接触到传奇计算机黑客 Morpheus 时，发现自己成为了警察的目标，Morpheus 向他揭示了我们现实的惊人真相。","tagline":"欢迎来到真实世界。","title":"黑客帝国"}},{"identity":4010,"label":"user","properties":{"id":44,"login":"Howard"}},{"identity":4066,"label":"user","properties":{"id":202,"login":"Enoch"}},{"identity":4126,"label":"user","properties":{"id":464,"login":"Wilburn"}}],"relationships":[{"dst":4126,"forward":true,"identity":0,"label":"is_friend","label_id":3,"src":4010,"temporal_id":0},{"dst":4010,"forward":true,"identity":0,"label":"is_friend","label_id":3,"src":4066,"temporal_id":0},{"dst":4066,"forward":true,"identity":0,"label":"is_friend","label_id":3,"src":4126,"temporal_id":0}]} |
* db.vertexLabels()
列出所有可用的顶点标签。

  **范围:** 整个实例。

  **输出:** {`name`} 的列表。

  **示例输入:**

  ```
  CALL db.vertexLabels()
  ```
  **示例输出:**

    | 标签     |
    | -------- |
    | 类型     |
    | 关键词   |
    | 电影     |
    | ...      |
输出:
* db.edgeLabels()
列出所有可用的边标签。

  **范围：** 整个实例。

  **输出：** 一个 {边标签} 的列表。

  **示例输入：**

  ```
  CALL db.edgeLabels()
  ```

  **示例输出：**

  | relationshipType |
  | ---------------- |
  | acted_in     |
  | directed     |
  | ...          |
输出：
* db.createVertexLabel(label_name, primary_field, field_spec...)
创建一个顶点标签。

  **范围:** 整个实例。

  **参数:**

  | 参数         | 参数类型       | 描述                      |
  | ------------ | -------------- | ------------------------- |
  | label_name   | 字符串        | 顶点标签的名称             |
  | primary_field | 字符串       | 顶点标签的主字段           |
  | field_spec   | 列表          | 字段的规范                 |

   其中每个 `field_spec` 是一个字符串列表，格式为 `[field_name, field_type, true]`，其中 true 仅针对可选字段指定。

    **输出:** 如果成功，将返回成功消息。

    **示例输入:**

    ```
    CALL db.createVertexLabel('Person', 'id', 'id', 'int64', false, 'name', 'string', true)
    ```

    **示例输出:**

    ```
    添加标签 [Person]
    ```
* db.getLabelSchema(label_type, label_name)
获取子图中标签的 schema 定义。

  **范围:** 子图，如 REST 或 RPC 请求中的 `graph` 参数所指定。

  **参数:**

  | 参数         | 参数类型     | 描述                     |
  | ------------ | ------------ | ------------------------ |
  | label_type   | 字符串       | 'vertex' 或 'edge'       |
  | label_name   | 字符串       | 标签的名称               |

  **输出:** 标签规格的列表，其中每个元素是以下字段的列表：

  | 字段名称     | 字段类型     | 描述                     |
  | ------------ | ------------ | ------------------------ |
  | name         | 字符串       | 字段的名称               |
  | type         | 字符串       | 字段的类型               |
  | optional     | 布尔值       | 字段是否可选             |

  **示例输入:**

  ```
  CALL db.getLabelSchema('vertex', 'Person')
  ```

  **示例输出:**

  | name         | type   | optional |
  | ------------ | ------ | -------- |
  | id           | INT32  | false    |
  | born         | INT32  | true     |
  | name         | STRING | true     |
  | poster_image | STRING | true     |
* db.createLabel(label_type, label_name, extra, field_spec...)
创建一个顶点或边的标签。

  **参数:**

  | 参数        | 参数类型      | 描述                      |
  | ---------- | -------------- | ------------------------- |
  | label_type | 字符串     | 'vertex' 或 'edge' |
  | label_name | 字符串     | 标签的名称                |
  | extra      | 字符串      | 对于边，表示约束；对于顶点，表示主要属性 |
  | field_spec | 列表       | 字段规格                  |

    其中每个 `field_spec` 是一个字符串列表，形式为 `[field_name, field_type, optional]`。
    对于边，`extra` 应该是一个 JSON 数组字符串，像这样 `[["label1","label2"], ["label3","label4"]]`，如果边没有约束，给一个空的 JSON 数组，像这样 `[]`。

  **输出:**

  如果成功，它返回一个成功消息。

  **示例输入:**

  ```
  CALL db.createLabel('vertex', 'new_label', 'id', ['id','int32',false], ['name','string', true]);
  CALL db.createLabel('edge', 'new_edge', '[["id1","id2"]]', ['id','int32',false], ['name', 'string', true]);
  ```

  **示例输出:**

  ```
  顶点标签 [new_label] 成功添加.
  ```
* db.deleteLabel(label_type, label_name)
删除一个顶点或边的标签。

  **参数:**

  | 参数        | 参数类型  | 描述                          |
  | ---------- | -------------- | ------------------------- |
  | label_type | string     | 可以是 'vertex' 或 'edge' |
  | label_name | string     | 标签的名称                  |

  **输出:**

  | 字段名称   | 字段类型  | 描述                           |
  | ---------- | ---------- | ------------------------------ |
  | affected   | integer    | 被删除的顶点/边的数量        |

  **示例输入:**

  ```
  CALL db.deleteLabel('vertex', 'Person')
  ```

  **示例输出:**

  | affected |
  | -------- |
  | 1024     |
* db.alterLabelDelFields(label_type, label_name, field_names)
删除指定字段从标签中。

  **参数：**

  | 参数        | 参数类型       | 描述                          |
  | ----------- | --------------- | ----------------------------- |
  | label_type  | 字符串       | 可以是 'vertex' 或 'edge'      |
  | label_name  | 字符串       | 标签的名称                     |
  | field_names | 字符串列表    | 要删除的字段名称               |

  **输出：**

  | 字段名称   | 字段类型   | 描述                            |
  | ---------- | ---------- | -------------------------------- |
  | affected   | 整数      | 修改的顶点/边的数量              |

  **示例输入：**

  ```
  CALL db.alterLabelDelFields('vertex', 'Person', ['name', 'image'])
  ```

  **示例输出：**

  | affected |
  | -------- |
  | 1024     |
* db.alterLabelAddFields(label_type, label_name, field_value_spec...)
向标签添加指定字段。

  **参数:**

  | 参数         | 参数类型    | 描述                       |
  | ------------ | ----------- | -------------------------- |
  | label_type   | 字符串      | 可以是 'vertex' 或 'edge'  |
  | label_name   | 字符串      | 标签的名称                 |
  | field_value_spec | 列表    | 字段的规范                 |

    其中每个 `field_value_spec` 是一个字符串列表，形式为 `[field_name, field_type, field_value, optional]`，其中：`field_value` 是字段的默认值。

  **输出:**

  | 字段名称     | 字段类型    | 描述                         |
  | ------------ | ----------- | ---------------------------- |
  | affected     | 整数        | 修改的顶点/边的数量         |

  **示例输入:**

  ```
  CALL db.alterLabelAddFields(
  'vertex',
  'new_label',
  ['birth_date', DATE, '', true],
  ['img', BLOB, '', true])
  ```

  **示例输出:**

  | affected     |
  | ------------ |
  | 1024         |
* db.alterLabelModFields(label_type, label_name, field_spec...)
修改标签中的指定字段。

  **参数:**

  | 参数        | 参数类型     | 描述                     |
  | ---------- | -------------- | ------------------------- |
  | label_type | 字符串       | 'vertex' 或 'edge'       |
  | label_name | 字符串       | 标签的名称               |
  | field_spec | 列表         | 字段的规范               |

    其中每个 `field_spec` 是一个字符串列表，格式为 `[field_name, field_type, optional]`。目标字段应该存在。

  **输出:**

  | 字段名称  | 字段类型     | 描述                           |
  | ---------- | ---------- | ------------------------------- |
  | affected   | 整数       | 修改的顶点/边的数量           |

  **示例输入:**

    ```
    CALL db.alterLabelModFields(
    'vertex',
    'new_label',
    ['birth_date', DATETIME, true],
    ['gender', BOOL, true])
    ```

  **示例输出:**

  | affected |
  | -------- |
  | 1024     |
* db.createEdgeLabel( label_name, field_spec...)
创建一个边标签。

  **参数:**

  | 参数         | 参数类型        | 描述                       |
  | ------------ | --------------- | ------------------------- |
  | label_name   | 字符串          | 标签的名称                 |
  | edge_constraints | 字符串      | 边的约束                   |
  | field_spec  | 列表            | 字段的规范                   |

  其中每个 `field_spec` 是一个字符串列表，格式为 `[field_name, field_type, optional]`，其中 optional 被指定为 true，仅用于可选字段。

  `edge_constraints` 是一个 json 数组字符串，该参数限制边的起始和结束顶点的组合，例如：`'[["vertex_label1","vertex_label2"],["vertex_label3","vertex_label4"]]'`，这限制了边的方向只能是从 `vertex_label1` 到 `vertex_label2` 或从 `vertex_label3` 到 `vertex_label4`。如果你不想有任何约束，给一个空数组字符串，如 `'[]'`。

  **输出:**

    如果成功，将返回成功消息。

   **示例输入:**

  ```
  CALL db.createEdgeLabel('KNOWS', '[]', 'name', 'int32', true)
  ```

  **示例输出:**

  ```
  添加类型 [KNOWS]
  ```
* db.addIndex(label_name, field_name, unique)
在某个顶点标签的某个字段上创建索引。

  **参数:**

  | 参数         | 参数类型   | 描述                           |
  | ------------ | ---------- | ------------------------------ |
  | label_name   | 字符串     | 标签的名称                     |
  | field_name   | 字符串     | 字段的规范                     |
  | unique       | 布尔值     | 指定索引是否唯一               |

   **输出:**

    如果成功，它将返回一条成功消息。

   **示例输入:**

  ```
  CALL db.addIndex('Person', 'id', true)
  ```

   **示例输出:**

  ```
  Added index [Person:id]
  ```
* db.addEdgeIndex(label_name, field_name, unique, pair_unique)  
在某个边标签的某个字段上创建索引。

  **参数：**

  | 参数         | 参数类型     | 描述                        |
  | ------------ | ------------ | --------------------------- |
  | label_name   | 字符串      | 标签的名称                  |
  | field_name   | 字符串      | 字段的规范                  |
  | unique       | 布尔值      | 指定索引是否唯一            |
  | pair_unique   | 布尔值      | 指定索引是否为成对唯一      |

  **输出：**

  如果成功，将返回成功消息。

  **示例输入：**

  ```
  CALL db.addEdgeIndex('BornIn', 'id', true, false)
  ```

  **示例输出：**

  ```
  添加索引 [BornIn:id]
  ```
* dbms.security.changePassword(current_password ,new_password)
更改当前用户的密码。

  **参数：**

  | 参数            | 参数类型      | 描述                  |
  | ---------------- | -------------- | -------------------- |
  | current_password | 字符串        | 当前密码             |
  | new_password     | 字符串        | 新密码               |

  **输出：**

    如果成功，将返回成功消息。

   **示例输入：**

  ```
  CALL dbms.security.changePassword('73@TuGraph','admin')
  ```

   **示例输出：**

  ```
  true
  ```
* dbms.security.changeUserPassword(用户名称, 新密码)
更改当前用户的密码。

  **参数:**

  | 参数          | 参数类型     | 描述           |
  | ------------ | ------------ | -------------- |
  | 用户名称     | 字符串       | 用户的名称     |
  | 新密码       | 字符串       | 新密码         |

  **输出:**

    如果成功，它将返回成功消息。

   **示例输入:**

  ```
  CALL dbms.security.changeUserPassword('quest','73@TuGraph')
  ```

  **示例输出:**

  ```
  true
  ```
* dbms.security.createUser(user_name, password)
在此图形数据库中创建新用户。

  **参数:**

  | 参数       | 参数类型       | 描述                   |
  | ---------- | -------------- | ---------------------- |
  | user_name  | 字符串       | 新用户的名称          |
  | password   | 字符串       | 新用户的密码          |
  
   **输出:**
  
    如果成功，它将返回一条成功消息。
  
   **示例输入:**
  
  ```
  CALL dbms.security.createUser('quest',"admin")
  ```
  
   **示例输出:**
  
  ```
  true
  ```
* dbms.security.deleteUser(user_name)  
在此图形数据库中删除用户。

  **参数：**

  | 参数       | 参数类型   | 描述                         |
  | ---------- | ---------- | ---------------------------- |
  | user_name  | 字符串    | 要删除的用户名               |

  **输出：**

    如果成功，返回成功消息。

  **示例输入：**

  ```
  CALL dbms.security.deleteUser('quest')
  ```

  **示例输出：**

  ```
  true
  ```
* dbms.security.listUsers()
获取图数据库中所有用户的名称。

  **输出：**

   用户名列表，每个元素是以下字段的列表：

  | 参数        | 参数类型   | 描述                       |
  | ----------- | ---------- | -------------------------- |
  | user.name   | 字符串      | 用户名                     |
  | is.admin    | 布尔值     | 此用户的权限               |

   **示例输入：**

  ```
  CALL dbms.security.listUsers()
  ```

  **示例输出：**

  | user.name | is.admin |
  | --------- | -------- |
  | admin     | true     |
  | ...       |  ...     |  
输出：
* dbms.security.showCurrentUser()
获取当前用户的名称。

  **输出：**

    用户名列表，其中每个元素是以下字段的列表：

  | 参数       | 参数类型      | 描述                   |
  | ---------- | -------------- | --------------------- |
  | user.user  | 字符串        | 当前用户名             |

  **示例输入：**

  ```
  CALL dbms.security.showCurrentUser()
  ```

  **示例输出：**

  | user.name |
  | --------- |
  | admin     |
输出：
* dbms.security.listAllowedHosts()
获取允许的IP列表。

  **输出：**

   允许的IP列表。

  **示例输入：**

  ```
  CALL dbms.security.listAllowedHosts()
  ```

  **示例输出：**

  | 主机       |
  | ------------ |
  | 192.168.1.22 |
  | ...      |
输出:
* dbms.security.deleteAllowedHosts(hosts)
从允许的 IP 列表中删除一些 IP。

  **输出：**

   被删除的 IP 数量。

   **示例输入：**

  ```
  CALL dbms.security.deleteAllowedHosts('192.168.1.22','192.168.1.23')
  ```

  **示例输出：**

  | success |
  | ------- |
  | 2       |
* dbms.security.addAllowedHosts(hosts)
将一些ip从允许的ip列表中添加。

  **输出：**

    已添加的ip数量。

  **示例输入：**

  ```
  CALL dbms.security.addAllowedHosts('192.168.1.22','192.168.1.23')
  ```

  **示例输出：**

  | 成功   |
  | ------ |
  | 2      |
* dbms.graph.createGraph(graph_name, description, max_size_GB)
在此图数据库中创建一个新的子图。

  **参数：**

  | 参数         | 参数类型  | 描述                          |
  | ------------ | --------- | ----------------------------- |
  | graph_name   | 字符串   | 新子图的名称                  |
  | description  | 字符串   | 新子图的描述                  |
  | max_size_GB  | 整数     | 子图容量的上限                |

   **输出：**

    如果成功，将返回 true。

   **示例输入：**

  ```
  CALL dbms.graph.createGraph('graph1', 'description', 2045)
  ```

  **示例输出：**

  | success |
  | ------- |
  | true    |
* dbms.graph.deleteGraph(graph_name)
在此图数据库中删除一个子图。

  | 参数        | 参数类型   | 描述                           |
  | ----------- | ---------- | ------------------------------ |
  | graph_name  | 字符串     | 要删除的子图的名称            |

  **输出：**

    如果成功，将返回 true。

  **示例输入：**

  ```
  CALL dbms.graph.deleteGraph('graph1')
  ```

   **示例输出：**

  | 成功      |
  | --------- |
  | true      |
* dbms.graph.modGraph(graph_name, config)
删除此图数据库中的子图。

  **参数：**

  | 参数         | 参数类型      | 描述                           |
  | ------------ | ------------- | ------------------------------ |
  | graph_name   | 字符串       | 要删除的子图的名称            |
  | config       | 映射         | 要修改的配置                  |

  **输出：**

    如果成功，将返回 true。

  **示例输入：**

  ```
  CALL dbms.graph.modGraph('graph1',{description:'this graph', max_size_GB:20})
  ```

  **示例输出：**

  | 成功     |
  | -------- |
  | true     |
* dbms.graph.listGraphs()
获取此图数据库中的所有子图。

  **输出：**

    一个 {子图和配置} 的列表。

  **示例输入：**

  ```
  CALL dbms.graph.listGraphs()
  ```

  **示例输出：**

  | graph.name | configuration                 |
  | ---------- | --------------------------------------------- |
  | default    | {"description":"","max_size_GB":1024}     |
  | graph1     | {"description":"this graph","max_size_GB":20} |
  | ...        |         ...                  |
* dbms.graph.listUserGraphs(user_name)
获取指定用户可以读取或写入的子图列表

  **输出:**

  一个 {子图和配置} 的列表。

  **示例输入:**

  ```
  CALL dbms.graph.listUserGraphs("test_user")
  ```

  **示例输出:**

  | graph.name | configuration                 |
  | ---------- | --------------------------------------------- |
  | default    | {"description":"","max_size_GB":1024}     |
  | graph1     | {"description":"this graph","max_size_GB":20} |
  | ...        |         ...                  |
输出:
* dbms.config.list()
获取此图数据库的配置。

  **输出：**

    一份 {配置} 的列表。

  **示例输入：**

  ```
  CALL dbms.config.list()
  ```

  **示例输出：**

  | 名称       | 值      |
  |-----------| ---------|
  | bind_host | 0.0.0.0 |
  | durable   | true    |
  | ...       |  ...    |
输出：
* dbms.config.update(updates)
获取此图数据库的一些配置。

  **输出：**

    如果成功，它将返回成功消息。

  **示例输入：**

  ```
  CALL dbms.config.update({
    enable_ip_check:false,
    durable:true,
    optimistic_txn:true,
    enable_audit_log:true})
  ```

  **示例输出：**

  ```
  更新成功。
  ```
* dbms.takeSnapshot()
在当前图形数据库上创建快照。

  **输出:**

    如果成功，它将返回快照的路径。

  **示例输入:**

  ```
  CALL dbms.takeSnapshot()
  ```

  **示例输出:**

  | 路径                                   |
  | ----------------------------------- |
  | log/db/snapshot/2020-07-20_17.20.03 |
* dbms.listBackupFiles()
获取备份文件的路径。

  **输出：**

    如果成功，它将返回快照的路径。

  **示例输入：**

  ```
  CALL dbms.listBackupFiles()
  ```

  **示例输出：**

  | path               |
  | -------------------------- |
  | tugraph/db/binlog/binlog_0 |
* algo.shortestPath(startNode, endNode, config)
获取两个顶点之间的最短路径之一。

  **参数:**

  | 参数       | 参数类型       | 描述                                      |
  | --------- | -------------- | ---------------------------------------------------------- |
  | startNode | Node       | 路径的源节点                              |
  | endNode   | Node       | 路径的目的节点                            |
  | config    | MAP        | 最短路径的筛选条件，格式为 {maxHops:3, relationshipQuery:'HAS_CHILD'} |

  **输出:**

    如果成功，它将返回一组最短路径的结果。

  **示例输入:**

  ```
  MATCH (n1 {name:'Hugo Weaving'}),(n2 {title:'The Matrix'})
  CALL algo.shortestPath(n1,n2) YIELD nodeCount,totalCost RETURN nodeCount,totalCost
  ```

  **示例输出:**

  | nodeCount | totalCost |
  | --------- | --------- |
  | 2     | 1     |
* algo.allShortestPaths(startNode, endNode, config))
获取备份文件的路径。

  **输出：**

    如果成功，它将返回快照的路径。

  **示例输入：**

  ```
  MATCH (n1 {name:'Hugo Weaving'}),(n2 {title:'The Matrix'})
  CALL algo.allShortestPaths(n1,n2) YIELD nodeIds,cost RETURN nodeIds,cost
  ```

  **示例输出：**

  | nodeIds | cost |
  | ------- | ---- |
  | [2,665] | 1    |
  | ...     |      |  
Output:
* algo.algo.native.extract(id, config)
获取一组顶点或边的字段值。

  **参数：**

  | 参数      | 参数类型 | 描述                                                   |
  | --------- | -------- | ------------------------------------------------------ |
  | id        | 任意     | 顶点或边的 id，id 必须是变量                          |
  | config    | 映射     | 这次提取顶点或边的配置                                |

  其中每个 `config` 是一个形式为 `{isNode:true, filed:'HAS_CHILD'}` 的映射，如果 `isNode` 被指定为 true，则 `id` 是一个顶点 id，否则它是一个边 id。

  **输出：**

    如果成功，它将返回指定字段的顶点或边的值的列表。

  **示例输入：**

  ```
  with [2,3] as vids CALL algo.native.extract(vids,{isNode:true, field:'id'})
  YIELD value  RETURN value
  ```

  **示例输出：**

  | value   |
  | ------- |
  | [4,5]   |

### 5.2.内置procedures完整列表
| 名称                                   | 描述                                   | 签名                                                                                                                                                                                 |
|---------------------------------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| db.subgraph                           | 列出点的子图                                | db.subgraph(vids::LIST) :: (subgraph::STRING)                                                                                                                                           |
| db.vertexLabels                       | 列出所有Vertex Label                      | db.vertexLabels() :: (label::STRING)                                                                                                                                                    |
| db.edgeLabels                         | 列出所有Edge Label                        | db.edgeLabels() :: (edgeLabels::STRING)                                                                                                                                                 |
| db.indexes                            | 列出所有索引                                | db.indexes() :: (label::STRING,field::STRING,label_type:STRING,unique::BOOLEAN,pair_unique::BOOLEAN)                                                                                    |
| db.listLabelIndexes                   | 列出所有与某个Label相关的索引                     | db.listLabelIndexes(label_name:STRING,label_type:STRING) :: (label::STRING,field::STRING,unique::BOOLEAN,pair_unique::BOOLEAN)                                                          |
| db.warmup                             | 预热数据                                  | db.warmup() :: (time_used::STRING)                                                                                                                                                      |
| db.createVertexLabel                  | 创建Vertex Label                        | db.createVertexLabel(label_name::STRING,field_specs::LIST) :: (::VOID)                                                                                                                  |
| db.createLabel                        | 创建Vertex/Edge Label                   | db.createLabel(label_type::STRING,label_name::STRING,extra::STRING,field_specs::LIST) :: ()                                                                                             |
| db.getLabelSchema                     | 列出label schema                        | db.getLabelSchema(label_type::STRING,label_name::STRING) :: (name::STRING,type::STRING,optional::BOOLEAN)                                                                               |
| db.getVertexSchema                    | 列出点的 schema                           | db.getVertexSchema(label::STRING) :: (schema::MAP)                                                                                                                                      |
| db.getEdgeSchema                      | 列出边的 schema                           | db.getEdgeSchema(label::STRING) :: (schema::MAP)                                                                                                                                        |
| db.deleteLabel                        | 删除Vertex/Edge Label                   | db.deleteLabel(label_type::STRING,label_name::STRING) :: (::VOID)                                                                                                                       |
| db.alterLabelDelFields                | 修改label删除属性                           | db.alterLabelDelFields(label_type::STRING,label_name::STRING,del_fields::LIST) :: (record_affected::INTEGER)                                                                            |
| db.alterLabelAddFields                | 修改label添加field                        | db.alterLabelAddFields(label_type::STRING,label_name::STRING,add_field_spec_values::LIST) :: (record_affected::INTEGER)                                                                 |
| db.alterLabelModFields                | 修改label field                         | db.alterLabelModFields(label_type::STRING,label_name::STRING,mod_field_specs::LIST) :: (record_affected::INTEGER)                                                                       |
| db.createEdgeLabel                    | 创建Edge Label                          | db.createEdgeLabel(type_name::STRING,field_specs::LIST) :: (::VOID)                                                                                                                     |
| db.addIndex                           | 创建索引                                  | db.addIndex(label_name::STRING,field_name::STRING,unique::BOOLEAN) :: (::VOID)                                                                                                          |
| db.addEdgeIndex                       | 创建索引                                  | db.addEdgeIndex(label_name::STRING,field_name::STRING,unique::BOOLEAN,pair_unique::BOOLEAN) :: (::VOID)                                                                                 |
| db.addVertexCompositeIndex            | 创建组合索引                                | db.addVertexCompositeIndex(label_name::STRING,field_names::LIST,unique::BOOLEAN) :: (::VOID)                                                                                            |
| db.deleteIndex                        | 删除索引                                  | db.deleteIndex(label_name::STRING,field_name::STRING) :: (::VOID)                                                                                                                       |
| db.deleteCompositeIndex               | 删除组合索引                                | db.deleteIndex(label_name::STRING,field_names::LIST) :: (::VOID)                                                                                                                        |
| db.backup                             | 备份数据                                  | db.backup(destination::STRING) :: ()                                                                                                                                                    |
| dbms.procedures                       | 列出所有procedures                        | dbms.procedures() :: (name::STRING,signature::STRING)                                                                                                                                   |
| dbms.security.changePassword          | 更改当前用户的密码                             | dbms.security.changePassword(current_password::STRING,new_password::STRING) :: (::VOID)                                                                                                 |
| dbms.security.changeUserPassword      | 更改指定用户的密码                             | dbms.security.changeUserPassword(user_name::STRING,new_password::STRING) :: (::VOID)                                                                                                    |
| dbms.security.createUser              | 创建用户                                  | dbms.security.createUser(user_name::STRING,password::STRING) :: (::VOID)                                                                                                                |
| dbms.security.deleteUser              | 删除用户                                  | dbms.security.deleteUser(user_name::STRING) :: (::VOID)                                                                                                                                 |
| dbms.security.listUsers               | 列出所有用户                                | dbms.security.listUsers() :: (user_name::STRING,user_info::MAP)                                                                                                                         |
| dbms.security.showCurrentUser         | 列出当前用户信息                              | dbms.security.showCurrentUser() :: (current_user::STRING)                                                                                                                               |
| dbms.security.getUserPermissions      | 列出指定用户的权限                             | dbms.security.getUserPermissions(user_name::STRING) :: (user_info::MAP)                                                                                                                 |
| dbms.graph.createGraph                | 创建子图                                  | dbms.graph.createGraph(graph_name::STRING, description::STRING, max_size_GB::INTEGER) :: (::VOID)                                                                                       |
| dbms.graph.modGraph                   | 修改子图属性                                | dbms.graph.modGraph(graph_name::STRING,config::MAP) :: (::VOID)                                                                                                                         |
| dbms.graph.deleteGraph                | 删除子图                                  | dbms.graph.deleteGraph(graph_name::STRING) :: (::VOID)                                                                                                                                  |
| dbms.graph.listGraphs                 | 列出所有子图                                | dbms.graph.listGraphs() :: (graph_name::STRING,configuration::MAP)                                                                                                                      |
| dbms.graph.getGraphInfo               | 列出指定子图的信息                             | dbms.graph.getGraphInfo(graph_name::STRING)::(graph_name::STRING,configuration::MAP)                                                                                                    |
| dbms.security.addAllowedHosts         | 添加ip到信任列表                             | dbms.security.addAllowedHosts(hosts::LIST) :: (num_new::INTEGER)                                                                                                                        |
| dbms.security.deleteAllowedHosts      | 从信任列表删除ip                             | dbms.security.deleteAllowedHosts(hosts::LIST) :: (record_affected::INTEGER)                                                                                                             |
| dbms.security.listAllowedHosts        | 列出信任列表中的主机ip                          | dbms.security.listAllowedHosts() :: (host::STRING)                                                                                                                                      |
| dbms.config.update                    | 更新TuGraph配置                           | dbms.config.update(updates::MAP) :: (message::STRING)                                                                                                                                   |
| dbms.config.list                      | 列出TuGraph配置                           | dbms.config.list() :: (name::STRING,value::ANY)                                                                                                                                         |
| algo.shortestPath                     | 查询两个点间的最短路径                           | algo.shortestPath(startNode::NODE,endNode::NODE,config::MAP) :: (nodeCount::INTEGER,totalCost::FLOAT)                                                                                   |
| algo.allShortestPaths                 | 查询两个点间的所有最短路径                         | algo.allShortestPaths(startNode::NODE,endNode::NODE,config::MAP) :: (nodeIds::LIST,relationshipIds::LIST,cost::LIST)                                                                    |
| algo.native.extract                   | 查询指定VertexId/EdgeUid（列表）指定field的值（列表） | algo.native.extract(id::ANY,config::MAP) :: (value::ANY)                                                                                                                                |
| db.flushDB                            | 刷新db                                  | db.flushDB() :: (::VOID)                                                                                                                                                                |
| dbms.security.listRoles               | 列出所有角色                                | dbms.security.listRoles() :: (role_name::STRING,role_info::MAP)                                                                                                                         |
| dbms.security.createRole              | 创建角色                                  | dbms.security.createRole(role_name::STRING,desc::STRING) :: (::VOID)                                                                                                                    |
| dbms.security.deleteRole              | 删除角色                                  | dbms.security.deleteRole(role_name::STRING) :: (::VOID)                                                                                                                                 |
| dbms.security.getRoleInfo             | 获取角色详细信息                              | dbms.security.getRoleInfo(role::STRING) :: (role_info::MAP)                                                                                                                             |
| dbms.security.disableRole             | 禁用/启用角色                               | dbms.security.disableRole(role::STRING,disable::BOOLEAN) :: (::VOID)                                                                                                                    |
| dbms.security.modRoleDesc             | 修改角色描述信息                              | dbms.security.modRoleDesc(role::STRING,description::STRING) :: (::VOID)                                                                                                                 |
| dbms.security.rebuildRoleAccessLevel  | 删除角色权限并重建                             | dbms.security.rebuildRoleAccessLevel(role::STRING,access_level::MAP) :: (::VOID)                                                                                                        |
| dbms.security.modRoleAccessLevel      | 修改角色对指定图的访问权限                         | dbms.security.modRoleAccessLevel(role::STRING,access_level::MAP) :: (::VOID)                                                                                                            |
| dbms.security.modRoleFieldAccessLevel | 修改角色对指定属性的访问权限                        | dbms.security.modRoleFieldAccessLevel(role::STRING,graph::STRING,label::STRING,field::STRING,label_type::STRING,field_access_level::STRING) :: (::VOID)                                 |
| dbms.security.getUserInfo             | 获取用户详细信息                              | dbms.security.getUserInfo(user::STRING) :: (user_info::MAP)                                                                                                                             |
| dbms.security.disableUser             | 禁用/启用用户                               | dbms.security.disableUser(user::STRING,disable::BOOLEAN) :: (::VOID)                                                                                                                    |
| dbms.security.setCurrentDesc          | 设置当前用户描述信息                            | dbms.security.setCurrentDesc(description::STRING) :: (::VOID)                                                                                                                           |
| dbms.security.setUserDesc             | 设置用户描述信息                              | dbms.security.setUserDesc(user::STRING,description::STRING) :: (::VOID)                                                                                                                 |
| dbms.security.getUserMemoryUsage      | 获取用户内存用量                              | dbms.security.getUserMemoryUsage(user::STRING) :: (memory_usage::INTEGER)                                                                                                               |
| dbms.security.setUserMemoryLimit      | 设置用户内存限制                              | dbms.security.setUserMemoryLimit(user::STRING,memorylimit::INTEGER) :: (::VOID)                                                                                                         |
| dbms.security.deleteUserRoles         | 删除用户与角色的联系                            | dbms.security.deleteUserRoles(user::STRING,roles::LIST) :: (::VOID)                                                                                                                     |
| dbms.security.rebuildUserRoles        | 清空用户角色的关系并重建                          | dbms.security.rebuildUserRoles(user::STRING,roles::LIST) :: (::VOID)                                                                                                                    |
| dbms.security.addUserRoles            | 新增用户与角色的联系                            | dbms.security.addUserRoles(user::STRING,roles::LIST) :: (::VOID)                                                                                                                        |
| db.plugin.loadPlugin                  | 装载plugin                              | db.plugin.loadPlugin(plugin_type::STRING,plugin_name::STRING,plugin_content::STRING or MAP,code_type::STRING,plugin_description::STRING,read_only::BOOLEAN,version::STRING) :: (::VOID) |
| db.plugin.deletePlugin                | 删除plugin                              | db.plugin.deletePlugin(plugin_type::STRING,plugin_name::STRING) :: (::VOID)                                                                                                             |
| db.plugin.listPlugin                  | 列出已装载的plugin                          | db.plugin.listPlugin(plugin_type::STRING,plugin_version::STRING) :: (plugin_description::LIST)                                                                                          |
| db.plugin.getPluginInfo               | 获取plugin的详细信息                         | db.plugin.getPluginInfo(plugin_type::STRING,plugin_name::STRING,show_code::BOOLEAN)::(plugin_description::MAP)                                                                          |
| db.plugin.callPlugin                  | 执行plugin                              | db.plugin.callPlugin(plugin_type::STRING,plugin_name::STRING,param::STRING,timeout::DOUBLE,in_process::BOOLEAN) :: (success::BOOLEAN,result::STRING)                                    |
| db.importor.dataImportor              | 导入点或边数据                               | db.importor.dataImportor(description::STRING,content::STRING,continue_on_error::BOOLEAN,thread_nums::INTEGER,delimiter::STRING) :: (::VOID)                                             |
| db.importor.schemaImportor            | 导入点或边schema                           | db.importor.schemaImportor(description::STRING) :: (::VOID)                                                                                                                             |
| db.addFullTextIndex                   | 添加全文索引                                | db.addFullTextIndex(is_vertex::BOOLEAN, label_name::STRING, field_name::STRING) :: (::VOID)                                                                                             |
| db.deleteFullTextIndex                | 删除全文索引                                | db.deleteFullTextIndex(is_vertex::BOOLEAN, label_name::STRING, field_name::STRING) :: (::VOID)                                                                                          |
| db.rebuildFullTextIndex               | 重建全文索引                                | db.rebuildFullTextIndex(vertex_labels::STRING, edge_labels::STRING) :: (::VOID)                                                                                                         |
| db.fullTextIndexes                    | 查看全文索引                                | db.fullTextIndexes() :: (is_vertex::BOOLEAN, label::STRING, field::STRING)                                                                                                              |
| dbms.meta.count                       | 查看点边总数                                | db.dbms.meta.count() :: (type::STRING, number::INTEGER)                                                                                                                                 |
| dbms.meta.countDetail                 | 查看点边总数详情                              | db.dbms.meta.countDetail() :: (is_vertex::BOOLEAN, label::STRING, count::INTEGER)                                                                                                       |
| dbms.meta.refreshCount                | 重新统计点边数量，统计期间停写。                      | db.dbms.meta.refreshCount() :: (::VOID)                                                                                                                                                 |
| dbms.task.listTasks                   | 查询正在执行的任务                             | dbms.task.listTasks()::(tasks::LIST)                                                                                                                                                    |
| dbms.task.terminateTask               | 中止任务                                  | dbms.task.terminateTask(task_id::STRING)::(::VOID)                                                                                                                                      |
| dbms.ha.clusterInfo                   | HA模式下查看集群状态                           | dbms.ha.clusterInfo() :: (cluster_info::LIST, is_master::BOOLEAN)                                                                                                                       |
| db.dropDB                             | 清空数据库                                 | db.dropDB() :: (::VOID)                                                                                                                                                                 |