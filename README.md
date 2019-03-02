#### Goolge Cloud Platform (GCP)

-__Preemptible Machines:__ We can rent them by very cheap price like 80% discount, if we agree that if anyone asked of the same resource for full price, we have to give it up.

These kinds of situations are good for big data and Hadoop techniques, which are fault-tolerant and when one resource is not available, they distribute it to other resources.


- __Dataproc:__ It is a Hadoop cluster that we can run Hadoop or Spark or other big data techniques.

An example is, we want to create a Dataproc cluster and we are going to use 10 standard VM's and 30 preemptible VM's. so we can still run it so fast using 30 more preemptible machines that are already 80% cheaper.

- All qwiklabs are available here:
```
https://googlecoursera.qwiklabs.com/
```

We need to make a VM on GCP, by simply getting to the dashboard and under __Computer Engine__ there is an option to make VM instances. 

It's straightforward to make one. 

After we made it we click on SSH button. and we will see the console opens.

Now we can ls to see there is nothing on our machine.

IF we want to see all information about the cpu we are using we run:
```
cat /proc/cpuinfo
```
Now, we want to install git on the VM. first we need to update apt-get:
```
sudo apt-get update
```
Now we can run git:
```
sudo apt-get install git
```
Don't forget we need to have access to sudo, and we need to be a super-user. In order to check it:
```
sudo su   //it returns the name of the user
```

and 
```
whoami    // if returns root it means we are the super user
```

When we have an analysis using a compute machine, we can save the results into cloud storage.

- Often the very first step in data life cycle is to get data into the cloud storage.

- How to store data into cloud?

We can install G Cloud SDK, and with it, a tool comes called __gsutil__.

On the local machien's folder via gsutil:
```
gsutil cp <file_name> gs://<bucket_name>/data/
```

(photo: copy-data-to-cloud-gsutil.jpg)

We also have rm to delete, and mv to move them.

Also mb to make a bucket, rb to delete the bucket.

```
Any language that can handle Htttp request, can request to GDP REST API and do what the gsutil can do.
```

