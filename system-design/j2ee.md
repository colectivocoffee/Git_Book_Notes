# J2EE

## Garbage Collection

## Pools vs Threads

## Synchronized annotation in Java

#### What is Synchronization in Java

* Synchronization in Java is achieved with the help of the keyword “synchronized”. This keyword can be used for methods or blocks or objects **`but cannot be used with classes and variables.`** A `synchronized piece of code allows only one thread to access and modify it at a given time.`
* However, a synchronized piece of code affects code performance as it increases the waiting time of other threads trying to access it. So a piece of code should be synchronized only when there is a chance for a race condition to occur. If not one should avoid it.

## Equal & HashCode

{% hint style="info" %}
The hash code is nothing but an integer value that is associated with each of the objects in Java.
{% endhint %}

The hashcode\(\) Method works in java by returning some hashcode value just as an Integer. This hashcode integer value is vastly used in some hashing based collections which are like HashMap, HashTable, HashSet, etc.. The hashcode\(\) method of Java is need to be overridden in every class which helps to override the methods like equal\(\).

Equal/Similar objects will produce the same hash code when the objects are equal up to the final extent. Unequal objects of hashcode\(\) don’t produce some distinct/different hash codes.

[https://www.educba.com/java-hashcode/?source=leftnav](https://www.educba.com/java-hashcode/?source=leftnav)

## Normalization

#### DB Normalization

Ensuring that databases are normalized.  
Normal forms and are numbered from one \(the lowest form of normalization, referred to as first normal form or 1NF\) through five \(fifth normal form or 5NF\)

* First normal form \(1NF\) sets the fundamental rules for an organized database
* Second normal form \(2NF\) further addresses the concept of removing duplicative data
* Third: Remove columns that are not dependent upon the primary key

Math 

## Abstract vs Interface

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Abstract</th>
      <th style="text-align:left">Interface</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Inherentence</td>
      <td style="text-align:left">only extend one class</td>
      <td style="text-align:left">
        <p>extend many interfaces</p>
        <p>(multiple inherentence)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">can have static methods</td>
      <td style="text-align:left">CANNOT have static methods</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">can have implementation within the abstract class</td>
      <td style="text-align:left">CANNOT have implementation within Interface</td>
    </tr>
    <tr>
      <td style="text-align:left">Keyword</td>
      <td style="text-align:left">abstract ... Extends</td>
      <td style="text-align:left">interface ... implements</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

## Java Servlet

Servlets are the Java programs that runs on the Java-enabled web server or application server. They are used to handle the request obtained from the web server, process the request, produce the response, then send response back to the web server.

The Servlet technology is similar to other Web server extensions such as PHP

**Stages of the Servlet Life Cycle**: The Servlet life cycle mainly goes through four stages,

* Loading a Servlet.
* Initializing the Servlet.    - init\(\)           invoking init\(\) method
* Request handling.           - service\(\)    request&response
* Destroying the Servlet.   - destroy\(\)   release all the references

### Servlet concurrency 

1. Your servlet service\(\) method should not access any member variables, unless these member variables are thread safe themselves.
2. Your servlet service\(\) should not reassign member variables, as this may affect other threads executing inside the service\(\) method.
3. Local variables are always thread safe.

