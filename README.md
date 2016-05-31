#### laawkesst
#####9 Combining 
sum all txt file's size
```
ls -l |awk '/\.txt$/{total +=$5; print}END{print total}'
```
