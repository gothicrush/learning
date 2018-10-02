tr)：返回字符串的长度(按字节，字母1个字节，汉字3个字节)

* 遍历字符串

  ```go
  r := []rune(str)
  for i := 0; i < len(r); i++ {
      fmt.Printf("%c\n",r[i])
  }
  ```

* 字符串转整数

  ```go
  import strconv
  
  n, err := strconv.Atoi("123")
  ```

* 整数转字符串

  ```go
  import strconv
  
  n := strconv.Itoa(123)
  ```

* 字符串转[]byte

  ```go
  var n []byte = []byte("hello")
  ```

* []byte转字符串

  ```go
  str := string([]byte("123456"))
  ```

* 10进制转2,4,8,16进制

  ```go
  str := strconv.FormatInt(132,2)
  str := strconv.FormatInt(132,4)
  str := strconv.FormatInt(132,5)
  str := strconv.FormatInt(132,16)
  ```
