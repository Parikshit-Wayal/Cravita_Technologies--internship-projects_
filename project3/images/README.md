#images

Project Architecture Diagram
Shows the overall data ingestion workflow from S3 → RDS → fallback to Glue using Docker.
![Architecture Diagram](./Dimage.png)


Dockerfile Screenshot
Displays the Dockerfile used to build the Python ingestion app image.
![Dockerfile Screenshot](./Docker-file.png)


S3 Bucket Created
Screenshot of the S3 bucket my-data-21 created in the Mumbai region.
![S3 Bucket Created](./bucket-created.png)


CSV File Uploaded to S3
Shows the data.csv file uploaded to the S3 bucket.
![CSV File in S3](./csvfile.png)


IAM User with Permissions
Displays the IAM user data-user created with programmatic access and policies attached.
![IAM User Created](./created-IAM.png)


Docker Run Command Execution
Screenshot of the Docker container execution using environment variables.
![Docker Run Command](./cont-cmd.png)


RDS MySQL Instance Created
Shows the created RDS MySQL instance with public access enabled.
![RDS Instance Created](./rds-created.png)


RDS Access via MySQL CLI
Screenshot of connecting to the RDS instance and verifying the data insertion.
![RDS MySQL CLI Access](./rds-mysql.png)


AWS Glue Table Created
Confirms that fallback data was registered in the Glue Data Catalog successfully.
![Glue Table Confirmation](./gluetable.png)
