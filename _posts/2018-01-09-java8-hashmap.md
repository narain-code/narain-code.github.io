---
layout: post
title: "Java 8 HashMap Performance Improvements"
date: 2018-01-09
---

 HashMap<K,V> is my go to datastructure for lookups in my java programs.HashMap implementation provides constant-time performance for the basic operations get and put assuming the hash function disperses the elements properly among the buckets. It uses hashCode() method of keys and and spreads the keys into buckets by XOR higher bits of hashCode.As a result hashes that vary only in bits above the current mask will always collide. (Among known examples are sets of Float keys holding consecutive whole numbers in small tables.).
