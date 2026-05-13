1. start-dfs.sh
2. start-yarn.sh
3. hdfs dfs -put input.txt /Platypus
4. hadoop jar /usr/local/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.9.0.jar -input /Platypus/input.txt/ -output /Platypus/output/ -mapper 'python3 mapper.py' -reducer 'python3 reducer.py'
5. hdfs dfs -cat /Platypus/output/part-00000
