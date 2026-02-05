# Parallel Process
----------------------------------------------------------------------------------------------------
Q. Given you have a flag which you have to monitor till it is set. Create a timeout mechanism such that, if the flag is not set within the time, it throws an error.

-------------------------------------------------------------------------------------------
This problem can be resolved by making use of the fork-join processes.

We create a task which will take inputs as the flag to be monitored, the value to be monitored (0,1,x,z), and the timeout value.

```systemverilog
  task automatic check_timeout(ref logic flag,input logic value, input time timeout);
    bit timed_out;
    fork begin
      fork
        begin
          #timeout;
          $error("!!!!!-------->>>TIMEOUT<<<--------!!!!!");
          timed_out = 1;          
        end
      join_none        
      
      wait(flag === value || timed_out);
      disable fork;
        end join
    
  endtask
```
We are using a nested fork-join such that, the outer fork-join makes sure that inner process is finished. Inner fork-join_none will directly move to the wait statement after starting the #timeout delay. If the flag is set to the desired value, it comes out of wait since event is triggered. If the flag doesn't get the desired value, and timeout is reached, then timed_out is set and wait event is triggered to move to next statement.

Note the enclosing fork/join is needed to prevent the disable fork from killing other child threads that may have been spawned before calling this task.

  * Why add another bit “timed_out” instead of using join_any?

-> Because the reference to signal inside a fork/join_any or join_none is not legal.

You can find the original thread in Verification Academy Forum here: https://verificationacademy.com/forums/t/wait-for-signal-value-in-a-task-with-timeout/34702

----------------------------------------------------------------------------------------------------
Try here:

<a href="https://edaplayground.com/x/HjmP" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="70" height="70">
</a>

----------------------------------------------------------------------------------------------------
