# UVM Sequence Library
----------------------------------------------------------------------------------------------------
Q. Given a very large number of sequences, how will you test all the sequences randomly without.
----------------------------------------------------------------------------------------------------

The above can be achieved by using a UVM Sequence Library. This would enable faster verification as well as cover all the test sequences randomly.

UVM Sequence Library encapsulates some or all the test sequences the user creates and creates a library of sequences. This library supports the methods to pick and randomize all available sequences in the library.

Class Declaration:
```systemverilog
class seq_lib extends uvm_sequence_library#(type_parameter);
  `uvm_object_utils(seq_lib)
  `uvm_sequence_library(seq_lib)

  function new(string name="seq_lib");
    super.new(name);
    add_typewide_sequence(seq_1::get_type());
    add_typewide_sequence(seq_2::get_type());
    .
    .
    add_typewide_sequence(seq_n::get_type());
  endfunction

endclass
```
The sequence library infrastructure is created using utility macro:
  
    `uvm_sequence_library()

The sequences are added to the library using method calls. 

    add_typewide_sequence(seq_name::get_type());

The library can be built / setup by calling the initialization method:

    init_sequence_library();

This initialization can be done either inside the sequence_library class itself or it can be called in the test class.

The library behaviour can be controlled using properties:
  * min_random_count  - Lower limit for randomization (default:10)
  * max_random_count  - Upper limit for randomization (default:10)
  * selection_mode    - Determines how sequences are selected
      - UVM_SEQ_LIB_RAND  - Random sequence selection
      - UVM_SEQ_LIB_RANDC - Random cyclic sequence selection
      - UVM_SEQ_LIB_ITEM  - Only generates the sequences and doesn't execute
      - UVM_SEQ_LIB_USER  - User - defined random - selection algorithm

A sequence library will generate random sequences between the min_random_count and max_random_count and then uses selection_mode to select and execute the sequences.

Find more information at https://www.youtube.com/watch?v=v53fx8dipjY. 

----------------------------------------------------------------------------------------------------
Try here:

<a href="https://edaplayground.com/x/ZhS6" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="70" height="70">
</a>

----------------------------------------------------------------------------------------------------
