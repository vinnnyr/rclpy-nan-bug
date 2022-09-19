# Minimal repro for rclpy bug
Depends on docker-compose being installed.

*Note*: might be `docker-compose` depending on if you have installed locally 

You can run the following examples without docker if wanted, just source the appropriate distro for the appropriate scenario

## Galactic
`TALKER_ROS_DISTRO="galactic" LISTENER_ROS_DISTRO="galactic" docker compose up`
This will work as expected with nan values.


## Humble (Publisher)
`TALKER_ROS_DISTRO="humble" LISTENER_ROS_DISTRO="galactic" docker compose up`
This will publisher (actually message construction) will *not* work.
We will get the following traceback:
```
rclpy-nan-bug-talker-1    | Traceback (most recent call last):
rclpy-nan-bug-talker-1    |   File "/code/main.py", line 42, in <module>
rclpy-nan-bug-talker-1    |     main()
rclpy-nan-bug-talker-1    |   File "/code/main.py", line 32, in main
rclpy-nan-bug-talker-1    |     rclpy.spin(minimal_publisher)
rclpy-nan-bug-talker-1    |   File "/opt/ros/humble/local/lib/python3.10/dist-packages/rclpy/__init__.py", line 222, in spin
rclpy-nan-bug-talker-1    |     executor.spin_once()
rclpy-nan-bug-talker-1    |   File "/opt/ros/humble/local/lib/python3.10/dist-packages/rclpy/executors.py", line 712, in spin_once
rclpy-nan-bug-talker-1    |     raise handler.exception()
rclpy-nan-bug-talker-1    |   File "/opt/ros/humble/local/lib/python3.10/dist-packages/rclpy/task.py", line 239, in __call__
rclpy-nan-bug-talker-1    |     self._handler.send(None)
rclpy-nan-bug-talker-1    |   File "/opt/ros/humble/local/lib/python3.10/dist-packages/rclpy/executors.py", line 418, in handler
rclpy-nan-bug-talker-1    |     await call_coroutine(entity, arg)
rclpy-nan-bug-talker-1    |   File "/opt/ros/humble/local/lib/python3.10/dist-packages/rclpy/executors.py", line 332, in _execute_timer
rclpy-nan-bug-talker-1    |     await await_or_execute(tmr.callback)
rclpy-nan-bug-talker-1    |   File "/opt/ros/humble/local/lib/python3.10/dist-packages/rclpy/executors.py", line 107, in await_or_execute
rclpy-nan-bug-talker-1    |     return callback(*args)
rclpy-nan-bug-talker-1    |   File "/code/main.py", line 21, in timer_callback
rclpy-nan-bug-talker-1    |     msg.data = float("nan")
rclpy-nan-bug-talker-1    |   File "/opt/ros/humble/local/lib/python3.10/dist-packages/std_msgs/msg/_float32.py", line 124, in data
rclpy-nan-bug-talker-1    |     assert value >= -3.402823e+38 and value <= 3.402823e+38, \
rclpy-nan-bug-talker-1    | AssertionError: The 'data' field must be a float in [-3.402823e+38, 3.402823e+38]
```

## Humble (Listener)
`TALKER_ROS_DISTRO="galactic" LISTENER_ROS_DISTRO="humble" docker compose up`
This will subscriber will *not* work.
We will get the following traceback:
```
rclpy-nan-bug-listener-1  | Unable to convert call argument to Python object (compile in debug mode for details)
```


