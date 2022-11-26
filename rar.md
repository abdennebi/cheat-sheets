- Extract all rar files into directories

````
find . -type f -name "*.rar" -exec rar x "{}" "{}-dir/" \; 
````
