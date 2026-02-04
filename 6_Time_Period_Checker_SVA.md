# Time Period Checker - SVA
----------------------------------------------------------------------------------------------------
Q. Write an Checker / System Verilog Assertion to check the time period of a given clock.
-------------------------------------------------------------------------------------------
The above can be achieved using concurrent assertion where the property evaluates at every posedge and checks the difference between the current posedge and the next posedge. 

We can create a parameterized property, that takes input as realtime.

```systemverilog
  property tp_check(realtime clk_period);
    realtime tp_prev;
    disable iff(!rst_n)
    (('1, tp_prev = $realtime) |-> ##1 (clk_period == $realtime - tp_prev));
  endproperty : tp_check
```

Assertion can be given as follows:
```systemverilog
  assert_time_period: assert property(@(posedge clk)tp_check(15)) 
                      $info("Assertion Pass @ %0t", $realtime);
```
Note that if using odd time periods, set the timescale precision accordingly.

----------------------------------------------------------------------------------------------------
Try here:

<a href="https://edaplayground.com/x/UaTz" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="70" height="70">
</a>

----------------------------------------------------------------------------------------------------
