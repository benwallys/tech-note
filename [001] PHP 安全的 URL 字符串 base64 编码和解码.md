如果直接使用 base64_encode 和 base64_decode 方法的话，生成的字符串可能不适用URL地址。下面的方法可以解决该问题：

1. URL 安全的字符串编码：
```
function urlsafe_b64encode($string) {
   $data = base64_encode($string);
   $data = str_replace(array('+','/','='),array('-','_',''),$data);
   return $data;
}
```


2. URL 安全的字符串解码：
```
function urlsafe_b64decode($string) {
   $data = str_replace(array('-','_'),array('+','/'),$string);
   $mod4 = strlen($data) % 4;
   if ($mod4) {
       $data .= substr('====', $mod4);
   }
   return base64_decode($data);
}
```
