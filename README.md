# Reflection #11: Pig

### List the 3 Pig Queries you wrote. You can also store them in files named Part1.pig, Part2.pig, and Part3.pig

__Loading data__
```
census = LOAD './dataset/median_income_by_zipcode_census_2000_cleaned.csv' USING PigStorage(',') AS (id:chararray, id2:chararray, zip5:chararray, zip3:int, income:int);

dump census;
```

__Part1.pig__
```
field_2 = FOREACH census GENERATE zip5, income;

query_1 = FILTER field_2 BY income < 50000;
dump query_1;

STORE query_1 INTO 'hdfs://localhost:9000/output1';

hdfs dfs -cat hdfs://localhost:9000/output1/part-m-00000 > ./output/Pig_Query_1.out
```

__Part2.pig__
```
income = FOREACH census GENERATE income;
dump income;

grouped_income = GROUP income all;
dump grouped_income;

avg_income = FOREACH grouped_income generate AVG(income) AS avg;
dump avg_income;

field_3 = FOREACH census GENERATE id, zip5, income;
dump field_3;

filtered_income = FILTER field_3 BY income > avg_income.avg;
dump filtered_income;

query_2 = order filtered_income by income;
dump query_2;

STORE query_2 INTO 'hdfs://localhost:9000/output2';

hdfs dfs -cat hdfs://localhost:9000/output2/part-r-00000 > ./output/Pig_Query_2.out
```

__Part3.pig (Filter top 5 percent of income__
```
census_all = GROUP census All;
census_count = foreach census_all Generate COUNT(census.id) as count;
count_top5 = FOREACH census_count GENERATE count * 0.05 as (top5:int);
sorted = order field_3 by income DESC;
query_3 = LIMIT sorted count_top5.top5;
dump query_3;

STORE query_3 INTO 'hdfs://localhost:9000/output3';

hdfs dfs -cat hdfs://localhost:9000/output3/part-r-00000 > ./output/Pig_Query_3.out
```

### What technical errors did you experience? Please list them explicitly and identify how you corrected them.

There was no technical issues, as setting up with Docker is smooth like whipped cream on my frappuccino.

### What conceptual difficulties did you experience?

The description and demonstration on the videos were amazing. There was not much of difficulties writing Pig. Also, I loved how oreilly book had amazing guidance on Pig.

### How much time did you spend on each part of the assignment?

About 3 hours

### Track your time according to the following items: Gitlab & Git, Docker setup/usage, actual reflection work, etc.

Gitlab & Git: 5 min
Docker setup/usage: 10 min
Actual reflection work: 3 hours

### What was the hardest part of this assignment?

The hardest part of the assignment was to find out how to calculate the average of an entire income column. With the help of oreilly book, and stackoverflow, I found out that I had to use group by all to calculate the average of overall.

### What was the easiest part of this assignment?

There was no really easiest part of the assignment. Pig was a new concept. Similar to SQL or others, but still new. I had to get good understanding of the Pig before I began my assignment.

### What advice would you give someone doing this assignment in the future?

Read the documents, and book really well before starting. Video do give a great job of basic guidance. However, books give more in depth of every details on Pig.

### What did you actually learn from doing this assignment?

I learned how to run MapReduce jobs using Pig. It was similar to previous mapreduce, SQL, and Hive. It felt like a mixture of everything I've learned so far. I understood why Pig was built, as it was super easy to run mapreduce without much coding involved.

### Why does what I learned matter both academically and practically?

As a enginer for executing data flows in parallel on Apache Hadoop, Pig is being used widely in both small or huge companies. The concept of MapReduce was great, but it was difficult to write code, and maintain with Java. Pig and Pig Latin made it easier to do so. In academic, I learned how Pig works, and how to run basic code with Pig. Now I have a basic understanding of the code, it shouldn't be so much difficult using it in the industry. Of course, I believe the whole structure is more complex. However, knowing the basic well is always the best before jumping into a dungeon of projects.