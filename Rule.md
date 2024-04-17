# Rule
Here we supports some rules which designed based on the UDriver. 
These rules serve as supplements to the existing logic of Apollo, without making extensive changes to its operational rules. 
Modifications to Apollo's operational rules can be tailored according to user needs.
## Predesigned rule
### Approaching intersection speed limit
```
rule "Speed management approaching intercations"
trigger
  approaching_intercation
condition
  find_intersection
then
  prep_dist(100)
  expect_speed(15)
until
  entering_intercation
end
```
### Speed management in different weather 
```
rule "Speed management in rainy"
trigger
  driving_in_rainy
condition
  is_raining
then
  decrease_ratio(50%)
until
  !is_raining
end
```
```
rule "Speed management in snowy"
trigger
  driving_in_rainy
condition
  is_raining
then
  decrease_ratio(50%)
  expansion_factor(50%)
until
  !is_snowing
end
```

## Online Actions
### Vehicle stop
```
rule "Stop"
trigger
  stopping
then
  stop
until
  launch
end
```
### Vehicle launch
```
rule "Vehicle launch"
trigger
  launching_vehicle
then
  launch
end
```
### Keep lane
```
rule "Keep lane"
trigger
  keeping_lane
condition
  in_lane
then
  lane_follow
end
```
### Cancle keep lane
```
rule "Cancle keep lane"
trigger
  cancle_keep_lane
condition
  lane_follow
then
  cancle_manoeuvre_control(lane_follow)
end
```
### Change lane
```
rule "Change lane"
trigger
  changing_lane
condition
  !lane_follow
then
  change_lane(r, 1)         #right, once
end
```
### Cancle change lane
```
rule "Cancle change lane"
trigger
  cancle_change_lane
condition
  change_lane
then
  cancle_manoeuvre_control(change_lane)
end
```
  
