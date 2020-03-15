最好的Java5种遍历HashMap数据的写法
====

### 通过EntrySet的迭代器遍历

***

```
Iterator < Entry < Integer, String >> iterator = coursesMap.entrySet().iterator();
while (iterator.hasNext()) {
    Entry < Integer, String > entry = iterator.next();
    System.out.print(entry.getKey() + ". ");
    System.out.println(entry.getValue());
}
```

输出结果

```
zzw:how_iterate_hashmap_in_java zzw$ java IterateHashMapExample 1
TIOBE Index for March 2020:
1. Java
2. C
3. Python
4. C++
5. C#
6. Visual Basic .NET
7. JavaScript
8. PHP
9. SQL
10. GO
```

***

### 通过KeySet的迭代器遍历

```
Iterator < Integer > iterator = coursesMap.keySet().iterator();
while (iterator.hasNext()) {
    Integer key = iterator.next();
    System.out.print(key + ". ");
    System.out.println(coursesMap.get(key));
}
```

输出结果

```
zzw:how_iterate_hashmap_in_java zzw$ java IterateHashMapExample 2
TIOBE Index for March 2020:
1. Java
2. C
3. Python
4. C++
5. C#
6. Visual Basic .NET
7. JavaScript
8. PHP
9. SQL
10. GO
```

***

### 通过ForEach循环遍历

```
for (Map.Entry < Integer, String > entry: coursesMap.entrySet()) {
    System.out.print(entry.getKey() + ". ");
    System.out.println(entry.getValue());
}
```

输出结果

```
zzw:how_iterate_hashmap_in_java zzw$ java IterateHashMapExample 3
TIOBE Index for March 2020:
1. Java
2. C
3. Python
4. C++
5. C#
6. Visual Basic .NET
7. JavaScript
8. PHP
9. SQL
10. GO
```

***

### 通过Lambda表达式遍历

```
coursesMap.forEach((key, value) -> {
    System.out.print(key + ". ");
    System.out.println(value);
});
```

输出结果

```
zzw:how_iterate_hashmap_in_java zzw$ java IterateHashMapExample 4
TIOBE Index for March 2020:
1. Java
2. C
3. Python
4. C++
5. C#
6. Visual Basic .NET
7. JavaScript
8. PHP
9. SQL
10. GO
```

***

### 通过Stream API遍历

```
coursesMap.entrySet().stream().forEach((entry) -> {
    System.out.print(entry.getKey() + ". ");
    System.out.println(entry.getValue());
});
```

输出结果

```
zzw:how_iterate_hashmap_in_java zzw$ java IterateHashMapExample 5
TIOBE Index for March 2020:
1. Java
2. C
3. Python
4. C++
5. C#
6. Visual Basic .NET
7. JavaScript
8. PHP
9. SQL
10. GO
```

[^ codesource]： 示例代码在[这里](https://github.com/davidzou/WonderingWall/tree/master/challenge/how_iterate_hashmap_in_java)
