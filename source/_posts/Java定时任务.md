---
title: Java 定时任务-笔记
tag: Java
categories:
  - [后端,Java,Spring]
---







<h1>Java 定时任务-笔记</h1>

[toc]



# 一、线程



```java

public class demo1 {
    public static void main(String[] args) throws InterruptedException {
        
        // 延迟 5 秒后,每隔1秒打印 hello world!
        final long interval  = 3000;
        Runnable runnable = new Runnable(){
            @Override
            public void run() {
                System.out.println("hello world!");
            }
        };

        Thread.sleep(5000);
        while(true){
            Thread thread1 = new Thread(runnable);
            Thread.sleep(1000);
            thread1.start();
        }
    }
}
```











# 二、Timer 类

   

```java
import java.util.Timer;
import java.util.TimerTask;



	public static void main(String[] args) {
        
        // 延迟5秒后，执行每隔1秒打印
        Timer timer = new Timer();
        
        // 侧重延迟时间稳定（错过之后，按新的节奏走）
        timer.schedule(new TimerTask() {
            int count = 0;
            @Override
            public void run() {
                ++count;
                System.out.println("hello world! "+count);
            }
        },5000,1000);
        
        // 侧重执行速度稳定（错过之后，按旧的节奏走）
        timer.scheduleAtFixedRate(new TimerTask() {
            int count = 0;

            @Override
            public void run() {
                ++count;
                System.out.println("hello world! "+count);
            }
        },5000,1000);
        
    }
```







# 三、Quartz 框架



Job实现类（需要无参构造，所以必须写成一个文件）：

```java
public class MyJob implements Job {
    @Override
    public void execute(JobExecutionContext jobExecutionContext) 
        throws JobExecutionException {
        
        System.out.println("hello world! ");
        
    }
}
```



利用时间间隔，任务调度：

```java
    public static void main(String[] args) throws SchedulerException {

        // 创建任务对象
        MyJob myJob = new MyJob();

        // 创建调度工厂
        StdSchedulerFactory factory = new StdSchedulerFactory();
        // 工厂生产调度器
        Scheduler scheduler = factory.getScheduler();

        // 绑定任务
        JobDetail jobDetail = JobBuilder.newJob(myJob.getClass())
                .withIdentity("job1", "group1")
                .build();

        // 建造触发器
        SimpleTrigger trigger = TriggerBuilder.newTrigger()
                .withIdentity("trigger", "triggerGroup")
                .startAt(new Date(System.currentTimeMillis()+5000))
                .withSchedule(
                        SimpleScheduleBuilder.simpleSchedule()
                                .withIntervalInSeconds(1)
                                .repeatForever()
                )
                .build();

        // 设置任务和触发器
        scheduler.scheduleJob(jobDetail,trigger);

        // 开始调度
        scheduler.start();

    }
```



利用`cron`表达式，任务调度：

```java
package com.cyw.task.test;

import org.quartz.*;
import org.quartz.impl.StdScheduler;
import org.quartz.impl.StdSchedulerFactory;

import java.util.Date;

public class demo3 {
    public static void main(String[] args) throws SchedulerException {

        // 创建任务对象
        MyJob myJob = new MyJob();

        // 创建调度工厂
        StdSchedulerFactory factory = new StdSchedulerFactory();
        // 工厂生产调度器
        Scheduler scheduler = factory.getScheduler();

        // 绑定任务
        JobDetail jobDetail = JobBuilder.newJob(myJob.getClass())
                .withIdentity("job1", "group1")
                .build();

        // 建造触发器
        CronTrigger cronTrigger = TriggerBuilder.newTrigger()
                .withIdentity("trigger2", "group1")
                .withSchedule(
           				// 一开始延迟5秒后，每隔1秒执行一次任务
                        CronScheduleBuilder.cronSchedule("5/1 * * * * ? *")
                )
                .build();

        // 设置任务和触发器
        scheduler.scheduleJob(jobDetail,cronTrigger);

        // 开始调度
        scheduler.start();

    }
}
```





# 四、SpringTask



在启动类上，`@EnableScheduling`开启任务调度：

```java
@EnableScheduling
@SpringBootApplication
public class TaskApplication {

	public static void main(String[] args) {
		SpringApplication.run(TaskApplication.class, args);
	}

}
```



编写任务类：

```java
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class MyTask {
    @Scheduled(cron = "40/5 * * * * ?")
    public void task(){
        System.out.println("hello world!");
    }
}
```

