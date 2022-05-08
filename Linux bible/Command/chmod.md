change permission
r=4
w=2
x=1

+=set permission
-=remove permission
```shell
chmod ugo filename
chmod 777 filename

chmod o+r filename
chmod u-w filename
```
where: (all number)
u=user
g=group
o=other
a=all(for +/- only)

options
-R=recursive for directory and its content
