## Cluster Setup using Kops

### Task-1:  Launching EC2 instances and Connecting to EC2 Instances using SSH

* Manually Launch a `t2.micro` instance with OS version as `Ubuntu 22.04 LTS` in North Virginia (us-east-1) Region.
* Enable `SSH`, `HTTP`, `HTTPS` and `edit`.
* Type : `Custom TCP`     Source Type: `Anywhere`    Port Range : `8080-8090`
* Configure Storage: `10 GiB`
* Once Launched, Connect to the Instance using `MobaXterm` or `Putty` or `EC2 Instance Connect` with username "`ubuntu`".

### Task 2: Create an IAM role

* Create an IAM role named "kops-admin-role" with the "AdministratorAccess" policy attached.
* Now, associate the IAM role "kops-admin-role" with your EC2 instance named "kops" by following these steps:
  ** Navigate to the EC2 console and select the "kops" instance.
  ** Click on "Action" > "Security" > "Modify IAM role."
  ** Search for the "kops-admin-role" role, select it, and click "Update IAM role."

### Task 3: Setting up a Kubernetes Cluster
Set the hostname to "kops"
```
sudo hostnamectl set-hostname kops
```
Open a new bash shell
```
bash
```
Download the Kops script
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/kubernetes-essentials/kops-v1.25.sh
```
Execute the Kops script
```
. ./kops-v1.25.sh
```
Retrieve information about the existing clusters
```
kops get cluster
```
Get information about the Kubernetes nodes in the cluster
```
kubectl get nodes
```
To scale out the node, execute the below command. Replace the `nodes-us-east-2a.mehar-2024-03-19-16-03.k8s.local` with the auto-scaling group (master or worker) in your case and also replace `region` with your region. MAke sure the Max capacity of the Auto Scaling Group is above or equal to the number you are trying to scale to.
```
aws autoscaling update-auto-scaling-group --auto-scaling-group-name nodes-us-east-2a.mehar-2024-03-19-16-03.k8s.local --desired-capacity 3 --region us-east-2
```
Run the below command if you are not able to retrieve the data. The below command comes in handy if you have downscaled your cluster and have scaled it up again. 
```
kops export kubeconfig --admin
```

### Delete the Cluster
```
kops delete cluster --name <cluster-naame> --state s3://<cluster-naame> --yes
```
