# Django-ElasticBeanstalk-Boilerplate
Boilerplate django project for AWS Elastic Beanstalk.

**Follow the steps to use this repot**

1. Clone this repo

	`git clone https://github.com/Alexmhack/Django-ElasticBeanstalk-Boilerplate.git`
	`cd Django-ElasticBeanstalk-Boilerplate`

2.  Create and activate virtualenv by running these commands, you can even use [pipenv](https://github.com/pypa/pipenv) but I prefer *virtualenv*

	`pip install virtualenv`
	`python3 -m virtualenv env`
	`cd env/Scripts`
	`activate`

3. Now install the python packages in virutalenv from *requirements.txt* file
	
	`pip install -r requirements.txt`

4. Install [AWS EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html)

5. Create Elastic Beanstalk application
	
	`eb init`
	Choose your region
	Choose python version 3.6
	Setup ssh for your instance, choose a keypair or create one

6. Create EB Environment by running
	
	`eb create TESTenv --database.engine postgres` # <- TESTenv is the name of environment
	postgres is the database engine for your database, enter the username and password of your choice when prompted

	**This process will take some time**

7. Change config for your elastic beanstalk environment

	`eb config`

	Change the **WSGIPath** from `application.py` to our projects path, `starter/wsgi.py`
	Save the file and close it, the environment update will start in *cmd*

8. Add env url to `ALLOWED_HOSTS`

	`eb status`
	Copy the `CNAME: <YOUR CNAME>` and paste it in `ALLOWED_HOSTS` list in **starter/settings.py** file.

9. Deploy

	`eb deploy`

10. Open the instance url in web browser

	`eb open`


11. You should see the standard Django2 start project welcome page.

12. Now get the RDS database host endpoint and port from **AWS EB console > Configuration > Database (at the bottom of webpage)**. Copy the endpoint and paste it in the `DATABASES` setting in **starter/settings.py** file.

## Django, Elastic Beanstalk & VPC
Follow these steps to create a Elastic Beanstalk environment inside a VPC and then run a Django backend instance.

**EB CLI [Create Reference](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-create.html?shortFooter=true) has all the options for our needs.**

*I assume you have downloaded awsebcli using pip, if not run `pip install --upgrade awsebcli `*

**Create a VPC in the AWS, here is [official guide](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/vpc-rds.html). Follow this guide before running the below commands**

1. Run the **eb** command
	`eb create deploy -db.engine postgres -db.user xxxxxxxxx -db.pass xxxxxxxxx --vpc --vpc.dbsubnets xxxxxxxxx,xxxxxxxxx,xxxxxxxxx,xxxxxxxxx -k KEY_PAIR_NAME`

	`-db.engine` -> the database engine, here postgres
	`-db.user` -> username of the database instance
	`-db.pass` -> password of the db instance
	`--vpc` -> launches environment in the Virtual Private Cloud (asks for details of vpc)
	`--vpc.dbsubnets xxxxxxxxx,xxxxxxxxx,xxxxxxxxx,xxxxxxxxx` -> the subnets of the db subnet groups
	`-k KEY_PAIR_NAME` -> If you want to connect to your instances using ssh use this option with your KEY_PAIR_NAME

2. Next follow the same commands stated in the above section to config and deploy your environment.
