# Rule
Here we support some rules designed based on the UDrive used in our evaluation.
These rules serve as supplements to the existing logic of Apollo, without making extensive changes to its operational rules. 
Modifications to Apollo's operational rules can be tailored according to user needs.
## Detailed
```
rule "Be cautious of obstacles near intersections."
trigger
  always
condition
  intersection_distance_leq(10)
then
  long_buffer_dist(5)
  lat_buffer_dist(1)
  follow_dist(15)
  yield_dist(10)
  stop_dist(8)
```

  
