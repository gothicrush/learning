* 查找字符串中有无特定子串

  ```go
  import strings
  
  b := strings.Contains("seafood", "food")
  ```

* 统计字符串中有几个特定子串

  ```go
  import strings
  
  c := strings.Count("ceeesseeee","e")
  ```

* 查找子串在字符串从左到右第一次出现的下标值，没有则返回-1

  ```go
  index := strings.Index("NTL_abc", "abc")
  ```

* 查找子串在字符串从右到左第一次出现的下标值，没有则返回-1

  ```go
  index := strings.LastIndex("NTL_abc", "abc")
  ```

* 如果字符串中有特定子串，则将其换为其他子串

  ```go
  str := strings.Replace("go hello", "go", "python")
  ```

* 字符串按某个子串进行分隔，返回slice

  ```go
  sl := strings.Split("hello-world-ok","-")
  ```

* 将字符串进行大小写转换

  ```go
  str := string.ToLower("HELLO")
  str := string.ToHigher("hello")
  ```

* 字符串去空格

  ```go
  去除左右两边的空格，strings.TrimSpace("  1  2  ")
  去除左右两边特定子串，strings.Trim("! 1 2 !", "!")
  去除左边特定子串，strings.TrimLeft("! 1 2 !", "!")
  去除右边特定子串，strings.TrimRight("! 1 2 !", "!")
  ```

* 判断字符串是否以某个子串开头/结尾

  ```go
  strings.HasPrefix("NLT_abc","NLT")
  strings.HasSuffix("NLT_abc","abc")
  ```
