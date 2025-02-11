# ISO GQL


## 1.GQL简介
Graph Query Language(GQL, 图查询语言)是一种国际标准语言，用于属性图查询，该语言建立在SQL的基础上，并整合了现有的[openCypher、PGQL、GSQL和G-CORE](https://gql.today/comparing-cypher-pgql-and-g-core/)语言的成熟思想。目前该标准仍然处于草稿阶段。

TuGraph基于[ISO GQL (ISO/IEC 39075) Antlr4 语法文件](https://github.com/TuGraph-family/gql-grammar)实现了GQL，并做了一些扩展与改造。目前并未完全支持所有的GQL语法，我们会在未来逐步完善。

(https://github.com/TuGraph-family/gql-grammar)仓库包含了图查询语言标准（ISO/IEC 39075） 的 Antlr4 语法文件实现，该标准目前处于草案阶段。我们最新的语法文件基于 2023 年 3 月版的草案。GQL 是一种用于将结构化数据建模为图的数据库语言，可用于在图数据库或其他图存储中存储、查询和修改数据。通过开源这些语法文件，我们旨在促进 GQL 在图数据库中的快速应用，并加速标准化过程。

## 2.Clauses

| 类别                | 子句           |
| ------------------- | -------------- |
| Reading clauses     | MATCH          |
|                     | OPTIONAL MATCH |
| Projecting clauses  | RETURN         |
|                     | NEXT           |
| Reading sub-clauses | WHERE          |
|                     | ORDER BY       |
|                     | SKIP           |
|                     | LIMIT          |
表格内容描述:
该表格包括两列：类别和子句。 

在“Reading clauses”类别下，有两个子句：MATCH 和 OPTIONAL MATCH。 

在“Projecting clauses”类别中，有两个子句：RETURN 和 NEXT。 

在“Reading sub-clauses”类别里，列出了四个子句：WHERE、ORDER BY、SKIP 和 LIMIT。 

总体来看，该表格列出了与查询相关的不同类别和其对应的子句，提供了对查询语句结构的清晰分类。

### 2.1.MATCH

`MATCH`子句式是GQL最基础的子句，几乎所有查询都是通过 `MATCH`展开。

`MATCH`子句用于指定在图中搜索的匹配模式，用来匹配满足一定条件的点或者路径。

#### 点查询

##### 查询所有点

```
MATCH (n)
RETURN n
```

##### 查询特定标签的点

```
MATCH (n:Person)
RETURN n
```

##### 通过属性匹配点

```
MATCH (n:Person{name:'Michael Redgrave'})
RETURN n.birthyear
```

返回结果
```JSON
[{"n.birthyear":1908}]
```

##### 通过过滤条件匹配点


```
MATCH (n:Person WHERE n.birthyear > 1910)
RETURN n.name LIMIT 2
```

返回结果
```JSON
[{"n.name":"Christopher Nolan"},{"n.name":"Corin Redgrave"}]
```

#### 边查询

##### 出边匹配

```
MATCH (n:Person WHERE n.birthyear = 1970)-[e]->(m)
RETURN n.name, label(e), m.name
```

返回结果
```JSON
[{"label(e)":"BORN_IN","m.name":"London","n.name":"Christopher Nolan"},{"label(e)":"DIRECTED","m.name":null,"n.name":"Christopher Nolan"}]
```

##### 入边匹配

```
MATCH (n:Person WHERE n.birthyear = 1939)<-[e]-(m)
RETURN n.name, label(e), m.name
```

返回结果
```JSON
[{"label(e)":"HAS_CHILD","m.name":"Rachel Kempson","n.name":"Corin Redgrave"},{"label(e)":"HAS_CHILD","m.name":"Michael Redgrave","n.name":"Corin Redgrave"}]
```

##### 带过滤条件的边匹配

```
MATCH (n:Person)-[e:BORN_IN WHERE e.weight > 20]->(m)
RETURN n.name, e.weight, m.name
```

返回结果
```JSON
[{"e.weight":20.549999237060547,"m.name":"New York","n.name":"John Williams"},{"e.weight":20.6200008392334,"m.name":"New York","n.name":"Lindsay Lohan"}]
```

#### 路径匹配

##### 不定跳查询

```
MATCH (n:Person)-[e]->{2,3}(m:Person)
RETURN m.name LIMIT 2
```

返回结果
```JSON
[{"m.name":"Liam Neeson"},{"m.name":"Natasha Richardson"}]
```

### 2.2.OPTIONAL MATCH

`OPTIONAL MATCH`匹配图模式，如果未命中，则返回`null`。

#### 查询命中

```
OPTIONAL MATCH (n:Person{name:'Michael Redgrave'})
RETURN n.birthyear
```

返回结果
```JSON
[{"n.birthyear":1908}]
```

#### 查询未命中

```
OPTIONAL MATCH (n:Person{name:'Redgrave Michael'})
RETURN n.birthyear
```

返回结果

```JSON
[{"n.birthyear":null}]
```

### 2.3.RETURN

`RETURN`子句指定返回结果，包括返回点、边、路径、属性等。

#### 返回点

```
MATCH (n)
RETURN n LIMIT 2
```

返回结果
```JSON
[{"n":{"identity":0,"label":"Person","properties":{"birthyear":1910,"name":"Rachel Kempson"}}},{"n":{"identity":1,"label":"Person","properties":{"birthyear":1908,"name":"Michael Redgrave"}}}]
```

#### 返回边

```
MATCH (n)-[e]->(m)
RETURN e LIMIT 2
```

返回结果

```JSON
[{"e":{"dst":2,"forward":false,"identity":0,"label":"HAS_CHILD","label_id":0,"src":0,"temporal_id":0}},{"e":{"dst":3,"forward":false,"identity":0,"label":"HAS_CHILD","label_id":0,"src":0,"temporal_id":0}}]
```

#### 返回属性

```
MATCH (n:Person)
RETURN n.name LIMIT 2
```

返回结果

```JSON
[{"n.name":"Christopher Nolan"},{"n.name":"Corin Redgrave"}]
```

#### 不常见字符串作为变量名

```
MATCH (`/uncommon variable`:Person)
RETURN `/uncommon variable`.name LIMIT 3
```

返回结果

```JSON
[{"`/uncommon variable`.name":"Christopher Nolan"},{"`/uncommon variable`.name":"Corin Redgrave"},{"`/uncommon variable`.name":"Dennis Quaid"}]
```

#### 列别名

```
MATCH (n:Person)
RETURN n.name AS nname LIMIT 2
```

返回结果

```JSON
[{"nname":"Christopher Nolan"},{"nname":"Corin Redgrave"}]
```

#### 可选属性

```
MATCH (n:Person)
RETURN n.age LIMIT 2
```

返回结果

```JSON
[{"n.age":null},{"n.age":null}]
```

#### 其它表达式

```
MATCH (n:Person)
RETURN n.birthyear > 1970, "I'm a literal", 1 + 2, abs(-2)
LIMIT 2
```

返回结果

```JSON
[{"\"I'm a literal\"":"I'm a literal","1 + 2":3,"abs(-2)":2,"n.birthyear > 1970":false},{"\"I'm a literal\"":"I'm a literal","1 + 2":3,"abs(-2)":2,"n.birthyear > 1970":false}]
```

#### 结果唯一性


```
MATCH (n)
RETURN DISTINCT label(n) AS label
```

返回结果

```JSON
[{"label":"Person"},{"label":"City"},{"label":"Film"}]
```

### 2.4.NEXT

`NEXT`子句用于连接多个子句。

#### 连接MATCH

```
MATCH (n:Person) WHERE n.birthyear = 1970
RETURN n
NEXT
MATCH (m:Person) WHERE m.birthyear < 1968
RETURN n.name, n.birthyear, m.name LIMIT 2
```

返回结果
```JSON
[{"m.name":"Rachel Kempson","n.birthyear":1970,"n.name":"Christopher Nolan"},{"m.name":"Michael Redgrave","n.birthyear":1970,"n.name":"Christopher Nolan"}]
```

### 2.5.WHERE

`WHERE`子句用于过滤记录。

#### 过滤点

```
MATCH (n:Person WHERE n.birthyear > 1965)
RETURN n.name
```

返回结果

```JSON
[{"n.name":"Christopher Nolan"},{"n.name":"Lindsay Lohan"}]
```

#### 过滤边

```
MATCH (n:Person WHERE n.birthyear > 1965)-[e:ACTED_IN]->(m:Film)
WHERE e.charactername = 'Halle/Annie'
RETURN m.title
```

返回结果

```JSON
[{"m.title":"The Parent Trap"}]
```

#### 布尔表达式

`AND`, `OR`, `XOR`和 `NOT`布尔表达式可以用在 `WHERE`中用来过滤数据。

```
MATCH (n:Person)
WHERE
	n.birthyear > 1930 AND (n.birthyear < 1950 OR n.name = 'Corin Redgrave')
RETURN n LIMIT 2
```

返回结果

```JSON
[{"n":{"identity":3,"label":"Person","properties":{"birthyear":1939,"name":"Corin Redgrave"}}},{"n":{"identity":11,"label":"Person","properties":{"birthyear":1932,"name":"John Williams"}}}]
```


### 2.6.ORDER BY

`ORDER BY`是`RETURN`的子句，对输出的结果进行排序。

#### 对结果排序

```
MATCH (n:Person WHERE n.birthyear < 1970)
RETURN n.birthyear AS q
ORDER BY q ASC
LIMIT 5
```

返回结果
```JSON
[{"q":1873},{"q":1908},{"q":1910},{"q":1930},{"q":1932}]
```

### 2.7.SKIP

`SKIP`指定结果偏移行数。

#### 未使用SKIP

```
MATCH (n:Person)
RETURN n.name LIMIT 3
```

返回结果

```JSON
[{"n.name":"Christopher Nolan"},{"n.name":"Corin Redgrave"},{"n.name":"Dennis Quaid"}]
```

#### 使用SKIP

```
MATCH (n:Person)
RETURN n.name SKIP 1 LIMIT 2
```

返回结果
```JSON
[{"n.name":"Corin Redgrave"},{"n.name":"Dennis Quaid"}]
```

### 2.8.LIMIT

`LIMIT`限制结果行数。

#### 使用LIMIT

```
MATCH (n:Person)
RETURN n.name LIMIT 2;
```

返回结果
```JSON
[{"n.name":"Christopher Nolan"},{"n.name":"Corin Redgrave"}]
```