# Constraint for generating Random Unique Numbers
----------------------------------------------------------------------------------------------------
Q. Write a constraint that can generate Unique Random numbers without using the SV keyword '_unique_'.
----------------------------------------------------------------------------------------------------

The above can be achived by using nested foreach loops that will iterate the indices against each index and check that the values are different.
Given below is the constraint:

```systemverilog
    constraint uniq_vals{
        foreach(data[i]){
            foreach(data[j]){
            if(i!=j) data[i] != data[j];
            }
        }
    }
```
=== Try Here ===

<a href="https://edaplayground.com/x/WxzY" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="50" height="50" x="20">
</a>

-------------------------------------------------------------------------------------------
