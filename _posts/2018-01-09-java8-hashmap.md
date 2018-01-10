---
layout: post
title: "Java 8 HashMap Performance Improvements"
date: 2018-01-09
---

 HashMap<K,V> is my go to datastructure for lookups in my java programs.HashMap implementation provides constant-time performance for the basic operations get and put assuming the hash function disperses the elements properly among the buckets. It uses hashCode() method of keys and and spreads the keys into buckets. This is done by applying bitwise XOR operation on the lower bits withthe higher bits of the hash and the result is used to find the bucket number in the hashmap. The bucket is found by applying bitwise and opertaion on the XOR result and number of buckets in the hashmap. As a result hashes that vary only in bits above the current mask will always collide. (Among known examples are sets of Float keys holding consecutive whole numbers in small tables.)Up until java7, when multiple values end up in the same bucket, values are placed in an ad-hoc linked list. In worst case, when all keys are mapped to the same bucket, the lookup in hash map degenerates to a linked list - from O(1) to O(n) lookup time.But starting Java 8 when a bucket becomes too big (currently: TREEIFY_THRESHOLD = 8), HashMap dynamically replaces it with an implementation of tree map and there by changing the worst case performance from O(n) to O(logn).This optimization is described in [JEP-180](http://openjdk.java.net/jeps/180).
 To see the effect of in numbers see the below graphs
 ![JavaHashMapPerformance-WithCollisions](/images/javahashmap-withcollisions.png){:class="img-responsive"}
