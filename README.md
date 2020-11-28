# REPPP-Cloud-Deployment Overview

This is the Cloud deployment Part of **Real-Estate-Price-Prediction-Project**. 

To know about the Datacleaning and Model Building part please visit this **Repo:** https://github.com/mz-tahmeed/Real-Estate-Price-Prediction-Project

* This Model predicts home prices from a Kaggle dataset with 85% accuracy.

* Engineered features from the dataset added new features also remove unwanted features using business logic and dimensionality reduction.

* Remove outlier using Standard Deviation and Mean also plot the data to understand better using Matlotlib.

* Optimized Linear Regression, Decision Tree, and Lasso Regression using GridsearchCV to reach the best model.

This data science project series walks through step by step process of how to build a real estate price prediction website. First build a model using sklearn and linear regression using banglore home prices dataset from kaggle.com. Second step would be to write a python flask server that uses the saved model to serve http requests. Third component is the website built in html, css and javascript that allows user to enter home square ft area, bedrooms etc and it will call python flask server to retrieve the predicted price. During model building we will cover almost all data science concepts such as data load and cleaning, outlier detection and removal, feature engineering, dimensionality reduction, gridsearchcv for hyperparameter tunning, k fold cross validation etc.

**Technology and tools wise this project covers:**
1. Python
2. Numpy and Pandas for data cleaning
3. Matplotlib for data visualization
4. Sklearn for model building
5. Jupyter notebook, Visual Studio Code and Spyder as IDE
6. Python flask for http server
7. HTML/CSS/Javascript for UI

## AWS Deployment Process

### Deploy this app to cloud (AWS EC2)

1. Create EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic
2. Now connect to your instance using a command like this,
```
ssh -i "C:\Users\**\.ssh\Banglore.pem" ubuntu@ec2-3-133-88-210.us-east-2.compute.amazonaws.com
```
3. nginx setup
   (1) Install nginx on EC2 instance using these commands,
   ```
   sudo apt-get update
   sudo apt-get install nginx
   ```
   (2) Above will install nginx as well as run it. Check status of nginx using
   ```
   sudo service nginx status
   ```
   (3) Here are the commands to start/stop/restart nginx
   ```
   sudo service nginx start
   sudo service nginx stop
   sudo service nginx restart
   ```
   (4) Now when you load cloud url in browser you will see a message saying "welcome to nginx" This means your nginx is setup and running.
   
4. Now you need to copy all your code to EC2 instance. You can do this either using git or copy files using winscp. We will use winscp. You can download winscp from here: https://winscp.net/eng/download.php
5. Once you connect to EC2 instance from winscp (instruction in a youtube video), you can now copy all code files into /home/ubuntu/ folder. The full path of your root folder is now: **/home/ubuntu/BangloreHomePrices**
6.  After copying code on EC2 server now we can point nginx to load our property website by default. For below steps,
    1. Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,
    ```
    server {
	    listen 80;
            server_name bhp;
            root /home/ubuntu/REPPP/client;
            index app.html;
            location /api/ {
                 rewrite ^/api(.*) $1 break;
                 proxy_pass http://127.0.0.1:5000;
            }
    }
    ```
    2. Create symlink for this file in /etc/nginx/sites-enabled by running this command,
    ```
    sudo ln -v -s /etc/nginx/sites-available/bhp.conf
    ```
    3. Remove symlink for default file in /etc/nginx/sites-enabled directory,
    ```
    sudo unlink default
    ```
    4. Restart nginx,
    ```
    sudo service nginx restart
    ```
7. Now install python packages and start flask server
```
sudo apt-get install python3-pip
sudo pip3 install -r /home/ubuntu/REPPP/server/requirements.txt
python3 /home/ubuntu/REPPPs/client/server.py
```
Running last command above will prompt that server is running on port 5000.
8. Now just load your cloud url in browser and this will be fully functional website running in production cloud environment.

## Sources:

**Dataset is downloaded from here:** https://www.kaggle.com/amitabhajoy/bengaluru-house-price-data

**You can view on the details of this project here:** https://www.youtube.com/channel/UCh9nVJoWXmFb7sLApWGcLPQ
