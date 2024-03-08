# Music Album Microservice - Part 1
## Overview
This repository is part of a series of projects to develop a scalable distributed system running on AWS. In this particular project, I focus on building a simple music service that stores data about albums. The primary tasks include implementing a basic API and developing a client for performance testing.

## Project Structure

### Client Design

The architecture I implemented for this project's client component strongly emphasizes Java's synchronization tools to prevent potential race conditions and deadlock situations. I leveraged classes like Java's `CountDownLatch`, `ExecutorService`, and `AtomicInteger` to effectively manage thread interactions, minimizing contention and maximizing request throughput.

### Major Classes

#### AlbumClient:
The `AlbumClient` class simulates client-server interactions by initializing and loading a server with a predefined number of threads. It uses Java's synchronization mechanisms, such as `CountDownLatch`, to control concurrent execution and prevent race conditions. After successfully executing these interactions, the class calculates and prints various performance metrics, including throughput and latency, to evaluate the server's performance.

#### AlbumThreadRunnable:
The `AlbumThreadRunnable` class represents a thread that sends a specified number of GET and POST requests to a server, tracking the success, failure, and latency of these requests. It uses the [Swagger-generated API client](https://app.swaggerhub.com/apis/IGORTON/AlbumStore/1.0.0) to interact with the server. This class alternates between making POST and GET requests in groups to simulate client interactions and calculates various performance metrics. The measured metrics, including the number of successful and failed requests, request latency, and other statistics, are aggregated and then updated in the `AlbumClient` class to assess the server's performance.

#### LoadCalculations:
The `LoadCalculations` class provides various statistical calculations for a list of latencies. It includes the methods to calculate the mean, median, p99 (99th percentile), minimum, and maximum response times in milliseconds. The class sorts the latencies in ascending order during initialization, ensuring accurate statistical calculations. It provides a convenient way to analyze latency data collected during load testing or performance monitoring.

#### WriteToCsv:
The `WriteToCsv` class is responsible for managing the writing of load test results to a CSV file. It uses Apache POI, a popular library for working with Microsoft Office documents, to create and write data to an Excel file (XLSX format). It ensures that data is written in a structured manner and multiple threads can safely contribute their results to the same file without conflicts. After the server load test is completed, all results are written to an Excel file.

### Packages

As already mentioned, the client imports and utilizes existing Java packages. The Gson library converts Album and Image data into JSON format for JSON serialization. Next, the Apache POI library writes the server load test results to Excel sheets for data collection. Lastly, the Swagger auto-generated client library Ian provided us creates a simple API client that is used to make thread-safe GET and POST calls.

## Results

My results are interesting for both part 1 and 2. I struggled to collect results because my home network only has a max upload speed of 11 Mbps. When loading a server and sending thousands of small images, response times become relatively slow for POST requests. Because of this, I had to travel and commute to multiple locations to utilize the home networks of friends with a faster upload speed. However, this led to results not always being consistent. Almost all my tests and results are collected on the same house network with an upload speed of 350+ Mbps, but my Go Server tests with a thread group size of 30 are on a slower network with upload speeds of 150 Mbps. The decrease in network upload speed caused my throughput results for my Go server thread group size 30 to be less than expected. This is why my plots for my Goserver become closer horizontal when going from a thread group size of 20 to a size of 30.
Also, I misread the instructions and thought we had to test each thread group size three times. That is why each server has so many images and Excel sheets of different group sizes. For the main plots, I take the average of the three tests for my throughput and wall time plots. For individual thread group result screenshots, I included the best throughput results.

### Client Part 1 Results

Through testing, I found a difference between server load tests when comparing the Go and Java servers. Both servers saw increased throughput as more thread groups were called and ran. Overall, the Java server ran faster for all tests.

#### Java Server- Thread Group Size 10
![Java Server- Thread Group Size 10 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part1/ImageResults/JavaServer/Java_Server_10Threads_T3.png)

#### Java Server- Thread Group Size 20
![Java Server- Thread Group Size 20 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part1/ImageResults/JavaServer/Java_Server_20Threads_T2.png)

#### Java Server- Thread Group Size 30
![Java Server- Thread Group Size 30 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part1/ImageResults/JavaServer/Java_Server_30Threads_T2.png)

#### Go Server- Thread Group Size 10
![Go Server- Thread Group Size 10 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part1/ImageResults/GoServer/Go_Server_10Threads_T2.png)

#### Go Server- Thread Group Size 20
![Go Server- Thread Group Size 20 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part1/ImageResults/GoServer/Go_Server_20Threads_T1.png)

#### Go Server- Thread Group Size 30
![Go Server- Thread Group Size 30 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part1/ImageResults/GoServer/Go_Server_30Threads_T2.png)

### Plot of Load Results

Since I ran three trials for each group size, I took each server's average throughput and wall time. See below.
![Plot Load Results Part 1 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part1/ImageResults/Plot_Load_Results_Part1.png)

### Client Part 2 Results

Like Client Part 1, the Java server produced faster throughputs and wall times for me. See the results below. All my throughput results between thread group sizes on my servers for Part 1 and Part 2 were within 5% of each other.

#### Java Server- Thread Group Size 10
![Java Server- Thread Group Size 10 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/JavaServer/Java_Server_10Threads_T3.png)

#### Java Server- Thread Group Size 20
![Java Server- Thread Group Size 20 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/JavaServer/Java_Server_20Threads_T2.png)

#### Java Server- Thread Group Size 30
![Java Server- Thread Group Size 30 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/JavaServer/Java_Server_30Threads_T2.png)

#### Go Server- Thread Group Size 10
![Go Server- Thread Group Size 10 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/GoServer/Go_Server_10Threads_T3.png)

#### Go Server- Thread Group Size 20
![Go Server- Thread Group Size 20 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/GoServer/Go_Server_20Threads_T1.png)

#### Go Server- Thread Group Size 30
![Go Server- Thread Group Size 30 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/GoServer/Go_Server_30Threads_T1.png)

### Plot of Load Results

Since I ran three trials for each group size, I took each server's average throughput and wall time. See below.
![Plot Load Results Part 2 Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/Plot_Load_Results_Part2.png)

### Plots

#### Plot of Loads on Each Server

This plot shows the throughput vs. time for each server for the different-sized thread groups. As the throughput increases, so does the thread group size. Each point represents a thread group size. Since I accidentally recorded three runs for each thread group size, each point represents the average results for that thread group.

The plot below shows that results from Part 1 and Part 2 are very close for each server. However, this plot makes it clear that for my server load results, the Java server runs faster than the Go server.

![Plot Load Results Comparison Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/Plot_Load_Results_Comparison.png)

#### Plot ThroughPut Over Time
![Plot Throughput Over Time Image](https://github.com/tsanevp/Music-Album-Microservice-Part1/blob/main/Client/src/main/java/Part2/ImageResults/Plot_Throughput_Over_Time.png)
