# Autoscale

The doAzureParallel package lets you autoscale your cluster in several ways, letting you save both time and money by automatically adjusting the number of nodes in your cluster to fit your job's demands.

This package pre-defines a few autoscale options (or *autoscale formulas*) that you can choose from and use in your JSON configuration file.

The options are:
 - "QUEUE"
 - "WORKDAY"
 - "WEEKEND"
 - "MAX_CPU"

*See more [below]() to learn how each of these settings work.*

When configuring your autoscale formula, you also need to set the mininum number of nodes and the maximum number of nodes. Each autoscale formula will use these as parameters to set it's upper and lower bound limits for pool size. 

By default, doAzureParallel uses autoscale and uses the QUEUE autoscale formula. This can be easily configured:

```javascript
{
  ...  
  "poolSize": {
    "minNodes": 1,
    "maxNodes": 20,
    "autoscaleFormula": "QUEUE",
  }
  ...
}
```

## Autoscale Formulas:

For four autoscale settings are can be selected for different scenarios:

### QUEUE
This is a task-based adjustment - the formula will scale up and down the pool size based on the amount of work in the queue

### WORKDAY/WEEKEND
These are time-based adjustments - the formula  will adjust your pool size based on the day/time of the week. 

For WORKDAY, we will check the current time, and if it's a weekday during working hours (8am - 6pm), the pool size will increase to the maximum size. Otherwise it will default to the minimum size.

FOR WEEKEND, we will check the current time and see if its the weekend.

### MAX_CPU


## When to use Autoscale

Autoscaling can be used in various scenarios when using the doAzureParallel package. 

### Time-based scaling

For time-based autoscaling adjustments, you would want to autoscale your pool in anticipation of incoming work. If you know that you want your cluster ready during the workday, you can select the WORKDAY formula and expect your clster to be ready when you get in for work, or expect your cluster to automatically shut down after work hours.

### Task-based scaling

In contrast, task-based autoscaling adjustments are ideal for when you don't have a pre-defined schedule for running work, and simply want your cluster to scale up or scale down according to your task queue. 

A good example for this is when you want to execute long-running jobs: you can kick off a long-running foreach loops at the end of the day without worrying about having to shut down your cluster when the work is done. With Task-based scaling (QUEUE), the cluster will automatically decrease in size until the minNode property is met. This way you don't have to worry about monitoring your job and manually shutting down your cluster.

To take advantage of this, you will also need to understand how to retreive the results of your foreach loop from storage. See [here](./23-persistent-storage.md) to learn more about it.

## Static Clusters

If you do not want your cluster to autoscale, you can simply set the property minNode equal to maxNodes. For example, if you wanted a static cluster of 10 nodes, you can set *"minNodes" : 10* and *"maxNodes" : 10*

---

doAzureParallel's autoscale comes from Azure Batch's autoscaling capabilities. To learn more about it, you can visit the [Azure Batch auto-scaling documentation](https://docs.microsoft.com/en-us/azure/batch/batch-automatic-scaling).

