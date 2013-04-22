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


Arrays were incorporated into Scala collections through two implicit conversions.
The first conversion mapped an Array[T] to an object of type ArrayOps. This made all sequence operations
available, and these methods would return objects of type Array. This works in all cases except when converting 
an array to a real sequence. This leads to the next type of conversion, from Array to WrappedArray. A WrappedArray
is a mutable vector that includes all vector operations. When using these conversions it is important to understand
that using a method on a WrappedArray returns another WrappedArray, whereas using a a method on an ArrayOps object
just returns an Array.

Scala contains a clause to avoid ambiguity when using arrays. The conversion from Array to ArrayOps is prioritized 
over Array to WrappedArray. This ensures that the WrappedArray type is only used when an array needs to be converted
to a sequence. Strings have a similar application.

Unlike in Java, Scala permits instance creations such as Array[T], with T being a type parameter. In order to implement 
this, additional runtime information describing type T must be acquired since Java does not have a uniform array 
representation. The Manifest mechanism in Scala 2.8 yields this information about type T with a Manifest[T] object. 
ClassManifest does the same, and these can be used knowing only the top-level class of type T. The GenericArray class 
in Scala 2.8 allows array creation without a class manifest, because they have a uniform representation in that all 
elements are kept in an Array[AnyRef]. GenericArray allows the programmer to use a generic array if the class manifest 
is impossible to get (with Manifest[T].) The only time GeneticArray is used is with the sortWith method of class Seq. 
*code example*

The new Scala collection framework fixes the array problems that have caused issues in the language. There are various 
benefits of this change. For one, much of the compiler "magic" is removed. Multiple features are new to Scala and very 
useful. Prioritization of implicates is allowed, manifests yield type information at runtime, and context bounds can be 
easier to use with some types of implicit arguments or parameters. 
