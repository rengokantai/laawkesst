## laawkesst
###2 AWK command line basics
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



###9 Combining 
sum all txt file's size
```
ls -l |awk '/\.txt$/{total +=$5; print}END{print total}'
```
