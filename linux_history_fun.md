# Linux History Móka
### 1.  Mindenki hozzon létre magának az alábbi szerveren egy felhasználót. Felhasználónév szabadon választható.
### 2. A felhasználódnak legyen saját home könyvtára.
```bash
# -s nem szükséges, mert a bash az alapértelmezett, de már itt lehetne mást beállítani
useradd -m zsolt -s "/bin/bash"
passwd zsolt

#hogy később fel tudjam telepíteni a zshellt
usermod -aG sudo zsolt
exit
```

### 3.  Jelentkezz ki a root felhasználóval, és jelentkezz be a sajátoddal ssh-n keresztül. 
```bash
ssh zsolt@146.190.37.189
```

### 4.  Változtasd meg az alapértelmezett shell-t bármelyik másikra, de mindenképp változtass az alapértelmezettet.
```bash
#fellelhető shellek
cat /etc/shells

#nincs még zsh ezért telepítem (nem kötelező, nehezíti a dolgokat...)
sudo apt install zsh

#váltás bash-ról zsh-re 
chsh -s /bin/zsh
exit

ssh zsolt@146.190.37.189
```

#### zshell menü beállítás
```bash
#zshell beállítás
1 
1

1 enter
2 HISTFILE> ~/.bash_history
3 enter
0
0
```

### 5.  Hozd létre a {neved}.html nevű fájlt és irányítsd bele a historyd.
```bash
#így csak ömlesztve lesz, nincs új sor
history > $USER.html
```

### 6.  Írj egy bash scriptet, ami megvalósítja az 5. pontot. Ezután átmásolja a létrejött állományt ide: /var/www/html/

```bash
nano mukodj.sh
```
#### fapados script
```bash
#!/bin/bash
history_file="/home/zsolt/.bash_history"
echo "" > /home/zsolt/zsolt.html

while read line
do
    echo "$line<br>" >> /home/zsolt/zsolt.html
done < $history_file

cp /home/zsolt/zsolt.html /var/www/html/zsolt.html
```

#### pimped
```bash
#!/bin/bash
history_file="/home/zsolt/.bash_history"
# ürítés
echo "" > /home/zsolt/zsolt.html

# html kód teteje
top="html_head.txt"
while read line
do
    echo "$line" >> /home/zsolt/zsolt.html
done < $top

# maga a tartalom
while read line
do
    echo "<li class="list-group-item">$line</li>" >> /home/zsolt/zsolt.html
done < $history_file

# html kód alja
bottom="html_tail.txt"
while read line
do
    echo "$line" >> /home/zsolt/zsolt.html
done < $bottom

# publikálás
cp /home/zsolt/zsolt.html /var/www/html/zsolt.html
```


#### random színezés
```bash
#!/bin/bash
history_file="/home/teemo/.bash_history"
# ürítés
echo "" > /home/teemo/zsolt.html

#random színezéshez
declare -a array=("list-group-item-primary"
                "list-group-item-secondary"
                "list-group-item-success"
                "list-group-item-danger"
                "list-group-item-warning"
                "list-group-item-info")
random_szin=${array[$RANDOM % 6]}

# html kód teteje
top="html_head.txt"
while read line
do
    echo "$line" >> /home/teemo/zsolt.html
done < $top

# maga a tartalom
while read line
do
    echo "<li class=\"list-group-item $random_szin\">$line</li>" >> /home/teemo/zsolt.html
done < $history_file

# html kód alja
bottom="html_tail.txt"
while read line
do
    echo "$line" >> /home/teemo/zsolt.html
done < $bottom

# publikálás
cp /home/teemo/zsolt.html /var/www/html/zsolt.html
```

#### html head
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bash history</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
  </head>
  <body>
    <ul class="list-group">
```

#### html tail
```html
    </ul>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>
  </body>
</html>
```

### 7.  Adj futtatási jogot mindenkinek a scripthez.
```bash
chmod 711 mukodj.sh
```

### 8.  Írj egy cron job-ot ami 5 percenként meghívja a 6. pontban létrehozott scriptet.
```bash
crontab -e

*/5 * * * * /home/zsolt/mukodj.sh
```

---

### history automatikus mentés
https://askubuntu.com/questions/67283/is-it-possible-to-make-writing-to-bash-history-immediate
#### .bashrc
```bash
export PROMPT_COMMAND="history -a;/home/zsolt/index_builder.sh;$PROMPT_COMMAND"

export PROMPT_COMMAND="history -a;$PROMPT_COMMAND"
```
