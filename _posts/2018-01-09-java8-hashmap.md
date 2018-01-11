---
layout: post
title: "Java 8 HashMap Performance Improvements"
date: 2018-01-09
---

HashMap<K,V> is my go to datastructure for lookups in my java programs. HashMap implementation provides constant-time performance for the basic operations get and put assuming the hash function disperses the elements properly among the buckets. It uses hashCode() method of keys and and spreads the keys into buckets. This is done by applying bitwise XOR operation on the 16 lower bits with the 16 higher bits of the hash and the result is used to find the bucket number in the hashmap. The bucket is found by applying bitwise & opertaion on the XOR result and number of buckets in the hashmap. As a result hashes that vary only in bits above the current mask will always collide. (Among known examples are sets of Float keys holding consecutive whole numbers in small tables.) 

Up until java7, when multiple values end up in the same bucket, values are placed in an ad-hoc linked list. In worst case, when all keys are mapped to the same bucket, the lookup in hash map degenerates to a linked list - from O(1) to O(n) lookup time. But starting with Java 8 when a bucket becomes too big (currently: TREEIFY_THRESHOLD = 8), HashMap dynamically replaces it with an implementation of tree map and there by changing the worst case performance from O(n) to O(logn).This optimization is described in [JEP-180](http://openjdk.java.net/jeps/180).
 On a average Java 8 hashmap 20% -25% faster than Java 7 in simple HashMap.get(). The overall performance is equally interesting: even with one million entries in a HashMap a single lookup taken less than 5 nanoseconds. But that's not what we were about to benchmark. If we were to produce where all the keys have the same hash then we can see the performance benefits of change in the Java 8 hashmap .
 To see the effect of in numbers see the below graph
 ![JavaHashMapPerformance-WithCollisions](/images/javahashmap-withcollisions.png){:class="img-responsive"}
 Results are along expected lines with the cost of HashMap.get() growing proportionally to the size of the HashMap itself in java 7. Since all entries are in the same bucket in one huge linked list, looking up one requires traversing half of such list (of size n) on average. Thus O(n) complexity as visualized on the graph.Here the Java 8 performance changes kicks in and you can see it in the graph. Probably a log scale would have been better for displaying the chart but i wanted to bring out the huge benefits of the change.
