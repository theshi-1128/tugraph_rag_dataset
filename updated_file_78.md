# TuGraph 嵌入模式 Python API 文档概述（Python开发存储过程接口）

## 介绍

TuGraph 是一个图数据库，与 SQLite 和 Neo4j 类似，支持嵌入式模式。在嵌入式模式下，TuGraph 作为一个库运行，允许用户编写应用程序并调用库函数来创建、查询和修改图数据。数据交换在同一进程中进行，操作简单且高效。

与 SQLite 和 Noe4j 类似，TuGraph 可以在嵌入式模式下工作。 在嵌入式模式下，它就像一个库。 您可以编写自己的应用程序并调用库函数来创建、查询和修改图。 在这种情况下，应用程序和图数据库之间的所有数据交换都在同一个进程中进行。 它非常简单高效。

这是 TuGraph 嵌入模式的 python API 文档。 通过嵌入式API，用户可以打开或创建数据库，然后查询或修改数据库。

## 接口

以下是 TuGraph 嵌入模式的 Python API 类及其成员的详细介绍：

### `liblgraph_python_api.AccessLevel`
表示用户在图上的访问权限。  
*Access that a user has on a graph.*

- **成员 Members**:
  - `NONE`: 无权限. *No access.*
  - `READ`: 只读权限. *Read-only access.*
  - `WRITE`: 写权限. *Write access.*
  - `FULL`: 完全权限. *Full access.*
- **属性 Property**:
  - `name`: 访问级别名称. *Name of the access level.*


### `liblgraph_python_api.EdgeUid`
表示边的标识符。  
*Edge identifier.*

- **属性 Property**:
  - `dst`: 目标顶点 ID. *Destination vertex ID.*
  - `eid`: 边的 ID. *ID of the edge.*
  - `lid`: 边的标签 ID. *Label ID of the edge.*
  - `src`: 源顶点 ID. *Source vertex ID.*
  - `tid`: 边的时间 ID. *Temporal ID of the edge.*

  

### `liblgraph_python_api.FieldData`
表示字段值的数据类型。  
*FieldData is the data type of field value.*

- **方法 Methods**:
  - `AsBlob(self: liblgraph_python_api.FieldData) → bytes`: 返回 BLOB 类型的值，如果类型不匹配则抛出异常.  
    *Get value as BLOB, throws exception on type mismatch.*
  - `AsBool(self: liblgraph_python_api.FieldData) → bool`: 返回布尔类型的值，如果类型不匹配则抛出异常.  
    *Get value as bool, throws exception on type mismatch.*
  - `AsDate(self: liblgraph_python_api.FieldData) → datetime.datetime`: 返回日期类型的值，如果类型不匹配则抛出异常.  
    *Get value as date, throws exception on type mismatch.*
  - `AsDateTime(self: liblgraph_python_api.FieldData) → datetime.datetime`: 返回日期时间类型的值，如果类型不匹配则抛出异常.  
    *Get value as datetime, throws exception on type mismatch.*
  - `AsDouble(self: liblgraph_python_api.FieldData) → float`: 返回双精度浮点数类型的值，如果类型不匹配则抛出异常.  
    *Get value as double, throws exception on type mismatch.*
  - `AsFloat(self: liblgraph_python_api.FieldData) → float`: 返回单精度浮点数类型的值，如果类型不匹配则抛出异常.  
    *Get value as float, throws exception on type mismatch.*
  - `AsInt16(self: liblgraph_python_api.FieldData) → int`: 返回 16 位整型值，如果类型不匹配则抛出异常.  
    *Get value as int16, throws exception on type mismatch.*
  - `AsInt32(self: liblgraph_python_api.FieldData) → int`: 返回 32 位整型值，如果类型不匹配则抛出异常.  
    *Get value as int32, throws exception on type mismatch.*
  - `AsInt64(self: liblgraph_python_api.FieldData) → int`: 返回 64 位整型值，如果类型不匹配则抛出异常.  
    *Get value as int64, throws exception on type mismatch.*
  - `AsInt8(self: liblgraph_python_api.FieldData) → int`: 返回 8 位整型值，如果类型不匹配则抛出异常.  
    *Get value as int8, throws exception on type mismatch.*
  - `AsString(self: liblgraph_python_api.FieldData) → str`: 返回字符串值，如果类型不匹配则抛出异常.  
    *Get value as string, throws exception on type mismatch.*
    - `static Blob(arg0: bytes) → liblgraph_python_api.FieldData`: 创建 BLOB 值.  
    *Make a BLOB value.*
  - `static Bool(arg0: bool) → liblgraph_python_api.FieldData`: 创建布尔值.  
    *Make a BOOL value.*
  - `static Date(*args, **kwargs)`: 重载函数.  
    *Overloaded function.*
    - `1.Date(arg0: str) → liblgraph_python_api.FieldData`: 创建日期值.  
      *Make a DATE value.*
    - `2.Date(arg0: datetime.datetime) → liblgraph_python_api.FieldData`: 创建日期值.  
      *Make a DATE value.*
  - `static DateTime(*args, **kwargs)`: 重载函数.  
    *Overloaded function.*
    - `1.DateTime(arg0: str) → liblgraph_python_api.FieldData`: 创建日期时间值.  
      *Make a DATETIME value.*
    - `2.DateTime(arg0: datetime.datetime) → liblgraph_python_api.FieldData`: 创建日期时间值.  
      *Make a DATETIME value.*
  - `static Double(arg0: float) → liblgraph_python_api.FieldData`: 创建双精度浮点数值.  
    *Make a DOUBLE value.*
  - `static Float(arg0: float) → liblgraph_python_api.FieldData`: 创建单精度浮点数值.  
    *Make a FLOAT value.*
  - `static Int16(arg0: int) → liblgraph_python_api.FieldData`: 创建 16 位整型值.  
    *Make a INT16 value.*
  - `static Int32(arg0: int) → liblgraph_python_api.FieldData`: 创建 32 位整型值.  
    *Make a INT32 value.*
  - `static Int64(arg0: int) → liblgraph_python_api.FieldData`: 创建 64 位整型值.  
    *Make a INT64 value.*
  - `static Int8(arg0: int) → liblgraph_python_api.FieldData`: 创建 8 位整型值.  
    *Make a INT8 value.*
  - `static String(arg0: str) → liblgraph_python_api.FieldData`: 创建字符串值.  
    *Make a STRING value.*
  - `ToPython(self: liblgraph_python_api.FieldData) → object`: 转换为相应的 Python 类型.  
    *Convert to corresponding Python type.*
  - `get(self: liblgraph_python_api.FieldData) → object`: 获取字段值.  
    *Get the field value.*
  - `isNull(self: liblgraph_python_api.FieldData) → bool`: 检查字段是否为 null.  
    *Check if the field is null.*
  - `set(self: liblgraph_python_api.FieldData, arg0: object) → None`: 设置字段值.  
    *Set the field value.*

  

### `liblgraph_python_api.FieldSpec`
表示字段的规格。  
*Represents the specifications of a field.*

- **属性 Property**:
  - `name`: 字段名称. *Name of this field.*
  - `nullable`: 字段是否可以为空. *Whether this field can be null.*
  - `type`: 字段类型（例如 INT8, FLOAT, STRING）. *Type of this field, INT8, INT16, …, FLOAT, DOUBLE, STRING.*


### `liblgraph_python_api.FieldType`
表示 FieldData 的数据类型。  
*Data type of FieldData.*

- **成员 Members**:
  - `NUL`: 空值类型.  
    *Null type.*
  - `BOOL`: 布尔类型.  
    *Boolean type.*
  - `INT8`: 8 位整型.  
    *8-bit integer type.*
  - `INT16`: 16 位整型.  
    *16-bit integer type.*
  - `INT32`: 32 位整型.  
    *32-bit integer type.*
  - `INT64`: 64 位整型.  
    *64-bit integer type.*
  - `FLOAT`: 单精度浮点数.  
    *Float type.*
  - `DOUBLE`: 双精度浮点数.  
    *Double type.*
  - `DATE`: 日期类型.  
    *Date type.*
  - `DATETIME`: 日期时间类型.  
    *DateTime type.*
  - `STRING`: 字符串类型.  
    *String type.*
  - `BLOB`: 二进制大对象类型.  
    *BLOB type.*

- **属性 Property**:
  - `name`: 数据类型名称.  
    *Name of the data type.*




### `liblgraph_python_api.Galaxy`
一个银河是一个 TuGraph 实例，持有多个 GraphDB。银河存储在一个目录中，管理用户和 GraphDB。每个 (用户, GraphDB) 对可以具有不同的访问级别。您可以使用 `db = Galaxy.OpenGraph(graph)` 打开一个图。由于 Python 中的垃圾回收是自动的，完成后需要使用 `Galaxy.Close()` 关闭银河。  
*A galaxy is a TuGraph instance that holds multiple GraphDBs. A galaxy is stored in a directory and manages users and GraphDBs. Each (user, GraphDB) pair can have different access levels. You can use `db = Galaxy.OpenGraph(graph)` to open a graph. Since garbage collection in Python is automatic, you need to close the galaxy with `Galaxy.Close()` when you are done with it.*

- **方法 Methods**:
  - `Close(self: liblgraph_python_api.Galaxy) → None`: 关闭此银河.  
    *Closes this galaxy.*
  
  - `CreateGraph(self: liblgraph_python_api.Galaxy, name: str, description: str = '', max_size: int = 4398046511104) → bool`: 创建一个图。  
    `name`: 图的名称.  
    `description`: 图的描述.  
    `max_size`: 图的最大大小，默认 1TB.  
    *Creates a graph. `name`: the name of the graph; `description`: description of the graph; `max_size`: maximum size of the graph, default 1TB.*
  
  - `CreateRole(self: liblgraph_python_api.Galaxy, name: str, desc: str) → bool`: 创建一个角色.  
    `name`: 角色名称.  
    `desc`: 角色描述.  
    *Create a role. `name`: name of the role; `desc`: description of the role.*
  
  - `CreateUser(self: liblgraph_python_api.Galaxy, name: str, password: str, desc: str) → bool`: 创建一个新用户账户.  
    `name`: 用户名称.  
    `password`: 用户密码.  
    `desc`: 此用户的描述.  
    *Creates a new user account. `name`: name of the user; `password`: password for the user; `desc`: description of this user.*
  
  - `DeleteGraph(self: liblgraph_python_api.Galaxy, arg0: str) → bool`: 删除一个图.  
    *Deletes a graph.*
  
  - `DeleteRole(self: liblgraph_python_api.Galaxy, arg0: str) → bool`: 删除指定角色.  
    *Deletes the specified role.*
  
  - `DeleteUser(self: liblgraph_python_api.Galaxy, arg0: str) → bool`: 删除用户账户.  
    *Deletes a user account.*
  
  - `DisableUser(self: liblgraph_python_api.Galaxy, arg0: str) → bool`: 禁用用户.  
    *Disables a user.*
  
  - `EnableUser(self: liblgraph_python_api.Galaxy, arg0: str) → bool`: 启用用户.  
    *Enables a user.*
  
  - `GetUserInfo(self: liblgraph_python_api.Galaxy, arg0: str) → lgraph_api::UserInfo`: 获取指定用户的信息.  
    *Get information of the specified user.*
  
  - `ListGraphs(self: liblgraph_python_api.Galaxy) → Dict[str, Tuple[str, int]]`: 列出图并返回字典 {名称:(描述, 最大大小)}.  
    *Lists graphs and returns a dictionary of {name:(desc, max_size)}.*
  
  - `ListUsers(self: liblgraph_python_api.Galaxy) → Dict[str, lgraph_api::UserInfo]`: 列出所有用户及其是否为管理员.  
    *Lists all users and whether they are admin.*
  
  - `ModGraph(self: liblgraph_python_api.Galaxy, graph_name: str, mod_desc: bool, description: str, mod_size: bool, new_max_size: int) → bool`: 修改图.  
    `mod_size`: 是否修改最大图大小.  
    `new_max_size`: 新的图的最大大小（以字节为单位）.  
    *Modifies a graph. `mod_size`: whether to modify max graph size; `new_max_size`: new maximum size of the graph, in bytes.*
  
  - `OpenGraph(self: liblgraph_python_api.Galaxy, graph: str, read_only: bool = False) → lgraph_api::GraphDB`: 打开一个图并返回 GraphDB 实例.  
    `graph`: 图的名称.  
    `read_only`: 是否以只读模式打开图.  
    *Opens a graph and returns a GraphDB instance. `graph`: name of the graph; `read_only`: whether to open the graph in read-only mode.*
  
  - `SetCurrentUser(self: liblgraph_python_api.Galaxy, user: str, password: str) → None`: 验证用户密码并设置当前用户.  
    `user`: 用户名.  
    `password`: 用户密码.  
    *Validate user password and set current user. `user`: user name; `password`: password of the user.*
  
  - `SetRoleAccessRights(self: liblgraph_python_api.Galaxy, arg0: str, arg1: Dict[str, liblgraph_python_api.AccessLevel]) → bool`: 设置指定角色的访问权限.  
    *Set access rights for the specified role.*
  
  - `SetRoleAccessRightsIncremental(self: liblgraph_python_api.Galaxy, arg0: str, arg1: Dict[str, liblgraph_python_api.AccessLevel]) → bool`: 为指定角色设置增量访问权限，仅影响指定图.  
    *Set access rights for the specified role, only affects the specified graphs.*
  
  - `SetRoleDesc(self: liblgraph_python_api.Galaxy, name: str, desc: str) → bool`: 设置指定角色的描述.  
    `name`: 角色名称.  
    `desc`: 角色描述.  
    *Set description of the specified role. `name`: name of the role; `desc`: description of the role.*
  
  - `SetUser(self: liblgraph_python_api.Galaxy, user: str) → None`: 验证给定用户并设置当前用户.  
    `user`: 用户名.  
    *Validate the given user and set current user given in the user.*
  
  - `SetUserGraphAccess(self: liblgraph_python_api.Galaxy, user: str, graph: str, access: liblgraph_python_api.AccessLevel) → bool`: 设置指定用户在图上的访问级别.  
    `user`: 用户名称.  
    `graph`: 图的名称.  
    `access`: 用户在该图上的访问级别.  
    *Set the access level of the specified user on the graph. `user`: name of the user; `graph`: name of the graph; `access`: access level of the user on that graph.*
  
  - `SetUserPass(self: liblgraph_python_api.Galaxy, name: str, old_password: str = '', new_password: str) → bool`: 修改用户密码.  
    `name`: 用户名称.  
    `old_password`: 当前密码，在修改其他用户时不需要.  
    `new_password`: 用户的新密码.  
    *Modifies user password. `name`: name of the user; `old_password`: current password, not needed when modifying another user; `new_password`: new password for the user.*
  
  - `SetUserRoles(self: liblgraph_python_api.Galaxy, name: str, roles: List[str]) → bool`: 为指定用户设置角色.  
    `name`: 用户名称.  
    `roles`: 该用户的角色列表.  
    *Set the roles for the specified user. `name`: name of the user; `roles`: list of roles for this user.*




### `liblgraph_python_api.GraphDB`
图数据库类。GraphDB 存储图的数据，包括标签、顶点、边和索引。由于 Python 中的垃圾回收是自动的，您需要在生命周期结束时使用 `GraphDB.Close()` 关闭数据库。在关闭数据库之前，确保已提交或中止每个使用数据库的事务。  
*The graph database class. A GraphDB stores the data about the graph, including labels, vertices, edges, and indexes. Since garbage collection in Python is automatic, you need to close the DB with `GraphDB.Close()` at the end of its lifetime. Make sure you have either committed or aborted every transaction that is using the DB before you close the DB.*

- **方法 Methods**:
  - `AddEdgeLabel(self: liblgraph_python_api.GraphDB, label_name: str, field_specs: List[liblgraph_python_api.FieldSpec], temporal_field: str = '', constraints: List[Tuple[str, str]] = []) → bool`: 添加边标签.  
    *Adds an edge label.*
  
  - `AddVertexIndex(self: liblgraph_python_api.GraphDB, label_name: str, field_name: str, is_unique: bool) → bool`: 添加索引.  
    *Adds an index.*
  
  - `AddVertexLabel(self: liblgraph_python_api.GraphDB, label_name: str, field_specs: List[liblgraph_python_api.FieldSpec], primary_field: str) → bool`: 添加顶点标签.  
    *Add a vertex label.*
  
  - `AlterEdgeLabelAddFields(self: liblgraph_python_api.GraphDB, label: str, add_fields: List[liblgraph_python_api.FieldSpec], default_values: List[liblgraph_python_api.FieldData]) → int`: 向边标签添加字段.  
    `label`: 标签名称.  
    `add_fields`: 新添加字段的 FieldSpec 列表.  
    `default_values`: 新添加字段的默认值.  
    *Add fields to an edge label. `label`: name of the label; `add_fields`: list of FieldSpec for the newly added fields; `default_values`: default values of the added fields.*
  
  - `AlterEdgeLabelDelFields(self: liblgraph_python_api.GraphDB, label: str, del_fields: List[str]) → int`: 从边标签中删除字段.  
    `label`: 标签名称.  
    `del_fields`: 字段名称列表.  
    *Delete fields from an edge label. `label`: name of the label; `del_fields`: list of field names.*
  
  - `AlterEdgeLabelModFields(self: liblgraph_python_api.GraphDB, label: str, mod_fields: List[liblgraph_python_api.FieldSpec]) → int`: 修改边标签中的字段.  
    `label`: 标签名称.  
    `mod_fields`: 修改字段的 FieldSpec 列表.  
    *Modify fields in an edge label. `label`: name of the label; `mod_fields`: list of FieldSpec for the modified fields.*
  
  - `AlterEdgeLabelModifyConstraints(self: liblgraph_python_api.GraphDB, label_name: str, constraints: List[Tuple[str, str]]) → bool`: 修改边约束.  
    *Modify edge constraints.*
  
  - `AlterVertexLabelAddFields(self: liblgraph_python_api.GraphDB, label: str, add_fields: List[liblgraph_python_api.FieldSpec], default_values: List[liblgraph_python_api.FieldData]) → int`: 向顶点标签添加字段.  
    `label`: 标签名称.  
    `add_fields`: 新添加字段的 FieldSpec 列表.  
    `default_values`: 新添加字段的默认值.  
    *Add fields to a vertex label. `label`: name of the label; `add_fields`: list of FieldSpec for the newly added fields; `default_values`: default values of the added fields.*
  
  - `AlterVertexLabelDelFields(self: liblgraph_python_api.GraphDB, label: str, del_fields: List[str]) → int`: 从顶点标签中删除字段.  
    `label`: 标签名称.  
    `del_fields`: 字段名称列表.  
    *Delete fields from a vertex label. `label`: name of the label; `del_fields`: list of field names.*
  
  - `AlterVertexLabelModFields(self: liblgraph_python_api.GraphDB, label: str, mod_fields: List[liblgraph_python_api.FieldSpec]) → int`: 修改顶点标签中的字段.  
    `label`: 标签名称.  
    `mod_fields`: 修改字段的 FieldSpec 列表.  
    *Modify fields in a vertex label. `label`: name of the label; `mod_fields`: list of FieldSpec for the modified fields.*
  
  - `Close(self: liblgraph_python_api.GraphDB) → None`: 关闭数据库.  
    *Closes the DB.*
  
  - `CreateReadTxn(self: liblgraph_python_api.GraphDB) → lgraph_api::Transaction`: 创建读取事务.  
    *Create a read transaction.*
  
  - `CreateWriteTxn(self: liblgraph_python_api.GraphDB, optimistic: bool = False) → lgraph_api::Transaction`: 创建写事务.  
    *Create a write transaction.*
  
  - `DeleteEdgeLabel(self: liblgraph_python_api.GraphDB, label_name: str) → int`: 删除边标签.  
    *Deletes an edge label.*
  
  - `DeleteVertexIndex(self: liblgraph_python_api.GraphDB, label_name: str, field_name: str) → bool`: 删除指定索引.  
    *Deletes the specified index.*
  
  - `DeleteVertexLabel(self: liblgraph_python_api.GraphDB, label_name: str) → int`: 删除顶点标签.  
    *Deletes a vertex label.*
  
  - `DropAllData(self: liblgraph_python_api.GraphDB) → None`: 删除数据库中的所有数据. 所有顶点、边、标签和索引将被删除.  
    *Drop all the data in this DB. All vertices, edges, labels, and indexes will be dropped.*
  
  - `DropAllVertex(self: liblgraph_python_api.GraphDB) → None`: 删除数据库中的所有顶点和边. 标签和索引（尽管由于顶点的删除，索引内容将被清除）将被保留.  
    *Drops all the vertices and edges in this DB. Labels and indexes (though index contents will be cleared due to deletion of vertices) will be preserved.*
  
  - `EstimateNumVertices(self: liblgraph_python_api.GraphDB) → int`: 获取顶点数量的估算值. 如果有顶点删除，这可能不准确.  
    *Gets an estimation of the number of vertices. This can be inaccurate if there were vertex removals.*
  
  - `Flush(self: liblgraph_python_api.GraphDB) → None`: 刷新写入的数据到磁盘.  
    *Flushes written data into disk.*
  
  - `GetDescription(self: liblgraph_python_api.GraphDB) → str`: 获取图的描述.  
    *Gets description of the graph.*
  
  - `GetMaxSize(self: liblgraph_python_api.GraphDB) → int`: 获取图的最大大小.  
    *Gets maximum size of the graph.*
  
  - `IsVertexIndexed(self: liblgraph_python_api.GraphDB, label_name: str, field_name: str) → bool`: 判断指定字段是否已索引.  
    *Tells whether the specified field is indexed.*




### `liblgraph_python_api.InEdgeIterator`
`InEdgeIterator` 可用于迭代目标顶点的所有入边。入边按 (dst, label, src, eid) 的顺序排序。  
*InEdgeIterator can be used to iterate through all the incoming edges of the destination vertex. Incoming edges are sorted in (dst, label, src, eid) order.*

- **方法 Methods**:
  - `Delete(self: liblgraph_python_api.InEdgeIterator) → None`: 删除当前边。如果有下一个出边，迭代器将指向下一个出边.  
    *Deletes the current edge. The iterator will point to the next out edge if there is any.*
  
  - `GetAllFields(self: liblgraph_python_api.InEdgeIterator) → dict`: 获取所有字段值并返回为字典.  
    *Gets all the field values and returns as a dict.*
  
  - `GetDst(self: liblgraph_python_api.InEdgeIterator) → int`: 返回目标顶点的 ID.  
    *Returns the id of the destination vertex.*
  
  - `GetEdgeId(self: liblgraph_python_api.InEdgeIterator) → int`: 返回当前边的 ID. 边 ID 在相同的 (src, dst) 集合中是唯一的.  
    *Returns the id of the current edge. Edge id is unique across the same (src, dst) set.*
  
  - `GetField(*args, **kwargs)`: 重载函数.
    1. `GetField(self: liblgraph_python_api.InEdgeIterator, field_name: str) → object`: 获取指定字段名称的字段值.  
       *Gets the field value of the field specified by field_name.*
    2. `GetField(self: liblgraph_python_api.InEdgeIterator, field_id: int) → object`: 获取指定字段 ID 的字段值.  
       *Gets the field value of the field specified by field_id.*

  - `GetFields(*args, **kwargs)`: 重载函数.
    1. `GetFields(self: liblgraph_python_api.InEdgeIterator, field_names: List[str]) → list`: 获取指定字段名称的字段值.  
       *Gets field values of the fields specified by field_names.*
    2. `GetFields(self: liblgraph_python_api.InEdgeIterator, field_ids: List[int]) → list`: 获取指定字段 ID 的字段值.  
       *Gets field values of the fields specified by field_ids.*

  - `GetLabel(self: liblgraph_python_api.InEdgeIterator) → str`: 返回边标签的名称.  
    *Returns the name of the edge label.*
  
  - `GetLabelId(self: liblgraph_python_api.InEdgeIterator) → int`: 返回边标签的 ID.  
    *Returns the id of the edge label.*
  
  - `GetSrc(self: liblgraph_python_api.InEdgeIterator) → int`: 返回源顶点的 ID.  
    *Returns the id of the source vertex.*
  
  - `GetUid(self: liblgraph_python_api.InEdgeIterator) → liblgraph_python_api.EdgeUid`: 返回边的 EdgeUid.  
    *Returns the EdgeUid of the edge.*
  
  - `Goto(self: liblgraph_python_api.InEdgeIterator, euid: liblgraph_python_api.EdgeUid, nearest: bool) → bool`: 转到由 euid 指定的入边.  
    *Goes to the in edge specified by euid.*
  
  - `IsValid(self: liblgraph_python_api.InEdgeIterator) → bool`: 告诉迭代器是否有效.  
    *Tells whether the iterator is valid.*
  
  - `Next(self: liblgraph_python_api.InEdgeIterator) → bool`: 转到当前目标顶点的下一个入边。如果没有更多的入边，迭代器将变得无效.  
    *Goes to the next in edge to current destination vertex. If there are no more in edges left, the iterator becomes invalid.*
  
  - `SetField(self: liblgraph_python_api.InEdgeIterator, field_name: str, field_value_object: object) → None`: 设置指定字段.  
    *Sets the specified field.*
  
  - `SetFields(*args, **kwargs)`: 重载函数.
    1. `SetFields(self: liblgraph_python_api.InEdgeIterator, field_names: List[str], field_value_strings: List[str]) → None`: 使用字符串表示的字段值设置指定字段.  
       *Sets the fields specified by field_names with field values in string representation.*
    2. `SetFields(self: liblgraph_python_api.InEdgeIterator, field_names: List[str], field_values: List[liblgraph_python_api.FieldData]) → None`: 使用新值设置指定字段.  
       *Sets the fields specified by field_names with new values.*
    3. `SetFields(self: liblgraph_python_api.InEdgeIterator, value_dict: dict) → None`: 设置字段值，如 value_dict 中所示.  
       *Sets the fields with values as specified in value_dict.*
    4. `SetFields(self: liblgraph_python_api.InEdgeIterator, field_ids: List[int], field_values: List[liblgraph_python_api.FieldData]) → None`: 使用字段值设置指定字段 ID.  
       *Sets the fields specified by field_ids with field values.*

  - `ToString(self: liblgraph_python_api.InEdgeIterator) → str`: 返回当前边的字符串表示.  
    *Returns the string representation of the current edge.*




### `liblgraph_python_api.IndexSpec`
`IndexSpec` 是索引规范。  
*Index specification.*

- **属性 Properties**:
  - `field`: 字段的名称.  
    *Name of the field.*
  
  - `label`: 标签的名称.  
    *Name of the label.*
  
  - `unique`: 指示索引值是否唯一.  
    *Whether the indexed values are unique.*



### `liblgraph_python_api.LGraphType`
`LGraphType` 表示图数据库中的数据类型。  
*Data types in the graph database.*

- **成员 Members**:
  - `NUL`: 表示空值.  
    *NUL*
  
  - `INTEGER`: 整数类型.  
    *INTEGER*
  
  - `FLOAT`: 浮点数类型.  
    *FLOAT*
  
  - `DOUBLE`: 双精度浮点数类型.  
    *DOUBLE*
  
  - `BOOLEAN`: 布尔类型.  
    *BOOLEAN*
  
  - `STRING`: 字符串类型.  
    *STRING*
  
  - `NODE`: 节点类型.  
    *NODE*
  
  - `RELATIONSHIP`: 关系类型.  
    *RELATIONSHIP*
  
  - `PATH`: 路径类型.  
    *PATH*
  
  - `LIST`: 列表类型.  
    *LIST*
  
  - `MAP`: 映射类型.  
    *MAP*
  
  - `ANY`: 任意类型.  
    *ANY*

- **属性 Property**:
  - `name`: 数据类型的名称.  
    *Name of the data type.*



### `liblgraph_python_api.OutEdgeIterator`
`OutEdgeIterator` 用于遍历源顶点的所有出边.  
*Iterates through all the outgoing edges of the source vertex.*

- **方法 Methods**:
  - `Delete(self: liblgraph_python_api.OutEdgeIterator) → None`  
    删除当前边. 如果还有其他出边，迭代器将指向下一个出边.  
    *Deletes the current edge; the iterator points to the next outgoing edge if available.*

  - `GetAllFields(self: liblgraph_python_api.OutEdgeIterator) → dict`  
    获取所有字段值并返回为字典.  
    *Gets all field values and returns them as a dict.*

  - `GetDst(self: liblgraph_python_api.OutEdgeIterator) → int`  
    返回目标顶点的 ID.  
    *Returns the id of the destination vertex.*

  - `GetEdgeId(self: liblgraph_python_api.OutEdgeIterator) → int`  
    返回当前边的 ID. 边 ID 在相同 (src, dst) 集合中是唯一的.  
    *Returns the id of the current edge; edge id is unique across the same (src, dst) set.*

  - `GetField(*args, **kwargs)`  
    **重载函数 Overloaded function**:  
    1. `GetField(self: liblgraph_python_api.OutEdgeIterator, field_name: str) → object`  
       获取指定字段名的字段值.  
       *Gets the field value specified by field_name.*
    2. `GetField(self: liblgraph_python_api.OutEdgeIterator, field_id: int) → object`  
       获取指定字段 ID 的字段值.  
       *Gets the field value specified by field_id.*

  - `GetFields(*args, **kwargs)`  
    **重载函数 Overloaded function**:  
    1. `GetFields(self: liblgraph_python_api.OutEdgeIterator, field_names: List[str]) → list`  
       获取指定字段名的字段值.  
       *Gets field values for the specified field_names.*
    2. `GetFields(self: liblgraph_python_api.OutEdgeIterator, field_ids: List[int]) → list`  
       获取指定字段 ID 的字段值.  
       *Gets field values for the specified field_ids.*

  - `GetLabel(self: liblgraph_python_api.OutEdgeIterator) → str`  
    返回边的标签名称.  
    *Returns the name of the edge label.*

  - `GetLabelId(self: liblgraph_python_api.OutEdgeIterator) → int`  
    返回边标签的 ID.  
    *Returns the id of the edge label.*

  - `GetSrc(self: liblgraph_python_api.OutEdgeIterator) → int`  
    返回源顶点的 ID.  
    *Returns the id of the source vertex.*

  - `GetUid(self: liblgraph_python_api.OutEdgeIterator) → liblgraph_python_api.EdgeUid`  
    返回边的 EdgeUid.  
    *Returns the EdgeUid of the edge.*

  - `Goto(self: liblgraph_python_api.OutEdgeIterator, euid: liblgraph_python_api.EdgeUid, nearest: bool = False) → bool`  
    跳转到指定的出边.  
    *Goes to the outgoing edge specified by euid.*

  - `IsValid(self: liblgraph_python_api.OutEdgeIterator) → bool`  
    检查迭代器是否有效.  
    *Tells whether the iterator is valid.*

  - `Next(self: liblgraph_python_api.OutEdgeIterator) → bool`  
    跳转到当前源顶点的下一个出边. 如果没有更多的出边，迭代器变为无效.  
    *Goes to the next outgoing edge from the current source vertex.*

  - `SetField(self: liblgraph_python_api.OutEdgeIterator, field_name: str, field_value_object: object) → None`  
    设置指定字段的值.  
    *Sets the specified field.*

  - `SetFields(*args, **kwargs)`  
    **重载函数 Overloaded function**:  
    1. `SetFields(self: liblgraph_python_api.OutEdgeIterator, field_names: List[str], field_value_strings: List[str]) → None`  
       用字符串表示的字段值设置指定字段名.  
       *Sets fields specified by field_names with field values in string representation.*
    2. `SetFields(self: liblgraph_python_api.OutEdgeIterator, field_names: List[str], field_values: List[liblgraph_python_api.FieldData]) → None`  
       用 FieldData 设置指定字段名的字段值.  
       *Sets fields specified by field_names with field values in FieldData.*
    3. `SetFields(self: liblgraph_python_api.OutEdgeIterator, value_dict: dict) → None`  
       根据字典设置字段值.  
       *Sets field values as specified in value_dict.*
    4. `SetFields(self: liblgraph_python_api.OutEdgeIterator, field_ids: List[int], field_values: List[liblgraph_python_api.FieldData]) → None`  
       设置指定字段 ID 的字段值.  
       *Sets fields specified by field_ids with field values.*

  - `ToString(self: liblgraph_python_api.OutEdgeIterator) → str`  
    返回当前边的字符串表示.  
    *Returns the string representation of the current edge.*



### `liblgraph_python_api.PluginErrorCode`
`PluginErrorCode` 表示插件的错误代码.  
*ErrorCode of the plugin.*

- **成员 Members**:
  - `SUCCESS`  
    表示成功.  
    *Indicates success.*
  
  - `INPUT_ERR`  
    表示输入错误.  
    *Indicates input error.*
  
  - `INTERNAL_ERR`  
    表示内部错误.  
    *Indicates internal error.*
  
  - `SUCCESS_WITH_SIGNATURE`  
    表示成功但有签名.  
    *Indicates success with signature.*

- **属性 Property**:
  - `name`  
    获取错误代码的名称.  
    *Gets the name of the error code.*



### `liblgraph_python_api.Transaction`
`Transaction` 代表一个图数据库事务.在嵌入模式下，所有操作都在事务中执行，因此享有事务的优势，如原子性和隔离性。您可以随时提交或中止事务，而无需担心其已产生的副作用。事务可以被提交或中止，之后它将被销毁并变得无效。在关闭相应的 GraphDB 之前，请确保您已经销毁了每个事务。事务还会跟踪已创建的迭代器，并在销毁期间释放所有迭代器。  
*In embedded mode, all operations are performed in transactions, enjoying atomicity and isolation. You can commit or abort a transaction at any time without worrying about the side effects it has already made. Transactions can be either committed or aborted, after which they become invalid. Make sure you have destructed every transaction before closing the corresponding GraphDB. Transactions also track the created iterators and release all iterators during destruction.*

- **方法 Methods**:
  - `Abort(self: liblgraph_python_api.Transaction) → None`  
    放弃当前事务.  
    *Aborts the current transaction.*

  - `AddEdge(*args, **kwargs)`  
    添加边. 有多个重载版本:  
    1. `AddEdge(self, src: int, dst: int, label_name: str, field_names: List[str], field_value_strings: List[str]) → liblgraph_python_api.EdgeUid`  
       从 `src` 到 `dst` 添加边，返回新边的 ID。 添加边从 `src` 到 `dst`，使用指定的标签名、字段名和字段值（字符串格式）。返回新添加边的 ID。未在 `field_names` 中的字段被视为 null。  
       *Adds an edge from src to dst with the specified label name, field names, and field values in string format. Returns the id of the newly added edge. Fields that are not in field_names are considered null.*

    2. `AddEdge(self, src: int, dst: int, label_name: str, field_names: List[str], field_values: List[liblgraph_python_api.FieldData]) → liblgraph_python_api.EdgeUid`  
       添加边从 `src` 到 `dst`，使用指定的标签名、字段名和字段值。返回新添加边的 ID。未在 `field_names` 中的字段被视为 null。  
      *Adds an edge from src to dst with the specified label name, field names, and field values. Returns the id of the newly added edge. Fields that are not in field_names are considered null.*
 
    3. `AddEdge(self, src: int, dst: int, label_id: int, field_ids: List[int], field_values: List[liblgraph_python_api.FieldData]) → liblgraph_python_api.EdgeUid`  
       添加边从 `src` 到 `dst`，使用指定的标签 ID、字段 ID 和字段值。返回新添加边的 ID。未在 `field_names` 中的字段被视为 null。  
      *Adds an edge from src to dst with the specified label id, field ids, and field values. Returns the id of the newly added edge. Fields that are not in field_names are considered null.*

    4. `AddEdge(self, src: int, dst: int, label_name: str, value_dict: dict) → liblgraph_python_api.EdgeUid`  
       添加边从 `src` 到 `dst`，使用指定的标签，并填充 `value_dict` 中给定的值。返回新添加边的 ID。未在 `value_dict` 中的字段被视为 null。  
      *Adds an edge from src to dst with the specified label, and fill it with the values given in value_dict. Returns the id of the newly added edge. Fields that are not in value_dict are considered null.*


  - `AddVertex(*args, **kwargs)`  
    添加顶点. 有多个重载版本:  
    1. `AddVertex(self, label_name: str, field_names: List[str], field_value_strings: List[str]) → int`  
       添加顶点，使用指定的标签名、字段名和字段值（字符串格式）。返回新添加顶点的 ID。未在 `field_names` 中的字段被视为 null。  
      *Adds a vertex with the specified label name, field names, and field values in string format. Returns the id of the newly added vertex. Fields that are not in field_names are considered null.*

    2. `AddVertex(self, label_name: str, field_names: List[str], field_values: List[liblgraph_python_api.FieldData]) → int`  
       添加顶点，使用指定的标签名、字段名和字段值。返回新添加顶点的 ID。未在 `field_names` 中的字段被视为 null。  
      *Adds a vertex with the specified label name, field names, and field values. Returns the id of the newly added vertex. Fields that are not in field_names are considered null.*

    3. `AddVertex(self, label_id: int, field_ids: List[int], field_values: List[liblgraph_python_api.FieldData]) → int`  
       添加顶点，使用指定的标签 ID、字段 ID 和字段值。返回新添加顶点的 ID。未在 `field_ids` 中的字段被视为 null。  
      *Adds a vertex with the specified label ids, field ids, and field values. Returns the id of the newly added vertex. Fields that are not in field_ids are considered null.*

    4. `AddVertex(self, label_name: str, value_dict: dict) → int`  
       添加顶点，使用指定的标签名，并根据 `value_dict` 中指定的值进行设置。返回新添加顶点的 ID。未在字典中指定的字段被视为 null。  
      *Adds a vertex with the specified label name and set the value as specified in value_dict. Returns the id of the newly added vertex. Fields that are not specified in the dict are considered null.*


  - `Commit(self: liblgraph_python_api.Transaction) → None`  
    提交当前事务.  
    *Commits the current transaction.*

  - `DumpGraph(self: liblgraph_python_api.Transaction) → None`  
    打印整个图的字符串表示.  
    *Prints the string representation of the whole graph.*

  - `GetEdgeFieldId(*args, **kwargs)`  
    获取边字段 ID. 有多个重载版本:  
    1. `GetEdgeFieldId(self, label_id: int, field_name: str) → int`  
       获取与此 (label_id, field_name) 关联的边字段 ID。  
      *Gets the edge field id associated with this (label_id, field_name).*
    2. `GetEdgeFieldId(self, label_id: int, field_names: List[str]) → List[int]`  
       获取多个字段的边字段 ID.
       *Gets the edge field ids associated with this (label_id, field_names).*

  - `GetEdgeLabelId(self: liblgraph_python_api.Transaction, label_name: str) → int`  
    获取边标签 ID.  
    *Gets the edge label ID associated with this label.*

  - `GetEdgeSchema(self: liblgraph_python_api.Transaction, label_name: str) → List[liblgraph_python_api.FieldSpec]`  
    获取边标签的模式说明.  
    *Gets the schema specification of the edge label.*

  - `GetInEdgeIterator(*args, **kwargs)`  
    获取入边迭代器. 有多个重载版本:  
    1. `GetInEdgeIterator(self, euid: liblgraph_python_api.EdgeUid, nearest: bool) → lgraph_api::InEdgeIterator`  
       获取指向具有 EdgeUid==euid 的入边的 InEdgeIterator。  
      *Gets an InEdgeIterator pointing to the in-edge of vertex dst with EdgeUid==euid.*

    2. `GetInEdgeIterator(self, src: int, dst: int, label_id: int) → lgraph_api::InEdgeIterator`  
        获取从 src 到 dst 的 InEdgeIterator，标签由 label_id 指定。  
      *Gets an InEdgeIterator from src to dst with the label specified by label_id.*


  - `GetNumEdgeLabels(self: liblgraph_python_api.Transaction) → int`  
    获取边标签数量.  
    *Gets the number of edge labels.*

  - `GetNumVertexLabels(self: liblgraph_python_api.Transaction) → int`  
    获取顶点标签数量.  
    *Gets the number of vertex labels.*

  - `GetOutEdgeIterator(*args, **kwargs)`  
    获取指向源顶点的出边迭代器。  
    *Gets an OutEdgeIterator pointing to the out-edge of vertex src.*
    1. `GetOutEdgeIterator(self, euid: liblgraph_python_api.EdgeUid, nearest: bool) → lgraph_api::OutEdgeIterator`  
       获取指向具有 EdgeUid==euid 的出边的 OutEdgeIterator。  
      *Gets an OutEdgeIterator pointing to the out-edge of vertex src with EdgeUid==euid.*

    2. `GetOutEdgeIterator(self, src: int, dst: int, label_id: int) → lgraph_api::OutEdgeIterator`  
       获取从 src 到 dst 的 OutEdgeIterator，标签由 label_id 指定。  
      *Gets an OutEdgeIterator from src to dst with the label specified by label_id.*


  - `GetVertexByUniqueIndex(*args, **kwargs)`  
    通过唯一索引获取顶点迭代器. 有多个重载版本:  
    1. `GetVertexByUniqueIndex(self, label_name: str, field_name: str, field_value_string: str) → lgraph_api::VertexIterator`  
       *Gets vertex iterator by unique index. Throws exception if there is no such vertex. label_name specifies the name of the indexed label. field_name specifies the name of the indexed field. field_value_string specifies the string representation of the indexed field value.*
    2. `GetVertexByUniqueIndex(self, label_name: str, field_name: str, field_value: object) → lgraph_api::VertexIterator`  
       *Gets vertex iterator by unique index. Throws exception if there is no such vertex. label_name specifies the name of the indexed label. field_name specifies the name of the indexed field. field_value specifies the indexed field value.*
    3. `GetVertexByUniqueIndex(self, label_id: int, field_id: int, field_value: liblgraph_python_api.FieldData) → lgraph_api::VertexIterator`  
       *Gets vertex iterator by unique index. Throws exception if there is no such vertex. label_id specifies the id of the indexed label. field_id specifies the id of the indexed field. field_value is a FieldData specifying the indexed field value. *

  - `GetVertexFieldId(self: liblgraph_python_api.Transaction, label_id: int, field_name: str) → int`  
    获取与此 (label_id, field_name) 关联的顶点字段 ID。  
    *Gets the vertex field ID associated with this (label_id, field_name).*

  - `GetVertexFieldIds(self: liblgraph_python_api.Transaction, label_id: int, field_names: List[str]) → List[int]`  
    获取与此 (label_id, field_names) 关联的顶点字段 ID。  
    *Gets the vertex field IDs associated with this (label_id, field_names).*

  - `GetVertexIndexIterator(*args, **kwargs)`  
    获取顶点索引迭代器. 有多个重载版本:  
    1. `GetVertexIndexIterator(self, label_id: int, field_id: int, key_start: liblgraph_python_api.FieldData, key_end: liblgraph_python_api.FieldData) → lgraph_api::VertexIndexIterator`  
       Gets an VertexIndexIterator pointing to the indexed item which has index value [key_start, key_end]. key_start=key_end=v returns an iterator pointing to all vertexes that has field value v. label_id specifies the id of the indexed label. field_id specifies the id of the indexed field. key_start is a FieldData containing the minimum indexed value. key_end is a FieldData containing the maximum indexed value.
    2. `GetVertexIndexIterator(self, label_id: int, field_id: int, value: liblgraph_python_api.FieldData) → lgraph_api::VertexIndexIterator`  
       label_id specifies the id of the indexed label. field_id specifies the id of the indexed field. value is a FieldData containing the indexed value.

  - `GetVertexIterator(*args, **kwargs)`  
    获取顶点迭代器. 有多个重载版本:  
    1. `GetVertexIterator(self: liblgraph_python_api.Transaction) → lgraph_api::VertexIterator`  
       返回指向第一个顶点的迭代器.  
    2. `GetVertexIterator(self: liblgraph_python_api.Transaction, vid: int) → lgraph_api::VertexIterator`  
       返回指向指定顶点的迭代器.  

  - `GetVertexLabelId(self: liblgraph_python_api.Transaction, label_name: str) → int`  
    获取顶点标签 ID.  
    *Gets the vertex label ID associated with this label.*

  - `GetVertexSchema(self: liblgraph_python_api.Transaction, label_name: str) → List[liblgraph_python_api.FieldSpec]`  
    获取顶点标签的模式说明.  
    *Gets the schema specification of the vertex label.*

  - `IsReadOnly(self: liblgraph_python_api.Transaction) → bool`  
    检查事务是否为只读.  
    *Checks if the transaction is read-only.*

  - `IsValid(self: liblgraph_python_api.Transaction) → bool`  
    检查事务是否有效.  
    *Checks if the transaction is valid.*

  - `IsVertexIndexed(self: liblgraph_python_api.Transaction, label_name: str, field_name: str) → bool`  
    检查指定字段是否被索引.  
    *Tells whether the specified field is indexed.*

  - `ListEdgeLabels(self: liblgraph_python_api.Transaction) → List[str]`  
    获取所有边标签的列表.  
    *Gets the list of all edge labels in the DB.*

  - `ListVertexLabels(self: liblgraph_python_api.Transaction) → List[str]`  
    获取所有顶点标签的列表.  
    *Gets the list of all vertex labels in the DB.*

  - `UpsertEdge(*args, **kwargs)`  
    更新或插入边. 有多个重载版本:  
    
    1. `UpsertEdge(self: liblgraph_python_api.Transaction, src: int, dst: int, label_name: str, field_names: List[str], field_value_strings: List[str]) → bool`  
       从 `src` 到 `dst` 插入或更新一条边，使用指定的标签名、字段名和字段值（字符串格式）。如果已经存在 `src->dst` 边，则使用新值更新；否则创建新边。如果边被创建，返回 True；如果边被更新，返回 False。未在 `field_names` 中的字段被视为 null。  
       *Upserts an edge from src to dst with the specified label name, field names, and field values in string format. If an src->dst edge already exists, it is updated with the new value. Otherwise, a new edge is created. Returns True if the edge is created, False if the edge is updated. Fields that are not in field_names are considered null.*

    2. `UpsertEdge(self: liblgraph_python_api.Transaction, src: int, dst: int, label_id: int, field_ids: List[int], field_values: List[liblgraph_python_api.FieldData]) → bool`  
       从 `src` 到 `dst` 插入或更新一条边，使用指定的标签 ID、字段 ID 和字段值。如果已经存在 `src->dst` 边，则使用新值更新；否则创建新边。如果边被创建，返回 True；如果边被更新，返回 False。未在 `field_names` 中的字段被视为 null。  
       *Upserts an edge from src to dst with the specified label id, field ids, and field values. If an src->dst edge already exists, it is updated with the new value. Otherwise, a new edge is created. Returns True if the edge is created, False if the edge is updated. Fields that are not in field_names are considered null.*

    3. `UpsertEdge(self: liblgraph_python_api.Transaction, src: int, dst: int, label_name: str, field_names: List[str], field_values: List[liblgraph_python_api.FieldData]) → bool`  
       从 `src` 到 `dst` 插入或更新一条边，使用指定的标签名、字段名和字段值。如果已经存在 `src->dst` 边，则使用新值更新；否则创建新边。如果边被创建，返回 True；如果边被更新，返回 False。未在 `field_names` 中的字段被视为 null。  
       *Upserts an edge from src to dst with the specified label name, field names, and field values. If an src->dst edge already exists, it is updated with the new value. Otherwise, a new edge is created. Returns True if the edge is created, False if the edge is updated. Fields that are not in field_names are considered null.*

    4. `UpsertEdge(self: liblgraph_python_api.Transaction, src: int, dst: int, label_name: str, value_dict: dict) → bool`  
       从 `src` 到 `dst` 插入或更新一条边，使用指定的标签，并填充 `value_dict` 中给定的值。如果已经存在 `src->dst` 边，则使用新值更新；否则创建新边。如果边被创建，返回 True；如果边被更新，返回 False。未在 `value_dict` 中的字段被视为 null。  
       *Upserts an edge from src to dst with the specified label, and fill it with the values given in value_dict. If an src->dst edge already exists, it is updated with the new value. Otherwise, a new edge is created. Returns True if the edge is created, False if the edge is updated. Fields that are not in value_dict are considered null.*
 

  - `VertexToString(self: liblgraph_python_api.Transaction, vid: int) → str`  
    返回指定顶点的字符串表示.  
    *Returns the string representation of the vertex specified by vid.*



### `liblgraph_python_api.VertexIndexIterator`
`VertexIndexIterator` 用于检索索引顶点的 ID。顶点 ID 按照 (index_value, vertex_id) 的升序排序。
 *VertexIndexIterator can be used to retrieve the id of indexed vertices. Vertex ids are sorted in ascending order of (index_value, vertex_id).*
- **方法 Methods**:
  - `GetIndexValue(self: liblgraph_python_api.VertexIndexIterator) → liblgraph_python_api.FieldData`  
    获取索引值。由于顶点 ID 按照 (index_value, vertex_id) 的顺序排序，调用 `Next()` 可能会改变当前的索引值。
    *Gets the indexed value. Since vertex ids are sorted in (index_value, vertex_id) order, calling Next() may change the current indexed value.*

  - `GetVid(self: liblgraph_python_api.VertexIndexIterator) → int`  
    获取当前指向的顶点 ID。
    *Gets the id of the vertex currently pointed to.*

  - `IsValid(self: liblgraph_python_api.VertexIndexIterator) → bool`  
    检查此迭代器是否有效。
    *Tells whether this iterator is valid.*

  - `Next(self: liblgraph_python_api.VertexIndexIterator) → bool`  
    移动到下一个索引的顶点 ID。如果在指定的键范围内没有更多顶点，迭代器将变为无效。
    *Goes to the next indexed vid. If there is no more vertex within the specified key range, the iterator becomes invalid.*




### `liblgraph_python_api.VertexIterator`
`VertexIterator` 可以用来检索一个顶点的信息，或扫描多个顶点。顶点按照其 ID 的升序排列。

- **方法 Methods**:
  - `Delete(self: liblgraph_python_api.VertexIterator) → Tuple[int, int]`  
    删除当前顶点。如果有下一个顶点，迭代器将指向下一个顶点。  
    *Deletes current vertex. The iterator will point to the next vertex if there is any.*

  - `GetAllFields(self: liblgraph_python_api.VertexIterator) → dict`  
    获取所有字段值，并以字典形式返回。  
    *Gets all the field values and return as a dict.*

  - `GetField(*args, **kwargs)`  
    重载函数。

    1. `GetField(self: liblgraph_python_api.VertexIterator, field_name: str) → object`  
       获取指定字段名的字段值。  
       *Gets the field value of the field specified by field_name.*

    2. `GetField(self: liblgraph_python_api.VertexIterator, field_id: int) → object`  
       获取指定字段 ID 的字段值。  
       *Gets the field value of the field specified by field_id.*

  - `GetFields(*args, **kwargs)`  
    重载函数。

    1. `GetFields(self: liblgraph_python_api.VertexIterator, field_names: List[str]) → list`  
       获取指定字段名的字段值。  
       *Gets the field values of the fields specified by field_names.*

    2. `GetFields(self: liblgraph_python_api.VertexIterator, field_ids: List[int]) → list`  
       获取指定字段 ID 的字段值。  
       *Gets the field values of the fields specified by field_ids.*

  - `GetId(self: liblgraph_python_api.VertexIterator) → int`  
    获取此顶点的整数 ID。GraphDB 为每个顶点分配一个整数 ID。  
    *Gets the integer id of this vertex. GraphDB assigns an integer id for each vertex.*

  - `GetInEdgeIterator(*args, **kwargs)`  
    重载函数。

    1. `GetInEdgeIterator(self: liblgraph_python_api.VertexIterator) → lgraph_api::InEdgeIterator`  
       获取指向此顶点的第一个入边的 InEdgeIterator。  
       *Gets an InEdgeIterator pointing to the first in-coming edge of this edge.*

    2. `GetInEdgeIterator(self: liblgraph_python_api.VertexIterator, arg0: liblgraph_python_api.EdgeUid, arg1: bool) → lgraph_api::InEdgeIterator`  
       获取 EdgeUid 为 euid 的此顶点的入边 InEdgeIterator。  
       *Gets an InEdgeIterator pointing to the in-edge of this vertex with EdgeUid==euid.*

  - `GetLabel(self: liblgraph_python_api.VertexIterator) → str`  
    获取当前顶点的标签名。  
    *Gets the label name of current vertex.*

  - `GetLabelId(self: liblgraph_python_api.VertexIterator) → int`  
    获取当前顶点的标签 ID。  
    *Gets the label id of current vertex.*

  - `GetNumInEdges(self: liblgraph_python_api.VertexIterator, n_limit: int = 18446744073709551615) → Tuple[int, bool]`  
    获取此顶点的入边数量。n_limit 指定扫描的最大边数。返回一个包含入边数量和一个布尔值的元组，指示是否超过限制。  
    *Gets the number of in-coming edges of this vertex. n_limit specifies the maximum number of edges to scan. Returns a tuple containing the number of in-edges and a bool value indicating whether the limit is exceeded.*

  - `GetNumOutEdges(self: liblgraph_python_api.VertexIterator, n_limit: int = 18446744073709551615) → Tuple[int, bool]`  
    获取此顶点的出边数量。n_limit 指定扫描的最大顶点数。返回一个包含出边数量和一个布尔值的元组，指示是否超过限制。  
    *Gets the number of out edges of this vertex. n_limit specifies the maximum number of vids to scan. Returns a tuple containing the number of out-edges and a bool value indicating whether the limit is exceeded.*

  - `GetOutEdgeIterator(*args, **kwargs)`  
    重载函数。

    1. `GetOutEdgeIterator(self: liblgraph_python_api.VertexIterator) → lgraph_api::OutEdgeIterator`  
       获取指向此顶点的第一个出边的 OutEdgeIterator。  
       *Gets an OutEdgeIterator pointing to the first out-going edge of this edge.*

    2. `GetOutEdgeIterator(self: liblgraph_python_api.VertexIterator, arg0: liblgraph_python_api.EdgeUid, arg1: bool) → lgraph_api::OutEdgeIterator`  
       获取 EdgeUid 为 euid 的此顶点的出边 OutEdgeIterator。  
       *Gets an OutEdgeIterator pointing to the out-edge of this vertex with EdgeUid==euid.*

  - `Goto(self: liblgraph_python_api.VertexIterator, vid: int, nearest: bool) → bool`  
    转到指定 ID 的顶点。如果 nearest==true，转到 ID >= vid 的最近顶点。  
    *Goes to the vertex specified by vid. If nearest==true, go to the nearest vertex with id>=vid.*
    
  - `IsValid(self: liblgraph_python_api.VertexIterator) → bool`  
    检查当前迭代器是否有效。  
    *Checks if the current iterator is valid.*

  - `ListDstVids(self: liblgraph_python_api.VertexIterator, n_limit: int = 18446744073709551615) → Tuple[List[int], bool]`  
    列出所有出边的目标顶点 ID。n_limit 指定返回的最大顶点数。返回一个包含顶点列表和一个布尔值的元组，指示是否超过限制。  
    *Lists all destination vids of the out edges. n_limit specifies the maximum number of vids to return. Returns a tuple containing a list of vids and a bool value indicating whether the limit is exceeded.*

  - `ListSrcVids(self: liblgraph_python_api.VertexIterator, n_limit: int = 18446744073709551615) → Tuple[List[int], bool]`  
    列出所有入边的源顶点 ID。n_limit 指定返回的最大源顶点数。返回一个包含顶点列表和一个布尔值的元组，指示是否超过限制。  
    *Lists all source vids of the in edges. n_limit specifies the maximum number of src vids to return. Returns a tuple containing a list of vids and a bool value indicating whether the limit is exceeded.*

  - `Next(self: liblgraph_python_api.VertexIterator) → bool`  
    转到 ID 大于 {current_vid} 的下一个顶点。  
    *Goes to the next vertex with id>{current_vid}.*

  - `SetField(self: liblgraph_python_api.VertexIterator, field_name: str, field_value_object: object) → None`  
    设置指定字段的值。  
    *Sets the specified field.*

  - `SetFields(*args, **kwargs)`  
    重载函数。

    1. `SetFields(self: liblgraph_python_api.VertexIterator, field_names: List[str], field_value_strings: List[str]) → None`  
       使用字符串表示形式的字段值设置指定字段名的字段。  
       *Sets the fields specified by field_names with field values in string representation.*

    2. `SetFields(self: liblgraph_python_api.VertexIterator, field_names: List[str], field_values: List[liblgraph_python_api.FieldData]) → None`  
       使用新值设置指定字段名的字段。  
       *Sets the fields specified by field_names with new values.*

    3. `SetFields(self: liblgraph_python_api.VertexIterator, value_dict: dict) → None`  
       使用 value_dict 中指定的值设置字段。  
       *Sets the fields with values as specified in value_dict.*

    4. `SetFields(self: liblgraph_python_api.VertexIterator, field_ids: List[int], field_values: List[liblgraph_python_api.FieldData]) → None`  
       使用字段值设置指定字段 ID 的字段。  
       *Sets the fields specified by field_ids with field values.*

  - `ToString(self: liblgraph_python_api.VertexIterator) → str`  
    返回当前顶点的字符串表示，包括属性和边。  
    *Returns the string representation of current vertex, including properties and edges.*
