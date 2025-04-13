Word Count MapReduce Job 
This project runs a Hadoop MapReduce streaming job for word count using Python scripts (mapper.py, reducer.py) on a single-node Hadoop setup. The job reads input.txt from HDFS, processes it, and writes word counts to HDFS.
Prerequisites
Hadoop installed (e.g., version 2.7.2).

Python 3.x available.

HDFS running with NameNode and DataNode.

mapper.py, reducer.py, and input.txt in your working directory.

Commands
Start HDFS:
bash

$HADOOP_HOME/sbin/start-dfs.sh

$HADOOP_HOME: Hadoop installation directory.

Verify HDFS is Running:
bash

jps

Expected: Shows NameNode, DataNode, SecondaryNameNode.

Create HDFS Input Directory:
bash

hadoop fs -mkdir -p /user/$USER/input

$USER: Your username (whoami).

Save Input File to HDFS:
bash

hadoop fs -put $WORKDIR/input.txt /user/$USER/input/input.txt

$WORKDIR: Directory with mapper.py, reducer.py, input.txt.

Ensure Scripts Are Executable:
bash

chmod +x $WORKDIR/mapper.py $WORKDIR/reducer.py

Remove Old Output (if exists):
bash

hadoop fs -rm -r /user/$USER/output_new

Run MapReduce Job:
bash

hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar \
  -input /user/$USER/input/input.txt \
  -output /user/$USER/output_new \
  -mapper "$PYTHON_BIN mapper.py" \
  -reducer "$PYTHON_BIN reducer.py" \
  -file $WORKDIR/mapper.py \
  -file $WORKDIR/reducer.py

$PYTHON_BIN: Python executable path.

Verify Output:
bash

hadoop fs -cat /user/$USER/output_new/part-00000

Expected output:

hello	2
world	1

