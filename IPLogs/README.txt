mapper.py
import sys 
import io 
input_stream=io.TextIOWrapper(sys.stdin.buffer) 
	for line in input_stream: 
		words=line.split( ) 
		for word in words: 
			print("%s\t%s"%(word,1)) 
-------------------------------------------------------------------------------------------------------------

reducer.py
import sys 
import io 
current_word=None 
current_count=0 
word=None 
for line in sys.stdin: 
	word,count=line.split('\t',1) 
	count=int(count) 
	if current_word==word: 
		current_count+=count 
	else: 
		if current_word: 
			print("%s\t%s"%(current_word,current_count)) 
	current_count=count 
	current_word=word 
if current_word==word: 
	print("%s\t%s"%(current_word,current_count))

-------------------------------------------------------------------------------------------------------------

COMMANDS:
start-dfs.sh
start-yarn.sh
hdfs dfs -put input.txt /Platypus
hadoop jar /usr/local/hadoop/share/hadoop/tools/lib/hadoop-streaming-3.3.6.jar -input /Platypus/input.txt/ -output /Platypus/output/ -mapper 'python3 mapper.py' -reducer 'python3 reducer.py'
hdfs dfs -cat /Platypus/output/part-00000

