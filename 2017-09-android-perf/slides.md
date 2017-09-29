<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->
# FlameGraph and Systrace
						
MunShik JOUNG / stephen@vcnc.co.kr

!!!

## Brendan Gregg

!!!

![](http://0.0.0.0:8000/images/brendan-gregg-website.png "") 

!!!

![](https://images-na.ssl-images-amazon.com/images/I/61bqu49XX9L._SX408_BO1,204,203,200_.jpg "System Performance Book")

note: Amazon Link https://www.amazon.com/gp/product/0133390098/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0133390098&linkCode=as2&tag=deirdrestraug-20

!!!

![](https://images-na.ssl-images-amazon.com/images/I/5117F9ZCSXL._SX370_BO1,204,203,200_.jpg "")

!!!

![](https://images-na.ssl-images-amazon.com/images/I/41b-N8w2KkL._SX340_BO1,204,203,200_.jpg "")

!!!

## Youtube

Linux Performance Tools, Brendan Gregg

!!! 

## Profiling

https://poormansprofiler.org

!!!

# FlameGraph 

Visualization technique

!!!

note: TBD FlameGraph Image Bash

!!! 

## Data to Vis 

flamegraph.pl

!!!

## Many Area

* MySQL 
* Linux, Solaris
* Java, node.js, ruby 
* Android 

!!!

# aflame.rhye.org

![](images/aflame-rhye.png "")

!!!

# Trace

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

# Systrace

!!!

![](images/systrace-1.png "")

!!!

## Google I/O 2017

* Android Performance: Overview
* Android Performance: UI

!!!

## How to Capture
GUI vs CLI

!!!

## DDMS

```bash
export ANDROID_HVPROTO=ddm
$ANDROID_HOME/tools/monitor
```

!!

* Android Monitor (in Android Studio)
* DDMS == Android Device Monitor

!!

TBD android monitor screenshot

!!!

## systrace.py

```
$ANDROID_HOME/platform-tools/systrace/systrace.py \
  -a kr.co.vcnc.android.couple -o trace-xyz.html
```

!!

```
adb shell am force-stop $PKG && \
sleep 2 && \
F=~/Documents/trace-`date +%Y%m%d-%H%M`.html; \
$ANDROID_HOME/platform-tools/systrace/systrace.py \
 -b 32768 \
 gfx input view sched freq \
 -a kr.co.vcnc.android.couple \
 -o $F \
&& sleep 2 && open $F
```

note: 여러번 잡으려고 할 때 이렇게 했습니다. 반자동.

!!

E250K(Note2) does not allow us, root permission needed.

!!!

## RecyclerView Systrace 

note: TBD screen shot

!! 

## RecyclerView Source

note: TBD include source

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

## TRACE

!!!

## How to get Android TRACE File

```
# export ANDROID_SERIAL="0A3C2D8E0F00700A"

adb profile start <pid> /data/loca/tmp/couple-xyz.trace
adb profile stop
adb pull /data/loca/tmp/couple-xyz.trace
```

!!

## Android Monitor

note: TBD android monitor screen shot

!!!

## Upload Trace File 

aflame.rhye.org

note: TBD screenshot 

!!

* Sometimes failed 
* But mostly OK.

!!!

<!-- .slide: data-background="#5D6FA5" -->
<!-- .slide: data-state="terminal" -->

## WORKSHOP


