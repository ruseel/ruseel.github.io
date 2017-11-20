<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
# Shell and Job
						
MunShik JOUNG / stephen@vcnc.co.kr

!!!

# Can I connect to host:port 

* read manual
* change config (server setting, aws web console)
* not working 
* Did I read the wrong manual?
* Did I changed the wrong config file?
* Did I change config right? 
* Should I issue some reload command?
* or Blocked in firewall?

!!!

# Direct Evidence

!!!

```
nc -v -G 3 $HOST $PORT
```

!!!

```
$ nc -v -G 3 $H 22
```
nc: connectx to ec2-52-198-194-246.ap-northeast-1.compute.amazonaws.com port 22 (tcp) failed: Operation timed out

!!!

```
$ nc -v -G 3 ec2-54-65-58-168.ap-northeast-1.compute.amazonaws.com 22
```
found 0 associations
found 1 connections:
     1:	flags=82<CONNECTED,PREFERRED>
	outif en0
	src 10.10.120.77 port 52974
	dst 54.65.58.168 port 22
	rank info not available
	TCP aux info available

Connection to ec2-54-65-58-168.ap-northeast-1.compute.amazonaws.com port 22 [tcp/ssh] succeeded!

!!! 

* Within the server  <!-- .element: class="fragment" data-fragment-index="1" -->
* Within the same availability zone <!-- .element: class="fragment" data-fragment-index="2" -->
* Inside the firewall  <!-- .element: class="fragment" data-fragment-index="3" -->
* Outside of the firewall <!-- .element: class="fragment" data-fragment-index="4" -->
* Within the fast-five laptop <!-- .element: class="fragment" data-fragment-index="5" -->
* Within the docker <!-- .element: class="fragment" data-fragment-index="6" -->

!!! 

# reapeative run

!!! 

$ bin/spark-submit --master spark://ec2-54-250-152-172.ap-northeast-1.compute.amazonaws.com:7077 
--deploy-mode client --total-executor-cores 20 --executor-memory 4g "2017/09/01" "1-20170901" 1

!!!

$ bin/spark-submit --master spark://ec2-54-250-152-172.ap-northeast-1.compute.amazonaws.com:7077 
--deploy-mode client --total-executor-cores 20 --executor-memory 4g "2017/09/01" "1-20170901" 1

$ bin/spark-submit --master spark://ec2-54-250-152-172.ap-northeast-1.compute.amazonaws.com:7077 
--deploy-mode client --total-executor-cores 20 --executor-memory 4g "2017/09/02" "1-20170902" 1

$ bin/spark-submit --master spark://ec2-54-250-152-172.ap-northeast-1.compute.amazonaws.com:7077 
--deploy-mode client --total-executor-cores 20 --executor-memory 4g "2017/09/03" "1-20170903" 1

!!!

```
$ function a { date; echo "hello"; ls -al; }
$ a
```

!!!

```
$ function a { date; echo $1; ls -al; }
$ a
```

!!!

function run { 
$ bin/spark-submit --master spark://ec2-54-250-152-172.ap-northeast-1.compute.amazonaws.com:7077 
--deploy-mode client --total-executor-cores 20 --executor-memory 4g $1 $2 1;
}

!!!

run "2017/09/01" "1-20170901"
run "2017/09/02" "1-20170902"
run "2017/09/03" "1-20170903"

!!!

# some times use funciton help

!!!

* baltimore-migrate 
* baltimore-job-server

!!!

# baltimore-migrate 

Scala - Build with sbt 

!!!

# baltimore-job-server

Clojure - Build with lein but just upload source

!!! 

Want to transfer this many times with few stroke

!!! 

* run this then run that 
* success then proceed

!!!

```
a; b
```

```
a && b
```

!!!

# upload whole directory

```
tar czvf - directory-a | ssh $H tar xzvf - 
```

!!!

# "function" "&&" "upload whole directory"

!!!

function ubm {
        (cd baltimore-migrate \
           && sbt assembly \
           && scp target/scala-2.11/Baltimore-assembly-0.1.0-SNAPSHOT.jar $H:/baltimore)
}

function ujs {
        tar czvf - baltimore-job-server | ssh $H tar xzvf - -C "/baltimore"
}

ubm && ujs

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
# Job 

!!!

* count 1e3~5  <!-- .element: class="fragment" data-fragment-index="1" -->
* each job 10min ~ 10days <!-- .element: class="fragment" data-fragment-index="2" -->
* want to throttle under concurrency level ~1e2 <!-- .element: class="fragment" data-fragment-index="3" -->

!!!

# Retry

!!!

* failure occurred in specific time range 
* fallure occurred with specific input type 
* counterpart transitioned to malfunction state

!!!

# Job as value

!!!

## Job

```
{:year 2017 :month 9 :day 1 :baltimore-migrate.core/fn (fn [] ...)}
```

In-memory Queue

!!!

## Error 

```
{:year 2017 :month 9 :day 1 :baltimore-migrate.core/error "..."}
```
records/error/2017/9/1/job.edn

!!!

## Success

```
{:year 2017 :month 9 :day 1 :baltimore-migrate.core/fn (function [] ...)}
```
records/successed/2017/9/1/job.edn

!!!

## Log 

tmp/2017/9/1/log.txt

!!!

## Spot Instance 

Can be gone 

!!!

* create EBS volume separatly 
* mount again when instance gone by aws or by me


