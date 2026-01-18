# UVM_ERROR
----------------------------------------------------------------------------------------------------
Q. How would you save simulation time using UVM Reporting feature?
----------------------------------------------------------------------------------------------------

UVM Reporting Macros help in providing informative messages and debug information.
The following messages provide that capability:
* `uvm_info
* `uvm_warning
* `uvm_error
* `uvm_fatal 

Reporting macros can be controlled via setting verbosity and severity.

Amongst these, `uvm_error macros is the one that can be used to raise an error.

Unlike `uvm_fatal , for critical errors, which requires the simulation to terminate, `uvm_error is used for non-critical errors (e.g., configuration failures).

`uvm_error calls uvm_report_error with a verbosity of UVM_NONE.  The message can not be turned off using the reporter’s verbosity setting, but can be turned off by setting the action for the message.  ID is given as the message tag and MSG is given as the message text.  The file and line are also sent to the uvm_report_error call.
```systemverilog
    `uvm_error(ID,MSG)
```
`uvm_error has the action, Display + Increment Error Count, wherein it displays the error message and with every error, the error count is incremented.

Coming to the question, saving the simulation time, we can make use of this error count and tell the simulator to quit or terminate the simulation. The below function sets the maximum number of error counts that are allowed before terminating the simulation.
```
set_report_max_quit_count(count_number);
```

Assume we have decided that a maximum of three errors are allowed before it is too much.
Then we use the above function and set the count_number to 3, which would tell the simulator to exit after five errors appear.

```systemverilog
class comp extends uvm_component;
    ...

  task body();
    `uvm_info("Comp", "This is an information", UVM_NONE); #10;
    `uvm_error("Comp", "ERROR 1"); #10;
    `uvm_error("Comp", "ERROR 2"); #10;
    `uvm_error("Comp", "ERROR 3"); #10;
    `uvm_error("Comp", "ERROR 4"); #10;
    `uvm_error("Comp", "ERROR 5");
  endtask
    ...

endclass

module tb;
  
  comp c;
  initial begin
    c = comp::type_id::create("c", null);
    //set the max errors to 3 before simulation termination
    c.set_report_max_quit_count(3); 
    c.body();
  end
  
endmodule

```
The simulation terminates after 30 ns.

```
KERNEL Output:
# UVM_INFO testbench.sv(14) @ 0: c [Comp] This is an information
# UVM_ERROR testbench.sv(15) @ 10: c [Comp] ERROR 1
# UVM_ERROR testbench.sv(16) @ 20: c [Comp] ERROR 2
# UVM_ERROR testbench.sv(17) @ 30: c [Comp] ERROR 3
# UVM_INFO /usr/share/questa/questasim/verilog_src/uvm-1.2/src/base/uvm_report_server.svh(847) @ 30: reporter [UVM/REPORT/SERVER] 
# --- UVM Report Summary ---
# 
# Quit count reached!
# Quit count :     3 of     3
# ** Report counts by severity
# UVM_INFO :    4
# UVM_WARNING :    0
# UVM_ERROR :    3
# UVM_FATAL :    0
# ** Report counts by id
# [Comp]     4
# [Questa UVM]     2
# [UVM/RELNOTES]     1
# 
# ** Note: $finish    : /usr/share/questa/questasim/verilog_src/uvm-1.2/src/base/uvm_root.svh(135)
#    Time: 30 ns  Iteration: 0  Instance: /tb
```

----------------------------------------------------------------------------------------------------
Try here:

<a href="https://edaplayground.com/x/WF5y" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="70" height="70">
</a>

----------------------------------------------------------------------------------------------------
