---
layout: post
title: "Interview coding"
date: 2014-10-26 14:24
comments: true
published: false
categories: 
---

I have a mixed reaction to code questions, when not used carefully they have the ability to taint the interview process. In a regular setting no one writes code under the time contraints and stress levels that are present in technical interviews. But it makes sense to ask a candidate, who is interviewing for a programming position, to actually write some code and produce a solution to a problem. The issue is with the overly complex questions, if you ask the candidate a bad code question it can be very off-putting.

Code questions are a fact of tech interviewing,  so I am going to do some analysis of a few to determine if they are good questions to ask


## Case 1: Algorithm design
**Question:** Given 2 words, write a function that determines if the words are anagrams of eachother


If you've never seen this question before, I think the answer can be difficult to visualize, and a candidate who is under pressure may struggle. How the question is presented makes a surprising amount of difference, what is the essence of an anagram? It's a word in the english language, a string of characters that - when rearranged - corresponds to another word.

So the same question re-worded:

**Real Question:** Given 2 strings, write a function that determines if all characters are in both strings.



###Some answers from stackoverflow: 
- [Sort both words by characters and string compare](http://stackoverflow.com/a/15045791/782808)
- [Map each letter to a hash, keep counts, compare counts](http://stackoverflow.com/a/15045892/782808)
- [Map each letter to a prime number and sum the results](http://stackoverflow.com/a/17004897/782808)

The most common answer would be to iterate through the first string, searching for characters in the second string, while keeping track of duplicates. Sorting both strings is a shortcut to this process.

###Takeaways
Rewording the question to reduce cruft should be your first step. The strings being anagrams is **completely irrelevant**. You are being asked to find the intersection of 2 sets of characters. The interviewer did not pose the question this way because ability to reduce cruft is what is being tested.


## Case 2: Code design
An alternative to writing an algorithm is the code design question. Take the following example:

**Question:** Elevators have a relatively simple control mechanism, the user calls an elevator using a button `UP` or `DOWN`. Design an elevator control system using object oriented paradigms. 

The objective of this is to talk about an MVC approach to designing. A good answer should model `Elevator` class with getters/setters:

- `-(int) currentFloor`
- `-(int) destinationFloor`
- `-(void) setDestinationFloor:(NSInteger)destinationFloor`

and `Button` class:

- `-(int) floor`
- `-(Direction) callDirection`
- `-(void) setCallDirection:(NSInteger)callDirection`
- `-(void) clearCall`
- 
and `ElevatorController` class:

`public`

- `-(Elevator *) callAvailableElevatorFromFloor:(NSInteger)floor direction:(Direction)direction`

`private`

- `@property NSArray *calledButtons` - Of type `Button`
- `@property NSArray *elevators` - Of type `Elevator`

some followup questions:

- Describe the process of calling an elevator and getting to the destination floor
- How would you handle a button call of `UP` and `DOWN` from the same floor?

This has more of a natural feel to it than the algorithm question, it's is a problem programmers would actually face regularly. Athough this question feels easy initially, we could extend the idea to a chess game, or a 4 player card game.

