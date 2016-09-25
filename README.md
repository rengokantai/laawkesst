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

####Exploring basic input-field separators
we can set multiple seperator
```
awk -F '[,!]' '{print $2}' file.txt
```
####Specifying field and record separators with variables
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
###9 Combining 
sum all txt file's size
```
ls -l |awk '/\.txt$/{total +=$5; print}END{print total}'
```
