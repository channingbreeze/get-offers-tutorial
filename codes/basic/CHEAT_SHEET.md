### 排序

数组排序

```java
int[] arr = new int[] {7, 4, 1, 5, 6};
Arrays.sort(arr);
```

List排序

```java
List<Integer> list =  Arrays.asList(7, 4, 1, 5, 6);
Collections.sort(list);
```

数组转List

```java
int[] arr = new int[] {7, 4, 1, 5, 6};
List<Integer> list = Arrays.stream(arr).boxed().collect(Collectors.toList());
```

List转数组

```java
List<Integer> list =  Arrays.asList(7, 4, 1, 5, 6);
int[] arr = list.stream().mapToInt(Integer::valueOf).toArray();
```

