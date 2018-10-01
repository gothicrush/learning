

  import time
  ```

* 获取当前时间

  ```go
  now := time.Now()
  ```

* 获取当前年月日，时分秒

  ```go
  now := time.Now()
  now.Year()
  now.Month()
  now.Day()
  now.Hour()
  now.Minute()
  now.Second()
  ```

* 日期时间格式化

  ```go
  // 方法1
  "%d/%d/%d %d:%d:%d",now.Year(),now.Month(),now.Day(),now.Hour(),now.Minute(),now.Second()
  
  // 方法2
  now.Format("2006/01/02 15:04:05")
  
  "2006/01/02 15:04:05" 这个时间固定的，必须这么写
  "2006/01/02 15:04:05" 中各个数字可以自由组合，这样可以格式化输出时间
  ```

* 时间常量

  ```go
  时    time.Hour
  分    time.Minute
  秒    time.Second
  毫秒  time.Millisecond
  微妙  time.Microsecond
  纳秒  time.Nanosecond
  ```

* 休眠

  ```go
  time.sleep(10 * time.Millisecond)
  ```

* 获取当前时间戳

  ```go
  time.Now().Unix()       // 从1970年到现在所经过的时间(单位秒)，类型int64
  time.Now().UnxiNano()   // 从1970年到现在所经过的时间(单位纳秒)，类型int64
  ```

