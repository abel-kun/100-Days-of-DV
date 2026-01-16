# Polymorphism and Casting in SystemVerilog
--------------------------------------------------------
Q. What is Polymorphism?
--------------------------------------------------------
### Polymorphism in SV means that, a class handle of a given type can be assigned to any inherited class instance.

Consider a base class called baseframe.
```systemverilog
class baseframe;
  
  function void iam();
    $display("This is BaseFrame");
  endfunction
  
endclass
```
From this base class we have extended / derived classes called shortframe, mediumframe and longframe.
```systemverilog
class shortframe extends baseframe;
  
  int s_data;
  
  function void iam();
    $display("This is ShortFrame");
  endfunction
  
endclass
```

When we declare an object, the object has a handle type and a handle name.
```systemverilog  
  baseframe bf;				//baseframe handle 
  shortframe sf = new();	//shortfram handle with a new object instance
  shortframe sf1;
```
Polymorphism would mean that, base class handle can be assigned with any subclass handle.
```
bf = sf;  ✔️
```
But the reverse is not possible, i.e., a subclass handle cannot be assigned with a base class handle
```
sf = bf;  ❎
```
Now when we call a method of from the bf, although bf is assigned with sf handle, the .iam() method will be invoked based on the TYPE of handle, which in this case for bf is baseframe handle
```systemverilog
bf.iam();                     
KERNEL Output: # This is BaseFrame
```
If we try to access the members of the shortframe from the bf handle, it is not possible.
Even hough bf is assigned with sf handle, by default, only base class members are accessible from the handle and not s_data which is a member of shortframe.
```systemverilog
bf.s_data = 15;                   
KERNEL Output: ** Error (suppressible): (vlog-13276) testbench.sv(35:4): Could not find field/method name (s_data) in 'bf' of 'bf.s_data'.
```
---------------------------------------------------------------------------------------------------------------
### Casting
A parent or base class handle cannot be copied to a subclass handle.

To workaround this, we can create one more handle of type shortframe and then assign the baseframe handle (bf) to the new shortframe handle (sf2).
    
Directly assigning sf2 with bf cannot be done, that is because, we are assuming that baseframe handle (bf) is pointing at a shortframe instance but that assumption can be wrong.
```
sf1 = bf;  ❎ 
```
    
This is where comes the concept of casting.
We need to check if the baseframe handle is actually pointing at a shortframe handle, which can be done by calling a $cast.

$cast copies contents of bf (using a pointer) into sf1 and checks if that pointer is pointing at an instance in memory which is compatible with the shortframe handle (sf1).
   
    **NOTE: $cast is actually a subroutine and can be defined as both function and task.
    It is a good practise to use $cast as a function as it returns false if cast fails which can further be handled.
    If used as a task, it will throw a run-time error with no recovery.

```systemverilog
$cast(sf1, bf); 		//called as a task
```
```systemverilog
//as a function
    if($cast(sf1, bf))
      $display("Cast successfull");
    else
      $display("Cast failed");
```
After $cast is successfull, the members and methods of the subclass can now be accessed via sf2 handle.
```    
sf1.iam();
KERNEL Output: 
    # Cast successfull
    # This is ShortFrame
```
------------------------------------------------------------------------------------------------------------------

Source: https://www.youtube.com/watch?v=KWCOxI0mL80

----------------------------------------------------------------------------------------------------
Try here:

<a href="https://edaplayground.com/x/JLQk" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="70" height="70">
</a>

----------------------------------------------------------------------------------------------------
