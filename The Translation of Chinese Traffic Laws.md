# The Translation of Chinese Traffic Laws

### Law38: 
Motor vehicle signal lights and non-motor vehicle signal lights indicate:
* When the green light is on, vehicles are allowed to pass, but turning vehicles shall not hinder the passing of straight vehicles and pedestrians that are being released.
```
trigger 
    in(lane)
condition
    find_signal(traffic_light, <, 10)
    !find_obstacle(pedestrain, (left, front, right), (<, 10))
    !find_obstacle(vehicle, front, (<, 10))
    traffic_light_state(green)
    vehicle_state((<, 0.5), _, _, _)
then
    launch
end
```
* When the yellow light is on, vehicles that have crossed the stop line can continue to pass;
```
trigger
    in(lane)
condition
    find_signal(traffic_light, <, 10)
    traffic_light_state(yellow)
then
    stop
end
```
* When the red light is on, vehicles are prohibited from passing. When the red light is on, vehicles turning right can pass without hindering the passage of vehicles or pedestrians.
```
trigger
    always
then
    r_red_turn(true)
end

trigger
    in(lane)
condition
    find_signal(traffic_light, <, 10)
    vehicle_state(_, _, straight, !right)
    traffic_light_state(red)
then
    stop
end

```
### Law40:
Lane signal lights indicate:
* When the green arrow light is on, allow vehicles in the lane to pass in the direction indicated;
```
trigger 
    in(lane)
condition
    find_signal(traffic_light, <, 10)
    traffic_light_state(green)
    vehicle_state((<, 0.5), _, _, _)
then
    launch
end
```
* When the red cross-shaped light or arrow light is on, vehicles in the lane are prohibited from passing.
```
trigger
    in(lane)
condition
    find_signal(traffic_light, <, 10)
    traffic_light_state(red)
then
    stop
end
```
Noted:The current map has decomposed traffic lights, assigning a virtual traffic light for left turns, straight movements, and right turns. As a result, there is no distinction between arrow-shaped traffic lights and circular traffic lights.
### Law41:
The direction of the arrow of the direction indicator light indicates left, up, and right to turn left, go straight, and turn right respectively.

Has been implied in Law38 and Law40.
### Law42:
The flashing warning signal light is a yellow light that continues to flash, reminding vehicles and pedestrians to pay attention when passing through, and pass after confirming safety.
```
trigger
    always
condition
    find_signal(traffic_light, <, 10)
    traffic_light_state(yellow, flash)
then
    cruise_speed(20)
end
```
Noted: Not applicable at now stage, because the map doesn’t support this kind of traffic lights.
### Law43
When two red lights flash alternately or one red light is on a road and railway intersection, it means that vehicles and pedestrians are prohibited; when the red light is off, it means that vehicles and pedestrians are allowed to pass.
```
trigger
    always
condition
    find_signal(traffic_light, <, 10)
    distance_to(raliway, <, 10)
    traffic_light_state(red, flash)
then
    stop
end

trigger
    always
condition
    find_signal(traffic_light, <, 10)
    distance_to(raliway, <, 10)
    traffic_light_state(!red)
    vehicle_state((<, 0.5), _, _, _)
then
    launch
end
```
Noted: Not applicable at now stage, because the map doesn’t support this kind of traffic lights.
### Law44
Where there are two or more motorized lanes in the same direction on the road, the left side is the fast lane and the right side is the slow lane. Motor vehicles traveling in a fast lane shall drive at the speed specified in the fast lane, and those that have not reached the speed specified in the fast lane shall drive in a slow lane. Motorcycles should drive in the rightmost lane. If there are traffic signs indicating the driving speed, drive at the indicated driving speed. When a motor vehicle in a slow lane overtakes the preceding vehicle, it can borrow the fast lane to drive. Where there are two or more motor vehicle lanes in the same direction on the road, the motor vehicle that changes lanes shall not affect the normal driving of the motor vehicle in the relevant lane.
```
trigger
    in(lane)
condition
    find_obstacle(vehicle, right, (<, 30))
then
    allow_change_lane(right, false)
end

trigger
    in(lane)
condition
    find_obstacle(vehicle, left, (<, 30))
then
    allow_change_lane(left, false)
end

trigger
    in(lane)
condition
    is_special_lane(fast_lane)
    find_obstacle(vehicle, front, (<, 50), (<, 20))
    !find_obstacle(vehicle, right, (<, 30))
then
    allow_change_lane(right, true)
    change_lane(right, 1)
```
### Law45
Motor vehicles must not exceed the speed indicated by the speed limit signs and
markings on the road. On roads without speed limit signs and markings, motor vehicles shall not exceed the following maximum speeds.
* For roads without a road centerline, urban roads are 30 kilometers per hour, and highways are 40 kilometers per hour;
```
trigger
    in(lane)
condition
    is_special_lane(without_centerline)
then
    max_plan_speed(30)
end

trigger
    in(motorway)
condition
    is_special_lane(without_centerline)
then
    max_plan_speed(40)
end
```
* For roads with only one motor vehicle lane in the same direction, 50 kilometers per hour for urban roads and 70 kilometers per hour for highways.
```
trigger
    in(lane)
condition
    is_special_lane(one_lane_same_direction)
then
    max_plan_speed(50)
end

trigger
    in(motorway)
condition
    is_special_lane(one_lane_same_direction)
then
    max_plan_speed(70)
end
```
### Law46
When a motor vehicle encounters one of the following conditions, the maximum speed shall not exceed 30 kilometers per hour, and the maximum speed of tractors, battery vehicles, and wheeled special machinery vehicles shall not exceed 15 kilometers per hour:
* When entering or leaving a non-motorized vehicle lane, passing through a railway crossing, a sharp curve, a narrow road, or a narrow bridge;
```
trigger
    in(lane)
condition
    is_special_lane(non-motorized)
then
    max_plan_speed(30)
end

trigger
    in(lane)
condition
    distance_to(railway, <, 20)
then
    max_plan_speed(30)
end
```
* When turning around, turning, or descending steep slopes;
```
trigger
    in(intersection)
condtion
    !vehicle_state(_, _, straight, straight)
then
   max_plan_speed(30)
end 
```
* In case of fog, rain, snow, sand dust, hail, the visibility is within 50 meters;
```
trigger
    always
condition
    weather_is(foggy)
then
    max_plan_speed(30)
end 

trigger
    always
condition
    weather_is(raining)
then
    max_plan_speed(30)
end 

trigger
    always
condition
    weather_is(snowing)
then
    max_plan_speed(30)
end 

trigger
    always
condition
    weather_is(sandstorm)
then
    max_plan_speed(30)
end

trigger
    always
condition
    weather_is(hail)
then
    max_plan_speed(30)
end 
```
* when driving on icy and muddy roads;
```
trigger
    in(icy_road)
then
    max_plan_speed(30)
end 

trigger
    in(muddy_road)
then
    max_plan_speed(30)
end 
```
* When towing a malfunctioning motor vehicle.
Noted: Some elements, including visibility, icy roads, and muddy roads, are not currently supported by the simulator.
### Law47
When a motor vehicle is overtaking, it shall turn on the left turn signal in advance, change the use of far and low beam lights, or honk the horn. On a road with no center line of the road or with only one motor vehicle lane in the same direction, when the vehicle in front meets the vehicle behind and sends an overtaking signal, if conditions permit, the speed should be reduced and the road should be made to the right. After confirming that there is a sufficient safety distance, the following vehicle should pass from the left side of the vehicle in front, and after pulling the necessary safety distance from the overtaken vehicle, turn on the right turn signal and drive back to the original lane.
```
trigger
    in(lane)
condition
    vehicle_state(_, _, change, left)
then
    set_light(left_turn_light)
end

trigger
    in(lane)
condition
    vehicle_state(_, _, change, left)
    time_is(night)
then
    off_light(high_beam)
    set_light(low_beam)
    hock_horn
end
```
### Law48
On roads without central isolation facilities or without a central line, motor vehicles come in opposite directions. The following regulations should
be observed when driving:
* Slow down and keep to the right, and keep a necessary safe distance from other vehicles and pedestrians;
* On a road with obstacles, the side with obstacles shall go first; but when the side with obstacles has entered the road with obstacles and the side with obstacles has not, the side with obstacles shall go first;
* On a narrow slope, the uphill side goes first; but when the downhill side has reached halfway and the uphill side is not uphill, the downhill side goes first;
* On the narrow mountain road, the side that does not rely on the mountain shall go first;
* At night meeting vehicles should switch to low beam lights 150 meters away from the oncoming vehicle in the opposite direction, and should use low beam lights when meeting vehicles on narrow roads, narrow bridges and non-motorized vehicles.
Note: This rule is hard to describe, because it’s vague. The experiments can not be conducted due to the limit of maps.
### Law49
Motor vehicles shall not make U-turns at locations where there are signs or markings forbidden to turn or turn left, and at railway crossings, crosswalks, bridges, sharp bends, steep slopes, tunnels or road sections prone to danger. Motor vehicles can make U-turns where there is no prohibition of turning or left-turning signs or markings, but it shall not hinder the passage of other vehicles and pedestrians in normal driving.
```
trigger
    always
condition
    find_signal(unable_u-turn, <, 10)
then
    allow_u_turn(false)
end

trigger
    always
condition
    find_signal(unable_left-turn, <, 10)
then
    allow_u_turn(false)
end

trigger
    in(railway)
then
    allow_u_turn(false)
end

trigger
    in(crosswalk)
then
    allow_u_turn(false)
end

trigger
    in(bridge)
then
    allow_u_turn(false)
end

trigger
    in(tunnel)
then
    allow_u_turn(false)
end
```
### Law50
When a motor vehicleis reversing, the situation behind the vehicle shall be ascertained and the vehicle shall be reversed after confirming that it is safe. Do not reverse in railway crossings, intersections, one-way roads, bridges, sharp bends, steep slopes, or tunnels.
```
tigger
    always
condition
    vehcial_state(_, _, back, straight)
    find_obstacle(all, (back), (<, 20))
then
    stop
end

tigger
    always
condition
    vehcial_state((<, 0.5), _, back, straight)
    !find_obstacle(all, (back), (<, 20))
then
    launch
end
    
```
Others have been implied in Law49.
### Law51
Motor vehicles passing through intersections controlled by traffic lights shall pass in accordance with the following regulations:
* At an intersection with a guide lane, drive into the guide lane according to the required direction of travel;
* Those who are preparing to enter the roundabout let motor vehicles already in the intersection go ahead;
* When turning to the left, turn to the left of the center of the intersection. Turn on the turn signal when turning, and turn on the low beam when driving at night;
```
trigger
    in(intersection)
condition
    vehical_state(_, _, straight, left)
then
    set_light(left_turn_light)
end

trigger
    in(intersection)
condition
    vehical_state(_, _, straight, left)
    time_is(night)
then
    off_light(high_beam)
    set_light(low_beam)
end
```
* Pass in turn when encountering a release signal;
* When the stop signal is encountered, stop outside the stop line in turn. If there is no stop line, stop outside the intersection;
```
trigger
    always
condition
    distance_to(stop_signal, <, 20)
then
    wait_time(stop_signal, 10)
    crawl_time(stop_signal, 10)
end
```
* When turning right when there is a car in the same lane waiting for the release signal, stop and wait in turn;
```
trigger
    in(lane)
condition
    distance_to(intersection, <, 10)
    vehical_state((<, 0.5), _, straight, right)
    find_obstacle(vehicle, (front), same, (<, 10), (<, 10))
then
    wait
end
```
* At intersections with no direction indicator lights, turning motor vehicles let straight vehicles and pedestrians go first. Right-turning motor vehicles traveling in the opposite direction let left-turning vehicles go first.
### Law52
When a motor vehicle passes through an intersection that is not controlled by traffic lights or commanded by traffic police, in addition to complying with the provisions of Article 51 (2) and (3), it shall also comply with the following provisions:
* If there are traffic signs and markings, let the party with priority to go first;
* If there is no traffic sign or marking control, stop and look at the intersection before entering the intersection and let the traffic on the right road go first;
```
trigger
    always
then
    stop_no_sig(true)
end
```
* Turning motor vehicles let straight vehicles go first;
* A right-turning motor vehicle driving in the opposite direction will let the left-turning vehicle go first.
### Law53
* When a motor vehicle encounters a traffic jam at an intersection ahead, it shall stop and wait in turn outside the intersection and shall not enter the intersection.
```
trigger
    in(lane)
condition
    distance_to(traffic_light, <, 5)
    is_jam(true)
    !vehical_state((<, 0.5), _, _, _)
then
    stop
end

trigger
    in(lane)
condition
    distance_to(traffic_light, <, 5)
    is_jam(false)
    vehical_state((<, 0.5), _, _, _)
    traffic_light_state(green)
then
    launch
end
```
* When a motor vehicle encounters a motor vehicle in front of the vehicle parked in a queue or is driving slowly, it shall be queued in sequence, and shall not pass through or overtake from both sides of the vehicle in front and shall not park and wait in the area of crosswalks or mesh lines.
```
trigger
    always
condition
    distance_to(intersection, <, 50)
then
    long_buffer_dist(5)
    lat_buffer_dist(1)
    follow_dist(15)
    yield_dist(10)
    stop_dist(8)
    expect_speed(20)
end
```
* When a motor vehicle is at an intersection or road section with reduced lanes, if there is a motor vehicle in front of the vehicle parked in a queue or driving slowly, one vehicle in each lane shall alternately drive into the intersection or road section with reduced lanes.
### Law57
Motor vehicles shall use turn signals in accordance with the following provisions:
* When turning left, changing lanes to the left, preparing to overtake, leaving a parking place or turning around, the left turn signal shall be turned on in advance;
```
trigger
    always
condition
    vehical_state(_, _, straight, left)
then
    set_light(left_turn_light)
end

trigger
    always
condition
    vehical_state(_, _, change, left)
then
    set_light(left_turn_light)
end
```
* When turning right, changing lanes to the right, driving back to the original lane after overtaking, or stopping by the side of the road, the right turn signal should be turned on in advance.
```
trigger
    always
condition
    vehical_state(_, _, straight, right)
then
    set_light(right_turn_light)
end

trigger
    always
condition
    vehical_state(_, _, change, right)
then
    set_light(right_turn_light)
end
```
### Law58
When a motor vehicle is driving at night without streetlights, poor lighting, or low visibility conditions such as fog, rain, snow, sand, hail, etc., it shall turn on the headlights, position lights and rear position lights, but when the vehicle behind and the vehicle in front driving in the same direction are driving at close distances, the high beam shall not be used. When a motor vehicle is driving in a foggy day, the fog lights and hazard warning flashes should be turned on.
```
trigger
    always
condition
    weather_is(foggy)
then
    set_light([fog_light, warning_flash])
end 

trigger
    always
condition
    weather_is(raining)
then
    set_light([fog_light, warning_flash])
end 

trigger
    always
condition
    weather_is(snowing)
then
    set_light([fog_light, warning_flash])
end 

trigger
    always
condition
    weather_is(sandstorm)
then
    set_light([fog_light, warning_flash])
end

trigger
    always
condition
    weather_is(hail)
then
    set_light([fog_light, warning_flash])
end
```
### Law59
When a vehicle passes sharp bends, slopes, arch bridges, crosswalks or intersections without traffic lights at night, it shall alternately use far and near lights. When a motor vehicle is approaching sharp bends, the top of a ramp and other road sections that affect the safe sight distance, as well as overtaking or in an emergency, the vehicle should slow down and honk the horn.
```
trigger
    in(bridge)
condition
    time_is(night)
then
    off_light(high_beam)
    set_light(low_beam)
end

trigger
    in(crosswalk)
condition
    time_is(night)
then
    off_light(high_beam)
    set_light(low_beam)
end

trigger
    in(intersection)
condition
    time_is(night)
then
    off_light(high_beam)
    set_light(low_beam)
end

trigger
    in(lane)
condition
    vehcial_state(_, _, change, _)
then
    except_speed(20)
    honk_horn
end
```
### Law62
Driving a motor vehicle shall not have the following behaviours:
* Turn off the engine or slide in neutral when descending a steep slope;
* Honk horns in areas or road sections where honking is prohibited.
```
trigger
    always
condition
    honking_allowed(false)
then
    !honk_horn
end
```
### Law64
When a motor vehicle is passing a Manshui Road or Manshui Bridge, it shall stop
and check the water conditions, and after confirming safety, pass at low speed. Not suitable for ego.
```
trigger
    in(flooded_road)
then
    except_speed(20)
end

trigger
    in(flooded_bridge)
then
    except_speed(20)
end
```
