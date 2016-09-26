## laawkesst
###2 AWK command line basics
####2 Working with records, fields, patterns, and actions
comma is a line seperator, not literal comma.
NF = number of fields (per line) no $ sign
```
awk '{print $2, $1}' file.txt  //ke hernan
awk '{print $2 $1}' file.txt  //kehernan
awk '{print $2 ", " $1}' file.txt  //ke, hernan
awk '{print NF, $0}' file.txt  //2 hernan ke
awk '/ke/{print NF, $0}' file.txt   //print all (whole)lines with `ke`
awk 'NF=3{print NF, $0}' file.txt  //print all (whole)lines with 3 fields
```
```
awk 'NF=3' file.txt
```
is same as
```
awk 'NF=3{print $0}' file.txt
```
multiple match matching (or)
```
awk '/ke/{print NF, $0} /hernan/{print NF, $0}' file.txt  /correct
```
note, it is not
```
awk '/ke/{print NF, $0}' '/hernan/{print NF, $0}' file.txt  //wrong!
```
If a line contains not only ke, but hernan, it will print twice
#####3Using AWK command-line flags
we have a file swap.txt (no single quote needed)
```
{print $2 $1}
```
we then use
```
awk -f swap.txt file.txt
```
-F = field seperator
```
awk -F, '{print $2}' file.txt
awk -F , '{print $2}' file.txt
awk -F t '{print $2}' file.txt   //t=tab
```

set var
```
awk -v hi=HELLO '{print $1, hi}'
```
###3. Understanding Records and Fields

####1 Exploring basic input-field separators
we can set multiple seperator
```
awk -F '[,!]' '{print $2}' file.txt
```
####2 Specifying field and record separators with variables
set FS to specify seperator
```
awk '{FS=","; print $2}'  //take effect on second line after
awk 'BEGIN{FS=","} {print $2}'  //first line
```
RS = recond seperator
```
awk 'BEGIN{RS="!";FS=","}{print $2}' file.txt
```

set internal var(seems like useless)
```
awk 'BEGIN{RS="";FS="\n"} {name=$1;addr=$2;print name ", " addr}' file.txt
```
OFS = output field seperator,default=space  
ORS = output field seperator,default=\n
```
awk 'BEGIN{OFS=", ";ORS="!"}{print NF, $0}' file.txt
```
####4 Solution: Change a CSV file to a tab-separated one
```
awk -F, '{print $1 "\t" $2 "\t" $3}' file.csv
```

###4. Understanding Variables and Operators
####1 Using built-in variables
If dealing with multiple files, NR=total record, FNR=record per file
```
awk '{print NR, FILENAME, FNR, $0}' file1.txt file2.txt
```
last field
```
awk '{print $NF}' file.txt
```
or
```
awk '{print $(NF-1)}' file.txt
```
####2 Creating user-defined variables
review
```
awk '{name=$1;addr=$2;print name ", " addr}' file.txt
```


this is accepted:
```
awk '{a=1;b=3; print a+b}'  //4
awk '{a=1;b=3; print a b}'  //13
```
num + string = num+0
```
awk '{a=1;b="a"; printa+b}'  //still 1
```


multiply has precedence over concat.
```
awk {a=1;b=2;c=3; print a b *c}' //16
awk {a=1;b=2;c=3; print (a b) *c}' //36
```
####3 Working with operators and arrays
awk only allows one d array.
```
awk '{a[1]=$1, a[2]=$2;print a[1],a[2]}' file.txt
```
###5 A Quick intro RE
####1 RE basics
Ex:
```
awk '/ke/{print}' file.txt
```
same as
```
awk '/ke/{print $0}' file.txt
```
print lines with /ke/ at 4th field (contains ke, maybe ke, key, ..)
```
awk '$4 ~ /ke/{print}' file.txt
```
/a.c/=abc=ac
####2 Working with character classes
Greedy and non-greedy. For HTML open close tag matching
```
/<.+>/       //match from open to clise
/<[^>]+>/      //open tag only
```




###6 Using Control Struct
####1. Building control structures
if statement with double layer braces
```
awk '{if (NF < 5) {print "shortline" ,$0}else{print"longline", $0}}' file.txt
```

####2 Creating an HTML table
```
BEGIN{
  FS="\t";
  print "<table>";
  print"<tr>";
  print"\t<th>header</th>";
  for(i=2;i<=5;i++){
    print "\t<th>Round" i "</th>";
  }
  print "</tr>";
}
{
  print"<tr>";
  print"\t<td>$1</td>";
  for(i=2;i<=5;i++){
    print "\t<th>"$i"</th>";
    total[i]+=$i;
  }
  print "</tr>";
}
END{
 print"<tr>";
  print"\t<td>average</td>";
  for(i=2;i<=5;i++){
    print "\t<td>"int(total[i]/NR)"</td>";
  }
  print "</tr>";
  print "</table>";
}
```
####4 Solution: Print only those lines consisting of a complete HTML entity
print complete HTML entities
```
BEGIN{
  FS='[<>]'
}
/^<.*>$/ {
  if($(NF-1_==("/"$2)){
    print;
  }
}
```
(this solution is very naive)


###7. Formatting the output
####1 Formatting output with printf()
syntax: printf(format,value...)  
Ex
```
awk -F ',' '{printf("%s\t%d\n",$1,$2)}' file.txt
```

####2. Formatting output with width and precision specifiers
with `-` it is left aligned. Otherwise right aligned
```
awk -F ',' '{printf("%-20s\t%-10d\n",$1,$2)}' file.txt
```
others: 6.2f->at least 6 char with 2 decimal points

###8. Functions and Arrays
####1 Manipulating strings
index(1-based)
```
index("abc","b") //2
```
match
```
match("abcd",/bc/) // 2, RSTART=2, RLENGTH=2
```
substr(str,start,len)  
Ex: remove `the`
```
BEGIN{
  target = "the";
}
{
  s=index($0,target);
  if(s==0){
    print;
  }else{
    print substr($0,1,s-1) substr($0,s+length(target));
  }
}
```
sub(each line's first occurance),gsub
note the sub function is in curly braces
```
awk '{sub(/ke/,"");print}' file.txt
```

split(field,arrayvar,splitor
```
awk -F, 'BEGIN{OFS='\t'} {split($1,arr,/ /); print a[2]", " a[1],$2}' file.txt
```

####2 Using associative arrays

###9 Combining 
sum all txt file's size
```
ls -l |awk '/\.txt$/{total +=$5; print}END{print total}'
```
