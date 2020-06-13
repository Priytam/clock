# AlarmClock
 AlarmClock with the features of
 - Alarm to wakeup 
 - Sync and Async Task
 - Scheduling Task
 - Recurring Task
 

# Basic Example and Usage

  1. [AlarmClock](#alarmclock)
  1. [Scheduling](#scheduling)
  1. [Recurring](#recurring)
  1. [Task](#task)

  
## AlarmClock

```java
public class AlarmClockExample {
    static {
        LogManager.getRootLogger().setLevel(Level.INFO);
        BasicConfigurator.configure();
    }

    public static void main(String[] args) throws InterruptedException {
        // Get and instance of clock
        AlarmClock clock = AlarmClock.getInstance("TestClock");

        //register to print 'ring ring' after 2 sec
        clock.register(2000, id -> System.out.println("ring ring"));

        //register to print 'ring ring1' after 3 sec
        clock.register(3000, id -> System.out.println("ring ring1"));

        //start this
        clock.start();

        //register to print 'ring ring2' after 2 sec
        clock.register(2000, id -> System.out.println("ring ring2"));

        //register to print 'ring ring3' after 2 sec
        clock.register(1000, id -> System.out.println("ring ring3"));

        //show all alarms set
        System.out.println(clock.getTaskList().showEntries());

        Thread.sleep( 6000);
        clock.shutdown();
    }
}

```
**Output**
```text
object = com.clock.AlarmClockHeartbeatTask@17550481, time remaining = 98
object = com.clock.example.AlarmClockExample$$Lambda$5/321142942@2c6a3f77, time remaining = 999
object = com.clock.example.AlarmClockExample$$Lambda$1/1032616650@2ed94a8b, time remaining = 1993
object = com.clock.example.AlarmClockExample$$Lambda$4/1935637221@180bc464, time remaining = 1999
object = com.clock.example.AlarmClockExample$$Lambda$2/649734728@5f2050f6, time remaining = 2995

ring ring3
ring ring
ring ring2
ring ring1

```

**[Back to top](#basic-example-and-usage)**

## Scheduling

```java
public class ScheduleExample {
    static {
        LogManager.getRootLogger().setLevel(Level.INFO);
        BasicConfigurator.configure();
    }

    public static void main(String[] args) {
        //create a schedule
        Schedule helloWorldSchedule = new Schedule("HelloWorldSchedule");

        //register a task to print 'Hello world'
        helloWorldSchedule.register(() -> System.out.println("Hello world"));

        //will print 'Hello world' every day 5:10
        helloWorldSchedule.addEntry(new DailyScheduleEntry(5, 10));

        //will print 'Hello world' every hour 1' o clock , 2' o clock, 3' clock
        helloWorldSchedule.addEntry(new HourlyScheduleEntry(0, 0));

        //will print 'Hello world' every monday at 5:10
        helloWorldSchedule.addEntry(new WeeklyScheduleEntry(WeeklyScheduleEntry.MONDAY, 5, 10));
        System.out.println(helloWorldSchedule.getStatus());

        //register another task to print hello Priytam and will run with all entries added above
        helloWorldSchedule.register(() -> System.out.println("Hello Priytam"));
        
        //shutdown
        helloWorldSchedule.shutDown();
    }
}
```

**output**
```text
	                HourlyEntry (0m:0s) 
				Next time will be at: Sun Jun 14 02:00:00 IST 2020
			DailyEntry (5h:10m:0s) 
				Next time will be at: Sun Jun 14 05:10:00 IST 2020
			WeeklyEntry (2d:5h:10m:0s) 
				Next time will be at: Mon Jun 15 05:10:00 IST 2020
```

**[Back to top](#basic-example-and-usage)**

## Recurring
```java
public class RecurringTaskExample {

    static {
        LogManager.getRootLogger().setLevel(Level.INFO);
        BasicConfigurator.configure();
    }

    public static void main(String[] args) throws InterruptedException {
        recurringSyncTaskExecution();//default
        recurringAsyncTaskExecution();
    }

    private static void recurringSyncTaskExecution() throws InterruptedException {
        // Create a recurring task with 'performTask' and 'onTaskFailure'
        RecurringTask recurringTask = new RecurringTask(1000) {
            @Override
            public boolean performTask(Object context) {
                System.out.println("Performing task");;
                return true;
            }

            @Override
            public void onTaskFailure(Object context) {
                System.out.println("Performing task failed");;
            }
        };

        //start recurring task
        recurringTask.start();
        Thread.sleep(4000);

        //stop  task
        recurringTask.stop();
    }
    
    private static void recurringAsyncTaskExecution() throws InterruptedException {
        RecurringTask recurringTask = new RecurringTask(1000) {
            @Override
            public boolean performTask(Object context) {
                System.out.println("Performing task");;
                return true;
            }

            @Override
            public void onTaskFailure(Object context) {
                System.out.println("Performing task failed");;
            }
        };
        // set async task execution
        recurringTask.setTaskExecution(new AsyncTaskExecution(4));

        //start recurring task
        recurringTask.start();
        Thread.sleep(4000);
        //stop  task
        recurringTask.stop();
    }
}
```

```text
Performing task
Performing task
Performing task
Performing task
4005 [main] INFO com.clock.AlarmClock  - shutdown()
4008 [main] INFO com.clock.AlarmClock  - start() - starting clock Alarmclock
Performing task
Performing task
Performing task
Performing task
8011 [main] INFO com.clock.AlarmClock  - shutdown()
```
**[Back to top](#basic-example-and-usage)**

## Task

```java
public class TaskExample {
    static {
        LogManager.getRootLogger().setLevel(Level.INFO);
        BasicConfigurator.configure();
    }
    public static void main(String[] args) {
        new Task() {
            @Override
            public boolean performTask(Object context) {
                System.out.println("Perform task");
                return true;
            }

            @Override
            public void onTaskFailure(Object context) {
                System.out.println("failed execution");
            }
        }.start();
    }
}

```
**[Back to top](#basic-example-and-usage)**
