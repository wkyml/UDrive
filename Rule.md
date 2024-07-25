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
end
```
```
rule "Reduce speed near intersections."
trigger
  in_lane
condition
  intersection_distance_leq(50)
then
  expect_speed(20)
end
```
```
rule "Wait for vehicles within the intersection."
trigger
  in_lane
condition
  stop_sign_distance_leq(3)
  ego_speed_leq(0.5)
  vehicle_front_dist_leq(25)
then
  wait
end
```
```
rule "Wait for vehicles within the intersection."
trigger
  in_lane
condition
  stop_sign_distance_leq(3)
  ego_speed_leq(0.5)
  vehicle_left_dist_leq(25)
then
  wait
end
```
```
rule "Wait for vehicles within the intersection."
trigger
  in_lane
condition
  stop_sign_distance_leq(3)
  ego_speed_leq(0.5)
  vehicle_right_dist_leq(25)
then
  wait
end
```
```
rule "Avoid entering congested intersections."
trigger
  in_lane
condition
  traffic_light_distance_leq(5)
  is_jam(true)
  is_stop(false)
then
  stop
end
```
```
rule "Start the vehicle once the intersection is no longer congested."
trigger
  in_lane
condition
  traffic_light_distance_leq(5)
  is_jam(false)
  is_stop(true)
  traffic_light_color(green)
then
  launch
end
```
```
rule "Avoid driving slowly in the fast lane."
trigger
  in_lane
condition
  is_fast_lane(true)
  vehicle_front_dist_leq(50)
  vehicle_front_speed_leq(20)
then
  change_lane(right,1)
end
```
```
rule "Avoid unsafe lane changes."
trigger
  in_lane
condition
  vehicle_left_dist_leq(20)
then
  all_change_lane(left,false)
end
```
```
rule "Avoid unsafe lane changes."
trigger
  in_lane
condition
  vehicle_right_dist_leq(20)
then
  all_change_lane(right,false)
end
```
```
rule "Pass around static obstacles."
trigger
  in_lane
condition
  static_front_dist_leq(20)
then
  borrow_lane
end
```



  
