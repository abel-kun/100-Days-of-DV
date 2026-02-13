# UVM Resource Database - resource_db & config_db
----------------------------------------------------------------------------------------------------
Q. Explain UVM Resource and Configuration Database.
----------------------------------------------------------------------------------------------------

* *uvm_config_db#()* and *uvm_resource_db#()* may seem like two independent mechanisms for passing and storing values in a testbench. However, they are not separate databases at all. Both are simply APIs that provide different levels of abstraction for accessing the same underlying global database — the *uvm_resource_pool*.

* uvm_config_db#() is built on top of uvm_resource_db#(), and uvm_resource_db#() itself sits as a higher-level layer over the low-level uvm_resource_pool.

The uvm_resource_pool manages resources using two lookup tables:

* A **Name Table**, which is a string-indexed associative array that maps resource names to queues of resource handles.
* A **Type Table**, which is a type-handle-indexed associative array that organizes resources by type into queues of handles.

This design provides both name-based and type-based lookup, enabling flexible access to resources across the UVM testbench hierarchy.

**Resource Retrieval in UVM**

*Retrieval by Name*

* Uses the uvm_resource_db::read_by_name API.
* A string or regular expression is matched against the entries in the Name Table.
* Since uvm_resource_db allows pseudo-scopes, the string used in retrieval doesn’t have to correspond to an actual uvm_component path — it only needs to match the name pattern provided when the resource was set.
* Example: retrieving "ANY::*" would match all resources that were stored with "ANY" in their name string and return the recent one.

      uvm_resource_db#(int)::set("ANY::*", "ANY", 55);
      uvm_resource_db#(string)::set("ANY::*", "ANY", "100-Days-of-DV");

*Retrieval by Type*

* Uses the uvm_resource_db::read_by_type API.
* The request is matched against the Type Table, where each resource type has its own queue of handles.
* If multiple resources of the same type exist, the last resource written is the one returned (“last write wins”).

**Config Retrieval in UVM**

*Retrieval with **uvm_config_db::get***

* Uses the uvm_config_db::get API.
* The scope argument must correspond to a valid uvm_component path in the testbench hierarchy. Arbitrary strings or pseudo-scopes are not allowed.
* A field name is provided to identify the specific configuration setting being requested.
* Wildcards can be used in the scope string (for example, env.agt.*) to match multiple hierarchical instances.
* If multiple resources match, the last value written is the one returned, following the last-write-wins rule.

This mechanism makes uvm_config_db more structured and less error-prone than uvm_resource_db, but it also means it is less flexible since it is tied strictly to the component hierarchy.


  **Recommendation**: *For most configuration scenarios, it is better to use uvm_config_db because it enforces structure, reduces ambiguity, and makes the environment easier to maintain in team projects. However, there are cases where uvm_resource_db is the more appropriate choice. Its flexibility makes it well-suited for setting virtual interfaces, performing type-based lookups, and sharing non-object resources such as integers, strings, or enums across different parts of the testbench. In short, uvm_config_db should be the default option for structured component configuration, while uvm_resource_db should be reserved for situations that require global sharing or more advanced retrieval mechanisms.*


For detailed explaination and original post, please go to -> https://www.linkedin.com/pulse/choosing-between-uvm-resource-config-db-best-practices-hassan-khaled-g9j3f/

----------------------------------------------------------------------------------------------------
