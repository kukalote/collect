# Jdbctemplate中query、queryForObject、queryForList、queryForMap方法使用



### 方法的区别

|方法名|查询结果|备注|
|---|---|---|
|**query**|**返回结果是list，且list中元素必须是自定义bean；**|---|
|**queryForObject**|**查询出一条记录并封装到一个对象中。可以返回的是String、Integer、Double或者自定义bean。但是如果查询的记录为0条或者大于1条，对不起抛出异常。**|---|
|**queryForList**|**这个方法返回一个list，这个方法比较特殊，可以返回List<String> ，还可以返回list<Map<String,Objec>>**|---|
|**queryForMap**|**查询一行数据封装到Map中。如果查询多行记录抛出异常**|---|



### 方法示例

#### queryForList

```java
// queryForList 示例1
String sql = "select * from user where id = 1";
List list = jdbcTemplate.queryForList(sql);

// queryForList 示例2
String sql = "select name from test where name=?";
List<Map<String, String>> rows = jdbcTemplate.queryForList(sql, new Object[]{"name5"}, String.class);
for(int i=0;i<rows.size();i++){     //遍历    
    Map userMap = rows.get(i);    
    System.out.println(userMap.get("id"));      
    System.out.println(userMap.get("name"));      
    System.out.println(userMap.get("age"));    
}
```

#### queryForInt

```java
// queryForInt 示例1 查询一行数据并返回int型结果
String sql = "select count(*) from test";
Integer ret = jdbcTemplate.queryForInt(sql);
```

#### queryForMap

```java
// queryForMap 示例1 查询一行数据，并将该行数据转换为Map返回  
String sql = "select * from test where name='name5'";
Map<String, Object> map = jdbcTemplate.queryForMap(sql);    
map.get("name");
```

#### queryForObject

```java
// queryForObject 示例1 查询一行任何类型的数据，最后一个参数指定返回结果类型
// queryForObject(sql, requiredType)
String sql = "select count(*) from test";
jdbcTemplate.queryForObject(sql, Integer.class);

// queryForObject 示例2 queryForObject(sql, requiredType, args…)
String sql = "select count(*) from user where ID<? AND ID>?";
jdbcTemplate.queryForObject(sql, Integer.class,4,2);

// queryForObject 示例3 queryForObject(sql, args[], requiredType)
String sql = "select count(*) from user where ID>? AND USER_NAME LIKE ?";
jdbcTemplate.queryForObject(sql, new Object[]{1,"%哈%"},Integer.class);

// 示例4 queryForObject(sql, rowMapper)
String sql = "select * from user WHERE ID = 1";
User user = jdbcTemplate.queryForObject(sql, new RowMapper<User>(){
    @Override
    public User mapRow(ResultSet rs, int rowNum) throws SQLException {
        User user = new User();
        user.setId(rs.getInt("ID"));
        user.setUserName(rs.getString("USER_NAME"));
        return user;
    }
});
// 示例5 使用RowMapper的实现类:BeanPropertyRowMapper
String sql = "select * from user WHERE ID = 1";
User user = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(User.Class));
```

#### queryForRowSet

```java
//   queryForRowSet 示例1  查询一批数据，返回为SqlRowSet，类似于ResultSet，但不再绑定到连接上  
String sql = "select * from test";
SqlRowSet rs = jdbcTemplate.queryForRowSet(sql);   
```



