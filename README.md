# Obstacle Avoidance using Move_Base package and reaching goals
## General Description of the Project
We have created 2 packages in-order to run our robot. First is to take the goal from /goals topic and to publish the sorted goals. Second one is for subscribing to the sorted goals and then publishing it to the /move_base/goal. This also publishes continuously about the status of the robot by using get_robo_stat topic.
Included Topics:


- /goals - All goal points are published into this topic and we are subscribing it to obtain the goals.


- /move_base/goal - It is a topic where goal points need to be published. We will be publishing goals on to move_base, so that it will create a plan and runs according to that.


- /amcl_pose - This topic helps us to find the exact position of the robot by localization. This applies Monte-Carlo localization technique for finding positions.


- /cmd_vel - It is a topic which helps us to give velocity as an input, in-order to move the robot around. At the beginning, it is important to localize robot accurately, for this robot need to be turned one full rotation. For this purpose, we make one rotation by publishing velocity to it.


- /get_robo_stat - It is a custom message to publish the different status of the robot. Status such as, Moving (While moving towards goal), Difficult-to-reach (When robot fails to reach goal), rotated (soon after first rotation for localization is done).


# Start gerty package to see robot moving
Launch the multi robots environment and then:

```bash
roslaunch gerty start.launch robot_id:=1/2/3/4
```
robot_id is the id for your robot
# Algorithm Description
Once goals have been sorted according the algorithm, this goals need to be published to the one of the move_base topic. Once it is published, work is not done!. Because as we know, once goal has been reached, we have to give other goal to move towards. Also, if robot fails to move towards goal, after few trials move_base will abort the mission. That's why it is always important to keep checking the movement of the robot. So we check the distance covered by robot at particular interval of time. If robot fails to move specified distance in the given time, then it will consider that goal to be difficult to reach and publishes that status. Then new goal will be given from the sorting package and starts moving towards next goal.

## When robot could not able to travel through the path for longer!!!!
In-order to avoid time waste while in searching of goals which are hard to reach, we will be checking the total distance covered by robot at particular period of time. When robot fails to reach specified distance within particular time, we consider the goal as difficult to reach and skip that goal. Because we do not want to waste time in search of nearly impossible goals.

## when move_base planner Aborts!!!!
When move_base does try it's all recovery possibilities and fail, it will abort the mission. When this happens, our robot look around it's Environment and finds available space. According to the space availability, it will move forward/backward/rotate and then publishes goal to move_base package again.

## Handling of missed goal Points
All missed goals will be saved in an array and once it finishes trying all goals to reach, these goals will be published to another topic. This topic will take missed goals and tries to reach these goals again.
We tried for this, but unfortunately we could not finish it on given time. So we consider this as our future work scope.

## Tuning move_base and amcl parameters for better output
move_base package can be made more efficient by changing few parameters from it. We have carried out few experiments to check it's effect on the robot. 

### We have used following parameter values in-order to obtain better results:

- yaw_goal_tolerance
- xy_goal_tolerance
