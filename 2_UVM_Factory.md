# UVM Factory
----------------------------------------------------------------------------------------------------
Q. What is UVM Factory and how does it work?
----------------------------------------------------------------------------------------------------
Consider you have a packet with the following constraint:
```systemverilog
class packet extends uvm_sequence_item;
    rand bit [7:0] data;

    constraint len{data > 0; data <255;}

endclass
```
Now assume you want to change the length of the data and want to modify this constraint, so you create an extended packet derived from the packet.
```systemverilog
class short_packet extends packet;
    ...

    constraint len{data < 50;}

endclass
```
We wish to use this packet in our test but everywhere we have created multiple instances of the original packet. So, how do I use my short_packet without editing all the instances of original packet?

Here comes the concept of Factory.

Factory is a centralized location which creates class instances for you. So you do not have to call constructor for the instances, rather factory creates the instances.

To use the factory, first we have to register the class type to the factory and then we use a factory method called "create", instead of the new constructor, to create class instances.

Factory registration can be done using utility macros:
```systemverilog
    `uvm_component_utils(component_name)
    `uvm_object_utils(object_name)
```
The instance can be created using the method 'create()':
```systemverilog
    component_name  = class_type::type_id::create("component_name",parent);
    object_name     = class_type::type_id::create("object_name");
```
### Factory Override
Now that we know how to register a class to factory and create the instances, we can look at how the new derived class can be replaced by the existing class at runtime.

To achieve the above, we can make use of Factory Overriding.
Factory Overriding can be defined as the ability to substitute an existing class with any of its inherited classes.

Factoy Override can be done by two ways:
    
* Type override by type/name
* Instance override by type/name
```systemverilog
// Override all the objects of a particular type
set_type_override_by_type ( uvm_object_wrapper original_type,
                            uvm_object_wrapper override_type,
                            bit replace=1);

set_type_override_by_name ( string original_type_name,
                            string override_type_name,
                            bit replace=1);

// Override a type within a particular instance
set_inst_override_by_type (uvm_object_wrapper original_type,
                           uvm_object_wrapper override_type,
                           string full_inst_path);

set_inst_override_by_name (string original_type_name,
                           string override_type_name,
                           string full_inst_path);
```
We will look at the most common / default type of overriding that is, Override Type by Type.
Consider the above mentioned classes, packet and short_packet.
We create a test class where we are making use of the build_phase of UVM to create the instance of packet using the create() method of factory.
```systemverilog
pkt = packet::type_id::create("pkt", this);
```
The above will create an instance of the class packet as pkt.
Now if we get the class details by calling the in-built print method, we can see that the type of the instance pkt is 'packet'.
```
KERNEL Output:
# -------------------------
# Name  Type    Size  Value
# -------------------------
# pkt   packet  -     @367 
# -------------------------
```
We can now use the Factory Override feature to change / substitute / override the type of pkt instance by short_packet.

```systemverilog
set_type_override_by_type(packet::get_type, short_packet::get_type);
```
After overriding, if we check the type of instance, we can see that it is now changed to short_packet.
```
KERNEL Output:
# -------------------------------
# Name  Type          Size  Value
# -------------------------------
# pkt   short_packet  -     @367 
# -------------------------------
```


You can checkout in detail about this topic here: https://www.chipverify.com/uvm/uvm-factory-override.


----------------------------------------------------------------------------------------------------
Try here:

<a href="https://edaplayground.com/x/YmS3" target="_blank">
  <img src="/utility/streamline-plump-color--gameboy.png" alt="Lets Play" width="70" height="70">
</a>

----------------------------------------------------------------------------------------------------
