## Bitmap

### Bitmap是什么

* Bitmap译名为位图，其工作原理是用一个bit来标识一个数据

### Bitmap的作用

* 用于海量数据的排序，去重，查询是否存在

### Bitmap的使用

* setbit key offset value

  ```bash
  setbit unique 0 1
  setbit unique 5 1
  setbit unique 11 1
  setbit unique 15 1
  setbit unique 20 1
  ```

* getbit key offset

  ```bash
  getbit unique 0
  getbit unique 5
  getbit unique 11
  getbit unique 15
  getbit unique 20
  ```

* bitcount key [start end]

  * 获取指定范围start，end字节中bit为1的个数

  * 如果不指定start，end则为全部

    ```bash
    bitcount unique 1 2
    ```

* bitop and|or|xor key1 key2

  * 求两个位图的交集，并集，异或

### Bitmap的本质

* Bitmap本质是字符串





## HyperLogLog

### HyperLogLog是什么

* 基于HyperLogLog算法实现的数据类型

### HyperLogLog的作用

* 以极小空间完成数量的统计

### HyperLogLog的使用

* pfadd key element1 [element2...]
  * 添加元素
* pfcount key
  * 统计元素
* pfmerge key1 key2
  * 合并多个 HyperLogLog

### HyperLogLog的本质

* HyperLogLog的本质是字符串

### HyperLogLog的注意事项

* HyperLogLog可能会出现错误，概率为0.81%
* HyperLogLog只能用于统计数目，而不能存储实际的数据





## GEO

### GEO类型是什么

* 记录地理信息的经度和纬度

### GEO的作用

* 计算两地距离，范围等等

### GEO的使用

* geo key 经度 纬度 地方名
  * 添加新的 地方名-经度-纬度
* geopos key 地方名
  * 查找 地方名对应的经度和纬度
* geodist key 地方名1 地方名2
  * 计算两个地方名之间的距离

### GEO的本质

* GEO类型本质使用zset实现
* 删除 API：zrem key 地方名