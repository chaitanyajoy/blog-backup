---
title: "Avoid RAM's First Stroke: Background Monitoring with Pytest"
datePublished: Thu Sep 14 2023 20:02:26 GMT+0000 (Coordinated Universal Time)
cuid: clmjlkkng000009l5069h3t6h
slug: avoid-rams-first-stroke-background-monitoring-with-pytest
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1694721469708/10a8a6e7-4d6c-4cf6-b631-e8f7039ab5bc.png
tags: python, performance, apis, test-automation, pytest

---

One day, when I was sleeping, my WhensAPP decided to update itself to 2.3.4.23.0.678, and started swallowing my gigabytes until it couldn't eat anymore. When I woke up, I couldn't help but shake my head with confusion. Why had I just made that sacrifice? For all my data had been given, nothing seemed to have changed in the user interface.

Yet, I googled why this mysterious update happened, and discovered that it is meant only to improve one thing. It would seem surprisingly simple, yet incredibly significant.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694721373244/ec03a104-7ca0-4e19-b84f-b67f4e0645e3.png align="center")

## **The Performance!**

Software or code performance is not just about a single thing, there are many factors associated with it. Not to sound or act like an expert, but I have listed a few from my notes. Even, we will try to understand some of them.

### **1\. Hardware Resources:**

*(I'll talk about this in the end, need to share more on this. Read the other eleven)*

### **2\. Software Design:**

This is the blueprint that guides how your app behaves. It's like the architecture of a house. Well-designed, efficient algorithms and data structures can lead to faster execution

### **3\. Code Quality:**

The better the code, the smoother the app runs. It's like having a well-oiled machine. Code with bottlenecks, memory leaks, or inefficient loops can degrade performance.

### **4\. Concurrency:**

Apps can do multiple things at once. It's like juggling multiple balls without dropping any. Multithreading or multiprocessing kind of.

### **5\. Network Performance:**

How fast your app talks to the internet. It's like the speed of your internet connection. Latency, bandwidth, and packet loss can all affect the performance of web-based or distributed applications.

### **6\. Database Performance:**

Where all the data lives. It's like a library; the faster you can find a book, the better. Database queries, indexing, and overall database design can significantly impact application performance.

### **7\. Caching:**

It's like having a cheat sheet. The app remembers things to make them faster to find. Caching frequently accessed data or computations would improve performance by reducing the need to perform repeated work

### **8\. Security Measures:**

Keeping your app safe; It's like locking your front door. Security features, such as encryption and authentication, can introduce overhead. Striking a balance between security and performance is essential.

### **9\. Operating System and Environment**:

The stage where your app performs, Is like the theatre for a play. Different operating systems have varying levels of resource management and efficiency.

### **10\. External Dependencies:**

Sometimes your app relies on others. It's like [a relay race](https://img.freepik.com/premium-vector/sports-relay-passing-baton-men-athletes-race_158415-212.jpg?w=2000). you're only as fast as the slowest runner. Apps often rely on external services, APIs, or libraries.

### **11\. Load and Scalability:**

Handling lots of users. It's like a restaurant that can serve a small dinner party or a grand banquet. Load balancing, horizontal scaling, and vertical scaling strategies can all affect performance.

### **12\. Monitoring and Optimization:**

Keeping an eye on things and making improvements. It's like having a coach for your app. Regular monitoring and profiling of the application can help identify performance bottlenecks and areas for optimization. Tools like Datadog could help.

Now talking about the first one:

## **01\. Hardware Resources:**

It's the stuff inside your device that makes everything work. Think of it as the engine in your car. This includes the CPU, RAM, storage devices (HDD or SSD), and network infrastructure. Saturating these will disrupt your device, and sometimes can corrupt your software.

**Last week (1): My Wife's BirthDay**

I am a victim of this. I was editing my wife's birthday video and ran out of storage, the device hung and the project corrupted. A big rework and a huge lesson.

**Last week (2): My Test's BadDay**

Not only that but recently, our team encountered a situation where our tests, when executing within Docker containers, had a sudden spike in RAM that went through the roof. Panic mode. This raised concerns about the efficiency of our tests.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694705738413/b1dbe39f-69e4-4f29-b7f8-0788b4ef049e.gif align="center")

So, I was asked to profile the performance of our tests. I'd love to share my trials and learnings with you, bear with me for some time.

### **Solution 1: Using pytest-monitor**

I installed [pytest-monitor plugin](https://pypi.org/project/pytest-monitor/). Out of the box, without any configuration, when I ran the tests, it monitored automatically and wrote the metrics onto `.pymon` file.

The plugin stores the data in a local SQLite database, which is available by default in Python. I opened it after the run and queried `SELECT * FROM TEST_METRICS`

Boom! Here you go!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694718196614/073ad2db-e794-44b4-b90f-944dddbc4e50.png align="center")

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><strong>But, The Problem: </strong>What made me rethink is there's no way (at least I didn't find) to stream live resource consumption as the test progresses. Pytest-monitor dumps the metrics into <code>.pymon</code> file only after the test run is finished. <mark>So, no way to find which areas of code is causing the spike</mark></div>
</div>

### **Solution 2: Using the psutil library**

```python
import psutil

class PerformanceMonitor:
    def __init__(self):
        self.cpu_percent = 0.0
        self.memory_percent = 0.0

    def measure(self):
        self.cpu_percent = psutil.cpu_percent(interval=1) #CPU usage
        process = psutil.Process() #memory usage
        self.memory_percent = process.memory_percent()

    def report(self):
        print(f"CPU Usage: {self.cpu_percent}%")
        print(f"Memory Usage: {self.memory_percent}%")

    def reset(self):
        self.cpu_percent = 0.0
        self.memory_percent = 0.0

performance_monitor = PerformanceMonitor()

#usage in tests
def test_blog_post_is_too_long():
    performance_monitor.measure() #problem - u need 
    assert "yes, but interesting" is not None
    performance_monitor.report()
    performance_monitor.reset()
```

The idea is to employ `PerformanceMonitor` to measure performance before and after a test logic within a test function (`test_blog_post_is_too_long`), facilitating performance analysis during testing.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><strong>But, The Problem:</strong> If you want to log resources in ten places, you will have to call <code>measure()</code>, <code>report()</code>, every time and <code>reset()</code> at the end of every test. Not practical at all</div>
</div>

### **Solution 3: psutil with Multithreading & Fixtures**

The ideal solution.

```python
import pytest
import threading
import psutil
import time

@pytest.fixture(scope="session", autouse=True)
def resource_monitor():
    def monitor_resources():
        while True:
            cpu_percent = psutil.cpu_percent(interval=1)
            process = psutil.Process()
            memory_percent = process.memory_percent()
            print(f"CPU Usage: {cpu_percent}%")
            print(f"Memory Usage: {memory_percent}%")
            time.sleep(5)

    resource_monitor_thread = threading.Thread(target=monitor_resources)
    resource_monitor_thread.daemon = True
    resource_monitor_thread.start()
    yield
    # Optionally, you can add cleanup code here if needed

def test_example():
    # Your test logic here
    assert "yes, but interesting" is not None
```

In this neat setup, I've created a helper fixture for the tests. This doesn't need an invitation; it automatically jumps in whenever your tests are running (it has that benefit, being a fixture). Its job is to watch how busy your CPU and memory are and log the metrics for you every 5 seconds.

The actual test will be running in another thread while resource\_monitor thread keeps an eye on the hardware resources.

**Why this is ideal?**

* It provides structured metrics in consistent intervals. This helps any service to consume it and trend it in a dashboard
    
* No new code in tests, just a new fixture
    
* No additional plugin
    

## Conclusion:

So, next time your tests go rogue on your RAM, just remember, you've got a sidekick named `psutil` is ready to join you on your trials for efficiency.

*(Edit: One more experience I'd like to share as we talked about performance)*

## Functionality vs Performance:

In the software world, I've learned that functional code is often more critical than faster code.

In my previous company, there was this super-experienced backend guy named Kanny. So, one day, I hit him with a question, why is our app slow sometimes, and what are we doing about it?

He said, "Functionality first, performance next. We'll ship the features to get the client's job done, and after that, we can make them efficient".

It was as if he said, "You don't ride a bike without a bullock cart once". Though he sounded a little cryptic, later, [I found out why Kanny is right](https://www.thoughtleadersllc.com/2019/12/speed-over-perfection-why-being-first-to-market-is-crucial)

Tables turned in two months, staff engineers, and performance specialists were deployed and the issues were addressed very quickly to improve the performance.