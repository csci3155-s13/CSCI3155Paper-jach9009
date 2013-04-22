Arrays in Scala
===============
By: Jake Charland, Taylor Kohn and Noelle Beaujon
-------------------------------------------------
Arrays are one of the quintessential tools used by Java programmers to accomplish things as simple as storing data 
in no particular order, or more complicated tasks like implementing a sorting algorithm on a set of data. Either way, 
arrays are likely the data structure chosen to implement these things. 

It turns out that implementing arrays in Scala is in fact a lot harder than it sounds for a number of reasons. 
Since Scala is built on the Java language, one would think that it would be as simple as using the already implemented 
version of arrays. This is not the case. In Java there are a total of nine different types of arrays: a reference type 
as well as byte, char, short, int, long, float, double, and Boolean. Because of this there are nine different arrays 
written into Java. In the case of Scala, we need to be able to incorporate it so that there is a single array type 
representation, and arrays can take the type T. For example you would be able to create an array by the query Array[T], 
where T is a type variable. As of now, only monomorphic array creations are allowed, so there is no generic array. 
Therefore, we cannot use the already implemented method to create this array. To get around this, Scala was written 
so that it “magically” dealt with this problem by having the compiler generate code based on the static types of 
expressions. This isn’t really a solution, instead it is just a way of dodging the problem without actually dealing 
with it head on. the only operations supported by Scala arrays are indexing, updating and getting length.

This at first may seem like an acceptable solution; I mean what does it really matter to the programmer how the 
arrays are being dealt with as long as the task is being accomplished? The problem is that in some situations with 
different combinations the memory actually leaks as well as having very poor performance. 

There were many different ideas when it came to fixing this solution. David MacIver suggested having an array type 
similar to that of Java which would be fast and interoperable, but also one that fit into the Scala collection hierarchy. 
This second type would have the potential to use all the methods already defined in Scala. MacIver later collaborated 
with Koltsov and proposed a slightly different idea. Whenever an array was an argument the compiler would simply split 
the method into both type versions of the array.
