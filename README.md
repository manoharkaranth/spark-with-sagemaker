# Spark_With_Sagemaker
### Problem statement:
   Build a Spark cluster on AWS's Elastic Map Reduce (EMR). Establish connection from it to a Sagemaker notebook instance over Livy. Conduct machine learning on data which reside in HDFS.

Solution is broadly divided into two parts.
1. Setting a connection up between EMR and Sagemaker.
2. Employing Machine learning on Spark cluster's data.

### 1. Setting a connection up between EMR to Sagemaker.
#### a. Set up EMR Spark and Livy.
Create a cluster in EMR with Spark and Livy. Configure as per your need while creating it. Once the cluster is in waiting state, go to hardware properties and note down the private IP of master node. 
#### b. Create appropriate security groups and ports.
Security groups for master and slave nodes will be automatically created once the cluster is ready. Construct a security group for Sagemaker notebook with no rules and collect the Group ID. Inside of already created security group,add an inboud Custom TCP Rule, on port 8998 and set the Security Group ID to the Group ID from the SageMaker notebook security group that we collected earlier. Add one more rule for SSH to your client server, on any unused port. 
#### c. Set up Jupyter notebook.
You also need to set up an IAM role with AmazonSageMakerFullAccess, plus access to any necessary Amazon S3 buckets. Select the same Subnet as the EMR cluster (you should have made note of this earlier, when you created your EMR cluster). Finally, set your security group to the group you created earlier for Notebook instances.

### 2. Employing Machine learning on Spark cluster's data.
This step is performed entirely in a notebook linked below.

