See details about hardware 

````
sudo dmidecode
````

See details about CPU
````
lscpu
````

Print exit code
````
echo $?
````

Date

````
NOW=$(TZ=GMT date +"%Y-%m-%dT%H:%M:%SZ")
````

````
cat /proc/cpuinfo
````

hostname --ip-address

hostname -f

## Memory

free -h

## Services

````
less /etc/services
````

## Networking

Check open ports
````
lsof -i -P -n 
ss -tulpn
lsof -i:22
````