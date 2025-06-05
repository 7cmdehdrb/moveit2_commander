# Service Manager Modules Documentation

## `FK_ServiceManager`

**Module:** `fk_service_manager.py`  
**Description:** Computes the forward kinematics (FK) using the `/compute_fk` service.

### Public Methods:
- **`run()`**: Computes the pose of the end effector for given joint states.

    Parameters:

    > joint_states (JointState): The joint states of the robot. This variable must be provided.

    > end_effector (str): The name of the end effector link for which to compute FK.

    Returns:
    > PoseStamped: The pose of the end effector in the base_link frame.

    Raises:
    > AssertionError: If joint_states is not an instance of JointState.

    > None: If the service call fails or the error code is not SUCCESS.


- **`fk_pose`**: Returns the last computed `PoseStamped` from the FK call (property).

---

## `IK_ServiceManager`

**Module:** `ik_service_manager.py`  
**Description:** Computes inverse kinematics (IK) for a given pose using the `/compute_ik` service.

### Public Methods:
- **`run()`**: Computes joint states that achieve the given `PoseStamped` for the end effector.

    Parameters:

    > pose_stamped (PoseStamped): The desired pose of the end effector in the base_link frame.
    
    > joint_states (JointState): The current joint states of the robot. This variable must be provided.
    
    > end_effector (str): The name of the end effector link for which to compute IK.
    
    > avoid_collisions (bool): Whether to avoid collisions during the IK computation. Defaults to False.

    Returns:
    
    > RobotState: The joint states that achieve the desired pose of the end effector.

    Raises:
    
    > AssertionError: If joint_states is not an instance of JointState or pose_stamped is not an instance of PoseStamped.
    
    > None: If the service call fails or the error code is not SUCCESS.

- **`ik_solution`**: Returns the last computed `RobotState` from the IK call (property).

---

## `GetPlanningScene_ServiceManager`

**Module:** `get_planning_scene_service_manager.py`  
**Description:** Retrieves the current planning scene using the `/get_planning_scene` service.

### Public Methods:
- **`run()`**: Fetches the current planning scene, including robot state and environment.

- **`scene`**: Returns the last computed `PlanningScene` from the GetPlanningScene call (property).

---

## `ApplyPlanningScene_ServiceManager`

**Module:** `apply_planning_scene_service_manager.py`  
**Description:** Manages the `/apply_planning_scene` service, applying a list of collision objects to a planning scene.

### Public Methods:
- **`run()`**: Applies a list of `CollisionObject` instances to a `PlanningScene` using the ApplyPlanningScene service.

    Parameters:

    > collision_objects (List[CollisionObject]): A list of CollisionObject instances to be added to the planning scene.
    
    > scene (PlanningScene): The PlanningScene instance to which the collision objects will be applied.

    Returns:
    
    > bool: True if the planning scene was successfully applied, False otherwise.

    Raises:
    
    > AssertionError: If scene is not an instance of PlanningScene or collision_objects is not a list of CollisionObject instances.

---

## `CartesianPath_ServiceManager`

**Module:** `catesian_path_service_manager.py`  
**Description:** Computes a Cartesian path through given waypoints using the `/compute_cartesian_path` service.

### Public Methods:
- **`run()`**: Computes a Cartesian path from a list of waypoints and the current joint states.

    Parameters:

    > header (Header): The header for the request, typically containing the timestamp and frame_id.
    
    > waypoints (List[Pose]): A list of Pose instances representing the waypoints for the Cartesian path.
    
    > joint_states (JointState): The current joint states of the robot. This variable must be provided.
    
    > end_effector (str): The name of the end effector link for which to compute the Cartesian path.

    Returns:
    
    > RobotTrajectory: The computed Cartesian path as a RobotTrajectory instance.

    Raises:
    
    > AssertionError: If header is not an instance of Header, waypoints is not a list of Pose instances,
        or joint_states is not an instance of JointState.
    
    > None: If the service call fails or the error code is not SUCCESS.
        
- **`trajectory`**: Returns the last computed `RobotTrajectory` from the GetCartesianPath call (property).

---

## `KinematicPath_ServiceManager`

**Module:** `kinematic_path_service_manager.py`  
**Description:** Computes motion plans using joint-space goals via the `/plan_kinematic_path` service.

### Public Methods:
- **`run()`**: Plans a joint-space trajectory using the provided joint goals and path constraints.

    Parameters:

    > goal_constraints (List[Constraints]): A list of Constraints objects that define the goal constraints for the kinematic path.
    
    > path_constraints (Constraints): Constraints that define the path constraints for the kinematic path. None is allowed.
    
    > joint_states (JointState): The current joint states of the robot. This variable must be provided.

    Planning Parameters:

    > num_planning_attempts (int): The number of planning attempts to make. Defaults to 100.
    
    > allowed_planning_time (float): The maximum time allowed for planning in seconds. Defaults to 1.0.
    
    > max_velocity_scaling_factor (float): The maximum velocity scaling factor for the planning. Defaults to 1.0.
    
    > max_acceleration_scaling_factor (float): The maximum acceleration scaling factor for the planning. Defaults to 1.0.

    Returns:

    > RobotTrajectory: The computed kinematic path as a RobotTrajectory instance.

    Raises:
    
    > AssertionError: If joint_states is not an instance of JointState or goal_constraints is not a list of Constraints instances.
    
    > None: If the service call fails or the error code is not SUCCESS.
        

- **`get_goal_constraint()`**: Generates a goal constraint from a given `JointState` and tolerance.

    Parameters:
    
    > goal_joint_states (JointState): The desired joint states for the goal constraint. This variable must be provided.
    
    > tolerance (float): The tolerance for the joint positions. Defaults to 0.05. If None, no tolerance is applied.

    Returns:
    
    > Constraints: A Constraints object containing the goal constraints for the kinematic path.
        
- **`trajectory`**: Returns the last computed `RobotTrajectory` from the GetGetMotionPlanCartesianPath call (property).
---

## `ExecuteTrajectory_ServiceManager`

**Module:** `execute_trajectory_service_manager.py`  
**Description:** Executes a planned trajectory using the `/execute_trajectory` action interface.

### Public Methods:
- **`run()`**: Executes a `RobotTrajectory` via the ExecuteTrajectory action client.

    Parameters:
    
    > trajectory (RobotTrajectory): The trajectory to be executed. This variable must be provided.

    Returns:
    
    > ExecuteTrajectory.Result: The result of the trajectory execution, which includes the status and any feedback.
        

- **`scale_trajectory()`**: Statically scales the duration and velocities of a trajectory by a given factor.

    Parameters:
    
    > trajectory (RobotTrajectory): The trajectory to be scaled.
    
    > scale_factor (float): The factor by which to scale the trajectory's duration and velocities.
    
    Returns:
    
    > RobotTrajectory: A new RobotTrajectory instance with scaled durations and velocities.
        