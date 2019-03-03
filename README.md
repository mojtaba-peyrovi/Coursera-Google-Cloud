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

#### Lab 2:

In this lab we want to make a new compute engine, fetch some code from github repo, load some data, tranform them, and finally, save them in the cloud.

- The steps for making the VM and installing git are all the same as before, and now we want to clone some data form github:
```
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```
now we have training-data-analyst downloaded into the instance. Inside it, we have to go to this folder:
```
training-data-analyst/CPB100/lab2b
```
inside it there are some codes. we want to have some data from outside source ingested into the vm. we have to see the code __ingest.sh__.
```
less ingest.sh
```
This code simply creates a file called earthquake.csv and dowloads the latest version of the earthquake from its website and saves into the csv file.

To run the ingest code we say:
```
bash ingest.sh
```
Now, it downloads the data and puts the csv file inside the folder we are currently on (lab2b).

If we want to see what is inside it:
```
head earthquakes.csv
```
Now we want to install some python packages, to convert the data into a map form to be more readable. In order to do this, we could install any python package we needed, using sudo apot-get but there is a code already prepared to install what is needed we just need to say:
```
bash install_missing.sh
```
After it, we need to run the python code called transform.py using bash command and it puts all the data into the map and makes a png file from it and saves it.

We can now see the png is generated:
```
ls -l   //shows the contents as a list view
```
now we want to create a bucket and have the file saved there. We simply go to GCP dashboard and on the right sidebar, there is a button for storage. We create a new bucket and copy the name of it.
 
 Now we push the 3 earthquake files into the bucket(we made a new directory called earthquakes):
 ```
 gsutil cp earthquakes.* gs://mj-lab2/earthquakes/
```   
We have the files available now but they are not public yet, they are available only inside our bucket.

In order to share them publicly, there is a button next to them inside the bucket folder that we can click and make the files available to public and have shareable links.

- check useful resuorces here:
        
     [Computing Engine](https://cloud.google.com/compute/)
     
     [Storage](https://cloud.google.com/storage/)
     
     [Pricing](https://cloud.google.com/pricing/)
     
     [Cloud Launcher]( https://cloud.google.com/launcher/)
     
     [Pricing Philosophy](https://cloud.google.com/pricing/philosophy/)
     
 
 - __Interesting about recommendation system:__ There are two types of user rating a product:
    
          1- Explicit: User rating normally with giving starts, etc.
          2- Implicit: User showing interest by clicking or viewing a specific content. Or how long the user looked at some product before they scrolled down the page.   
     
   
__Found the Answer for my HQ recommendation:__

The problem was my matrix was sparse and I couldn't extract any recommendation from it.

- here is the story: I found how many trips each user has had to a specific hotel. It was so sparse because there were millions of users and alomst 700k hotels. When I ran my comparison between the users, because it was so sparse, it wouldn't give me any useful recommendation. 

The answer is here:

    - we need to write a ML engine, that fills the empty cells of the dataset based on what the majority of people have done, or the closest people to the user would have reated. after we filled all the matrix, then we will be able to predict the top similar users or hotels.  

Here is exactly what was mentioned in the course:

 - So you have those ratings and the thing to realize is how many of the products actually get rated? So let's say now you have a catalog of 10,000 houses, how many of those houses will individual user have rated? Maybe five, maybe 10, that's pretty much it. So it's from these ratings. So let's say every user has rated five houses. If we have a million users and we have 100,000 objects, we essentially have a matrix that's got one million rows and 100,000 columns and that matrix is extremely sparse because every user has rated only maybe five or 10 of those items and the remaining 999,995 objects are unrated but we need to come up with a rating for each of them. So that's basically what the ML model needs to do. It needs to predict what a user would rate a house that they haven't yet visited. In order to do that, you need to build ML model. But let's say we have that machine learning model that can basically say this house the user would rate it 2.1, this house they would rate it 3.4, this house they may rate it 1.7 et cetera, et cetera. Essentially, the recommending part of it is simply to go through that catalog of 100,000 houses, take for every user and then take the ratings, find the top five ratings and basically suggest those to them. That's essentially all the recommending is. The recommending is a very cheap operation, it's just find the top five, return the top five. The interesting thing is how you build the ML model. So, how would you build this model to predict the rating of a house for a user? Intuitively, we know how this ought to work. We will basically say that if you have two users that happen to have rated house A the same because they happened to have visited it and the first user rates it a four, second user also rates it a four, we could say intuitively that, let's go to the first user, find out all the other houses that they've rated and say well, these two guys now if they rated this house both a four maybe they will share the rating for everything else. So in other words, you could basically say who is this user like? So go through all of those users or those houses and say has this user rated the same house as somebody else and let's propagate that rating that way. Remember, the whole idea is to fill out the matrix of every user, let's say a million users, and every item, let's say 100,000 items. So, this 1 million by 100,000 matrix needs to get filled out and could get filled out starting by looking at whether two rows with each row corresponds to a user are not similar to each other. The other way that you could do this is to look at the houses themselves and say popular houses will tend to be liked, unpopular houses will tend not to be liked. So you might have a house that everyone who's looked at it agrees that it's a dump. They're basically rating it one or two out of five. You could, from that, infer that everyone is going to rate it at one or two it's a dump. The house is a bad house, everyone's going to rate it badly. So, in the absence of other information about a particular user and the kinds of things that they like, go with the majority vote and that's the second way they could do it, you do it. So essentially, we need to cluster users, we need to cluster items and combine the two of them to produce a rating. So, how often do we need to compute this predicted rating? Remember that computing the predicted rating is filling out that huge matrix. It takes a long time, it may take hours. Well, when do you need to recompute the rating? You will need to recompute the rating whenever the user comes in and rates a new house, or something else happened, maybe some other user rates a house that causes your rating to change. Now, this is not a very common occurrence, it's not something that you may have to do up to the minute. So, this is the kind of thing that many people do as a batch job. They may say well, I'm going to do this once a week. Based on all of the ratings that I have I'm going to create my new recommendation model and that is a recommendation model that I'm going to basically use for doing product recommendations. Because this is a batch job run occasionally, a good place to run it would be Hadoop. So, we will look at running this using PySpark, that's Park in Python on a Hadoop cluster which on GCP the Hadoop cluster is called Dataproc. 


- Another question, is where to save the recommendations for each user? The best answer is to save them on MySQL relational database platform of GCP called CouldSQL. 

 
__What is Cloud SQL?__ 

It is a mysql database hosted on GCP.   

Official definition:
`
Cloud SQL is a fully-managed database service that makes it easy to set up, maintain, manage, and administer your relational databases on Google Cloud Platform. You can use Cloud SQL with either MySQL or PostgreSQL.`  

The benefit is:

1- we can use it at a specifi time of the day and passivate it when we don't use it and simply don't pay for it.

2- Google manages the backups. It does take backup based on a schedule we give it.

3- You can connect to it from everywhere because it is on the cloud.

4- You can also get very fast connection from Google Compute engine, and Google App Engine.

#### Lab 3: Setup Rental Data in  Cloud SQL

First we activate Google Cloud Shell by clicking the button on the top right.

Google cloud shell is a virtual machine will almost all the tools we need already installed on it.

In order to see if we are authenticated on cloud shell we will run this:
```
gcloud auth list
```
In order to see the project id we are working on:
```
gcloud oonfig list project
```

Again we need to dowload the source codes:
```
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```

Again like before, we will navigate to lab3a.

Inside that, we navigate to the folder called cloudsql and copy everything inside it, into a new bucket, I called it mj-rental. like this:
```
gsutil cp * gs://mj-rental/sql
```
It makes a subfloder, called sql.

Now I will create a cloud SQL instance on the left sidebar, and click on SQL, and create instance.

When we want to make the Mysql instance, we are going to click on "show configure options" and "set connectivity" and "add network". here we are needed to add our computer IP.

We shouldn't forget the password we set when we created the instance. (it was rentals for this case)

Also, after the mysql instance being created, there would be a public IP that we need to have it saved somewhere.

When the instance is created, we can click on the import button on the top and it asks us to find the file we want to import from the buckets we have. we import the table creation sql file.

Then we will import both accommodation.csv and rating.csv files. The name of the database has to match with the name mentioned in the creation sql file.


Now we open the shell again, and type this:
```
mysql --host=35.238.97.21 --user=root --password  //the public IP of the mysql instance
```
Then it asks for password, we put the password we made (rentals) and then we are connected to the database.

Now we can say:
```
use recommendation_spark // the name of the database
```

Now if we say:
```
show tables;
```
It shows the 3 tables. 
