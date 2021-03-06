#  RegExp Playground

This repository contains several regexp examples.


### Examples

### 1. Catastrophic Backtracking

This is Java example of backtracking catastrophic shown in [Coding Horror post](http://blog.codinghorror.com/regex-performance/)
This is a known behavior when using **nesting repetition operators**, that has been analyzed in [stackoverlow](http://stackoverflow.com/questions/15082010/stackoverflowerror-when-matching-large-input-using-regex), [regular expressions dedicated sites](http://www.regular-expressions.info/catastrophic.html).

It can be seen the exponential explosion that take to parse using regexp for a simple pattern: 

```
(x+x+)y+
```

Details of this issue can be found on the above links.


### Execute

```
mvn clean package
mvn exec:java -Dexec.mainClass="cl.devweb.regexp.exploit.CatastrophicBacktracking"
```



### Rule of Thumb - Solution


 The solution is simple. When nesting repetition operators, **make absolutely sure that there is only one way to match the same match**. If repeating the inner loop 4 times and the outer loop 7 times results in the same overall match as repeating the inner loop 6 times and the outer loop 2 times, you can be sure that the regex engine will try all those combinations.


### OUTPUT

```
 mvn exec:java -Dexec.mainClass="cl.devweb.regexp.exploit.CatastrophicBacktracking"
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building regexp-exploit 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- exec-maven-plugin:1.4.0:java (default-cli) @ regexp-exploit ---
Found value: xxxxxxxxxxxxxy  timer=113.2 μs
Invalid match of 1 chars took 20.97 μs
Invalid match of 2 chars took 14.28 μs
Invalid match of 3 chars took 21.82 μs
Invalid match of 4 chars took 149.2 μs
Invalid match of 5 chars took 26.92 μs
Invalid match of 6 chars took 43.73 μs
Invalid match of 7 chars took 80.80 μs
Invalid match of 8 chars took 135.6 μs
Invalid match of 9 chars took 207.1 μs
Invalid match of 10 chars took 240.6 μs
Invalid match of 11 chars took 442.9 μs
Invalid match of 12 chars took 848.0 μs
Invalid match of 13 chars took 1.623 ms
Invalid match of 14 chars took 2.504 ms
Invalid match of 15 chars took 4.339 ms
Invalid match of 16 chars took 8.163 ms
Invalid match of 17 chars took 16.60 ms
Invalid match of 18 chars took 33.27 ms
Invalid match of 19 chars took 69.98 ms
Invalid match of 20 chars took 138.2 ms
Invalid match of 21 chars took 268.9 ms
Invalid match of 22 chars took 537.9 ms
Invalid match of 23 chars took 977.8 ms
Invalid match of 24 chars took 2.132 s
Invalid match of 25 chars took 4.260 s
Invalid match of 26 chars took 8.433 s
Invalid match of 27 chars took 17.18 s
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 34.977 s
[INFO] Finished at: 2016-03-16T12:38:59-03:00
[INFO] Final Memory: 9M/245M
[INFO] ------------------------------------------------------------------------
```

We can see taking a Thread Dump, where is spending (wasting?) most of the time:

```
"main" #1 prio=5 os_prio=31 tid=0x00007f8f9a006000 nid=0x1303 runnable [0x000000010a718000]
   java.lang.Thread.State: RUNNABLE
	at java.util.regex.Pattern$GroupHead.match(Pattern.java:4658)
	at java.util.regex.Pattern$Loop.match(Pattern.java:4785)
	at java.util.regex.Pattern$GroupTail.match(Pattern.java:4717)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$GroupHead.match(Pattern.java:4658)
	at java.util.regex.Pattern$Loop.match(Pattern.java:4785)
	at java.util.regex.Pattern$GroupTail.match(Pattern.java:4717)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$GroupHead.match(Pattern.java:4658)
	at java.util.regex.Pattern$Loop.match(Pattern.java:4785)
	at java.util.regex.Pattern$GroupTail.match(Pattern.java:4717)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$GroupHead.match(Pattern.java:4658)
	at java.util.regex.Pattern$Loop.match(Pattern.java:4785)
	at java.util.regex.Pattern$GroupTail.match(Pattern.java:4717)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$GroupHead.match(Pattern.java:4658)
	at java.util.regex.Pattern$Loop.match(Pattern.java:4785)
	at java.util.regex.Pattern$GroupTail.match(Pattern.java:4717)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$GroupHead.match(Pattern.java:4658)
	at java.util.regex.Pattern$Loop.match(Pattern.java:4785)
	at java.util.regex.Pattern$GroupTail.match(Pattern.java:4717)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$GroupHead.match(Pattern.java:4658)
	at java.util.regex.Pattern$Loop.match(Pattern.java:4785)
	at java.util.regex.Pattern$GroupTail.match(Pattern.java:4717)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$Curly.match0(Pattern.java:4272)
	at java.util.regex.Pattern$Curly.match(Pattern.java:4234)
	at java.util.regex.Pattern$GroupHead.match(Pattern.java:4658)
	at java.util.regex.Pattern$Loop.matchInit(Pattern.java:4801)
	at java.util.regex.Pattern$Prolog.match(Pattern.java:4741)
	at java.util.regex.Pattern$Start.match(Pattern.java:3461)
	at java.util.regex.Matcher.search(Matcher.java:1248)
	at java.util.regex.Matcher.find(Matcher.java:637)
	at cl.devweb.regexp.exploit.CatastrophicBacktracking.main(CatastrophicBacktracking.java:81)
```

### TODO

 - add more examples :)



### License

MIT
.
.

