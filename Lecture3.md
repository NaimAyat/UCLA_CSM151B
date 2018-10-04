# Lecture 3: October 4, 2018
## Visitor Pattern
* Enables definition of a new operation on an object structure without changing the classes of the objects
### First Approach: Instanceof and Type Casts
* The running Java example: summing an integer list.
  ```
  interface List {}
  class Nil implements List {}
  class Cons implements List {
     int head;
     List tail;
  }
  List l; // The List-object
  int sum = 0;
  boolean proceed = true;
  while (proceed) {
    if (l instanceof Nil)
      proceed = false;
    else if (l instanceof Cons) {
      sum = sum + ((Cons) l).head;
      l = ((Cons) l).tail;
      // Notice the two type casts!
    }
  }
  ```
  * Advantage: the code is written without touching the classes `Nil` and `Cons`
  * Drawback: The code constantly uses type casts and instanceof to determine what class of object it is considering
  
