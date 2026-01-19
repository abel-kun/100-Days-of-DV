# Constrained Random Queue
----------------------------------------------------------------------------------------------------
Q. Write constraint for the below requirements:

    * The queue size will be 15. Randomize a queue such that it exactly has four 7 in it.
    * No 7’s should be at the consecutive next to each other.
----------------------------------------------------------------------------------------------------

The above constraint can be written as follows:

First, we limit the size of the queue to 15.
```systemverilog
  constraint con_1{
    queue.size() == 15;
  }
```
Next, the requirement is, the queue should have exactly four 7s. For this we can make use of the built-in array methods.
```systemverilog
  constraint con_2{
    (queue.sum() with (item == 7)) == 4;
  }
``` 
.sum() is an array method that returns the sum of all array elements. Here, when we say, sum() with (item == 7), it would imply that the sum of all the elements that are 7. So, the queue is contrained to have exactly four 7s.

The other requirement is that, no 7 shuld be consecutive next to each other. This can be achieved using the following constraint.
```systemverilog
  constraint con_3{
    foreach(queue[i]){
      if(i>0) !((queue[i] == 7) && (queue[i-1] == 7));
    }
  }
``` 
The above constraint makes sure that the consecutive indices do not have 7 as the element.


----------------------------------------------------------------------------------------------------
Try here:

<a href="https://edaplayground.com/user/416269" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="70" height="70">
</a>

----------------------------------------------------------------------------------------------------
