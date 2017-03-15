# 正则说明：

  * i  忽略大小写

  * 完全匹配时使用^...$；检验字符串是否正确时经常使用。如颜色正则：/^#[\da-f]{6}$/i.test('#ffffff');

# 常用
  
  * /#[\da-f]{6}/i 十六进制颜色; 示例：/#[\da-f]{6}/i.test('#ffffff');
  * /https?://[\S]*/i  网址Url
  * /[a-zA-Z0-9]+/  数字与字母
  * /[\u4e00-\u9fa5a-zA-Z0-9_]+/  字符字母数字下划线
  * /[^\x00-\xff]/  双字节字符（常用于中文检测）
  * /(\+86)?1[34578]\d{9}/  手机号
  * /(http|https)://[a-z|A-Z|0-9]+\\.(tmall|taobao|yao.[0-9]*)\\.(com|hk)(?=.*id)/i 淘宝或天猫
  * /seller_?id=(\w*).*activity_?id=(\w*)|activity_?id=(\w*).*seller_?id=(\w*)/i  优惠券链接
  * /((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)/ ip
  * /(http|https)://[a-z|A-Z]+\\.(.+)\\.(com|hk).+[?|&]id=(\\d+)/i 获取商品链接中的id
  * /*|and|exec|insert|select|delete|update|count|master|truncate|declare|char(|mid(|chr(|'|--|;/ 防注入过滤字符
