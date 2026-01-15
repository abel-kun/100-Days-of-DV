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

----------------------------------------------------------------------------------------------------
Try here:

<a href="https://edaplayground.com/x/WxzY" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="70" height="70">
</a>
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><defs><filter id="SVGp0LL3caJ"><feGaussianBlur in="SourceGraphic" result="y" stdDeviation="1"/><feColorMatrix in="y" result="z" values="1 0 0 0 0 0 1 0 0 0 0 0 1 0 0 0 0 0 18 -7"/><feBlend in="SourceGraphic" in2="z"/></filter></defs><g filter="url(#SVGp0LL3caJ)"><circle cx="5" cy="12" r="4" fill="#02fa0b"><animate attributeName="cx" calcMode="spline" dur="2s" keySplines=".36,.62,.43,.99;.79,0,.58,.57" repeatCount="indefinite" values="5;8;5"/></circle><circle cx="19" cy="12" r="4" fill="#02fa0b"><animate attributeName="cx" calcMode="spline" dur="2s" keySplines=".36,.62,.43,.99;.79,0,.58,.57" repeatCount="indefinite" values="19;16;19"/></circle><animateTransform attributeName="transform" dur="0.75s" repeatCount="indefinite" type="rotate" values="0 12 12;360 12 12"/></g></svg>

----------------------------------------------------------------------------------------------------
