
# WordCount-Using-MapReduce-Hadoop

This project implements a word count program using Hadoop MapReduce. The program reads a text file, tokenizes words, counts occurrences, and outputs the results in descending order of frequency.

# Approach and Implementation

Mapper Logic- The TokenizerMapper reads each line from the input text. It tokenizes the line into words and emits each word with a count of 1.

Reducer Logic-The IntSumReducer aggregates word counts received from the Mapper.It stores the word counts in a HashMap and sorts them in descending order before writing the output.

## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn clean package
```

### 3. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/dataset
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/dataset
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command:

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/dataset/input.txt /output
```

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ shared-folder/output/
    ```
## Challenges faced
1. Difficulty Understanding Hadoop Syntax: Initially faced challenges in understanding Hadoop’s Mapper and Reducer functions. Resolved this by studying official documentation and running small test programs.

2. Java Programming Complexity: Debugging Java exceptions and managing class dependencies was challenging. Used online resources and IDE debugging tools to resolve issues efficiently.


## Sample Input and Output
Input Format
A text file where each line contains a sentence or a set of words. Example:
```bash
Hello world
Hello Hadoop
Hadoop is powerful
Hadoop is used for big data
```

Expected Output
Each word should be counted, and the output should be:
```bash
Hadoop 3
Hello 2
is 2
used 1
for 1
big 1
data 1
powerful 1
world 1
```