# Apache Airflow Kick Start

## What is Airflow?

Apache Airflow is an open-source platform for developing, scheduling, and monitoring batch-oriented workflows. Airflow’s extensible Python framework enables you to build workflows connecting with virtually any technology. A web-based UI helps you visualize, manage, and debug your workflows. You can run Airflow in a variety of configurations — from a single process on your laptop to a distributed system capable of handling massive workloads.

## Sensor

A Sensor is a particular operator that waits for a condition to be true. If the condition is true, the task is marked successful , and the next task runs. If the condition is false, the sensor waits for another interval until it times out and fails.

- Sensor wait for an event/condition to be met to complete.
- A sensor times out after `timeout` seconds (7 days by default)
- A sensor checks an event/condition at every `poke_interval` (60 seconds by default)
- While a sensor waits, it continuously takes a work slot
- If you have many sensors or expect them to take time before complete, use the `reschedule` mode
- With the reschedule mode, while a sensor is waiting, its status will be `up_for_reschedule`
- Create a sensor with `@task.sensor`

<details>
  <summary>Why Sensors? </summary>
  The purpose of a Sensor is to wait for an event.

  That can be useful for many different use cases, such as:

- Processing files from an S3 bucket as they arrive while waiting for them.
- Running different tasks at different times but within the same DAG.
- Triggering a data pipeline when another one completes.
- Ensuring an API is available to make requests.
- Transforming data as soon as data are present in a SQL table.

</details>

<details>
  <summary>Best Practices</summary>

When using sensors, keep the following in mind to avoid potential performance issues:

- Always define a meaningful timeout parameter for your sensor. The default for this parameter is seven days, which is a long time for your sensor to be running. When you implement a sensor, consider your use case and how long you expect the sensor to wait and then define the sensor's timeout accurately.
- Whenever possible and especially for long-running sensors, use the reschedule mode so your sensor is not constantly occupying a worker slot. This helps avoid deadlocks in Airflow where sensors take all of the available worker slots.
- If your poke_interval is very short (less than about 5 minutes), use the poke mode. Using reschedule mode in this case can overload your scheduler.
- Define a meaningful poke_interval based on your use case. There is no need for a task to check a condition every 60 seconds (the default) if you know the total amount of wait time will be 30 minutes.

</details>
