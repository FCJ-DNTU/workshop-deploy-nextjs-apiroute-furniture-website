---
title: "Deploy Application to EC2 Instance"
date: 2025
weight: 6
chapter: false
pre: "<b>6. </b>"
---

#### 1. Install Git and NodeJS

- Install Git

```shell
$ sudo apt install -y git
$ git --version
```

- Install NodeJS and npm

```shell
$ curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
$ sudo apt install -y nodejs
$ node -v
$ npm -v
```

- Install PM2 to manage the application

```shell
$ sudo npm install -g pm2
$ pm2 -v
```

#### 2. Clone Repository

```shell
$ git clone https://github.com/nhungnguyen-9/e-commerce-furniture.git
```

- Use the **`ls`** command to check if the project has been cloned successfully
  ![clone-project](/images/6-deploy-the-application-to-ec2/6.1.png)

- Navigate to the **e-commerce-furniture** directory and install dependencies

```shell
$ cd e-commerce-furniture
$ npm install
```

#### 3. Migrate data from MongoDB to Amazon DocumentDB

- In this workshop, we will use the offline migration method. You can refer to other methods in this blog: [Migrate from
  MongoDB to Amazon
  DocumentDB](https://aws.amazon.com/blogs/database/migrate-from-mongodb-to-amazon-documentdb-using-the-offline-method/)
  ![migrate-offline](/images/6-deploy-the-application-to-ec2/offline-migration-approach.gif)

- Install **mongosh**

```shell
$ wget -qO - https://pgp.mongodb.com/server-6.0.asc | sudo tee /usr/share/keyrings/mongodb-server-key.asc
$ echo "deb [signed-by=/usr/share/keyrings/mongodb-server-key.asc] https://repo.mongodb.org/apt/ubuntu
focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
$ sudo apt update
$ sudo apt install -y mongodb-mongosh
$ mongosh --version
```

- Check connection to **DocumentDB**

- Go to the **Amazon DocumentDB Cluster** interface, then select the created cluster
  ![choose-cluster](/images/6-deploy-the-application-to-ec2/6.3.png)
- Download the **global-bundle.pem** file to EC2 and connect to DocumentDB (replace **insertYourPassword** with your
  actual password)
  ![connect](/images/6-deploy-the-application-to-ec2/6.4.png)
  ![connect2](/images/6-deploy-the-application-to-ec2/6.5.png)

- Run **show dbs** to check if there are any databases in DocumentDB
  ![check](/images/6-deploy-the-application-to-ec2/6.6.png)

- Next, proceed with data migration

- Install **MongoDB Database Tools**

```shell
$ wget https://fastdl.mongodb.org/tools/db/mongodb-database-tools-ubuntu2204-x86_64-100.9.0.tgz
$ tar -xvzf mongodb-database-tools-ubuntu2204-x86_64-100.9.0.tgz
$ sudo mv mongodb-database-tools-ubuntu2204-x86_64-100.9.0/bin/* /usr/local/bin/
$ sudo apt install -y mongodb-mongosh
$ mongodump --version
```

- **Dump** data from MongoDB. The data will be backed up into a folder named **backup**

```shell
$ mongodump
--uri="mongodb+srv://nguyennhung9846:3siHCKWL69z5DoS1@nhaxinh.02uuoha.mongodb.net/?retryWrites=true&w=majority&appName=nhaxinh"
--out=backup
$ ls
```

![mongodump](/images/6-deploy-the-application-to-ec2/6.7.png)

- **Restore** data to DocumentDB
- In the created cluster interface, copy the connection string
  ![mongorestore](/images/6-deploy-the-application-to-ec2/6.8.png)
- Run the following command to restore data to DocumentDB (replace **insertYourPassword** with your actual password)

```shell
$ mongorestore --uri="mongodb://user123:<insertYourPassword>
  @docdb-nextjs-workshop.cluster-c10k88ou8amc.ap-southeast-1.docdb.amazonaws.com:27017/?tls=true&tlsCAFile=global-bundle.pem&replicaSet=rs0&readPreference=secondaryPreferred&retryWrites=false"
  --dir=backup --drop
  $ ls
```

![mongorestore](/images/6-deploy-the-application-to-ec2/6.9.png)

- Verify data restoration and reconnect to DocumentDB
- In the **Cluster docdb-nextjs-workshop** interface, copy **Connect to this cluster with the mongo shell**, then
  paste it into EC2 to test the connection
  ![check-connect](/images/6-deploy-the-application-to-ec2/6.10.png)
  ![check-connect](/images/6-deploy-the-application-to-ec2/6.11.png)
- Run the following commands to check databases and collections

```shell
$ show dbs
$ use test
$ show collections
```

![check-db](/images/6-deploy-the-application-to-ec2/6.12.png)

#### 4. Add environment variables (.env)

- Create a **.env** file to store database connection details and application configurations

```shell
$ touch .env
$ nano .env
```

- Add environment variables

Download the **.env** file and add the required variables
**[.env](https://drive.google.com/file/d/1PH2-dZjuWKzp2cHs6MVGqb57LLpHhWoR/view?usp=sharing)**

![env](/images/6-deploy-the-application-to-ec2/6.13.png)

#### 5. Build and run the application

- Build the application

```
npm run build
```

- Run the application with PM2

```
pm2 start npm --name "nextjs-app" -- run start
pm2 save
pm2 startup
```

- Check if the application is running

```
pm2 list
```

![build](/images/6-deploy-the-application-to-ec2/6.14.png)

#### 6. Verify deployment

- Open a browser and visit

```
http://your-ec2-public-ip:3000
```

![deploy](/images/6-deploy-the-application-to-ec2/6.15.png)

#### 7. Verify storage in DocumentDB

- In the created cluster, copy the connection string under **Connect to this cluster with an application**
- In EC2, run the following command (replace **insertYourPassword** with your actual password)

  ```
    mongosh docdb-nextjs-workshop.cluster-c10k88ou8amc.ap-southeast-1.docdb.amazonaws.com:27017 --tls --tlsCAFile
    global-bundle.pem --retryWrites=false --username user123 --password <your-cluster-documentdb-password>
  ```

- Check the stored data

  ```
    use test
    show collections
    db.your-collection.find().pretty()
  ```

  ![check-database](/images/6-deploy-the-application-to-ec2/6.16.png)

{{< center>}}

### **Completed! 🚀**

{{< /center>}}
