``` makefile
j=j++：
temp = j；
j = j+1;
j = temp;

j=++j:
j = j+1;
temp = j ;
j = temp;
```
*注意 ：j++,++j 单独用还是没问题的。*