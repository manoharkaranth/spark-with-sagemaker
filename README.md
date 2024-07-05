## Spark with Sagemaker 
### Problem statement:
   Build a Spark cluster on AWS's Elastic Map Reduce (EMR). Establish connection from it to a Sagemaker notebook instance over Livy, a Spark REST server. Conduct machine learning on data which reside in HDFS.
<br/> <br/>
<br/><br/>
Solution is broadly divided into two parts.
1. Setting a connection up between EMR and Sagemaker.
2. Employing Machine learning on Spark cluster's data.

### 1. Setting a connection up between EMR and Sagemaker.
#### a. Set up EMR Spark and Livy.
*  Create a cluster in EMR with Spark and Livy. Configure as per your need while creating it. 
*  Don't forget to attach a pre-generated key-value pair to establish SSH connection with master node later from your local server.
*  Once the cluster is in waiting state, go to hardware properties and note down the private IP of master node.
*  Under Network, select Your VPC. You ought to make a note of your EC2 Subnet too.
#### b. Create appropriate security groups and ports.
* Security groups for master and slave nodes will be automatically created once the cluster is ready.
* Construct a security group for Sagemaker notebook with no rules and collect the Group ID. 
* Inside of already created security group for master node, add an inboud Custom TCP Rule, on port 8998 
* Set the Security Group ID to the Group ID from the SageMaker notebook security group that we collected earlier.
* Add one more rule for SSH to your client server, on any unused port and attach your local system's IP. 
#### c. Set up Jupyter notebook.
* Create a new notebook instance.
* You need to set up an IAM role with AmazonSageMakerFullAccess, plus access to any necessary Amazon S3 buckets. 
* Select the same Subnet as the EMR cluster (you should have made note of this earlier, when you created your EMR cluster). 
* Finally, set your security group to the group you created earlier for Notebook instances.
#### d. Configuring the notebook instance.
* Open the notebook instance with Jupyter notebook or Jupyter lab.
* Open a terminal and give following commands.
     - <code> cd .sparkmagic</code>
     - <code> wget https://raw.githubusercontent.com/jupyter-incubator/sparkmagic/master/sparkmagic/example_config.json</code>
     - <code> mv example_config.json config.json </code>
* Edit the config.json and replace every instance of _localhost_ with the Private IP of your EMR Master.
* We should test our connection to EMR over Livy. This can be done by running the following command:
     - <code> curl <EMR Master Private IP>:8998/sessions</code>
* Close the terminal and open a notebook with Sparkmagic (PySpark). Restart the kernel and do analytics.

### 2. Employing Machine learning on Spark cluster's data.
This step is performed entirely on a notebook linked here &#8594; [![nbviewer](https://user-images.githubusercontent.com/2791223/29387450-e5654c72-8294-11e7-95e4-090419520edb.png)](https://nbviewer.jupyter.org/github/manoharkaranth/spark-with-sagemaker/blob/master/sagemaker-spark.ipynb)</br>
<br/><br/>
### Final thoughts:
Salient element here is to utilise a sufficiently heavy machine learning compute instance for notebook. Otherwise, one may see a  _connection failed_ error. Also, enable autoscaling for EMR especially whilst working on a large data. Furthermore, Livy server's timeout is defaulted to 1 hour, to increase it, connect with the master node via SSH or Kerberos and configure it.
<br/><br/>
##### Reference: AWS documentations.

