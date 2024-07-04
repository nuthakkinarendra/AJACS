import json
import boto3
import pymysql
import os

# Environment variables
rds_host = os.environ['RDS_HOST']
db_username = os.environ['DB_USERNAME']
db_password = os.environ['DB_PASSWORD']
db_name = os.environ['DB_NAME']
table_name = os.environ['TABLE_NAME']

# Initialize S3 client
s3 = boto3.client('s3')

def lambda_handler(event, context):
    for record in event['Records']:
        # Process each record from S3 event
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        
        # Download file from S3
        local_file_path = '/tmp/data.csv'
        s3.download_file(bucket, key, local_file_path)
        
        # Connect to RDS
        conn = pymysql.connect(host=rds_host,
                               user=db_username,
                               password=db_password,
                               database=db_name)
        cursor = conn.cursor()
        
        # Example: Insert data into RDS
        with open(local_file_path, 'r') as file:
            csv_data = csv.reader(file)
            next(csv_data)  # Skip header row
            for row in csv_data:
                cursor.execute(f"INSERT INTO {table_name} (column1, column2, column3) VALUES (%s, %s, %s)", row)
        
        # Commit changes and close connection
        conn.commit()
        cursor.close()
        conn.close()
        
        # Log successful processing
        print(f"Successfully processed file {key} from bucket {bucket}")
    
    return {
        'statusCode': 200,
        'body': json.dumps('Data loaded successfully')
    }








IAM Policy in S3 Bucket



    {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::533267223585:role/LambdaS3AccessRole"
            },
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::rds-task",
                "arn:aws:s3:::rds-task/*"
            ]
        },
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::533267223585:role/LambdaS3AccessRole"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::rds-task/*"
        },
        {
            "Sid": "S3PolicyStmt-DO-NOT-MODIFY-1719492067280",
            "Effect": "Allow",
            "Principal": {
                "Service": "logging.s3.amazonaws.com"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::rds-task/*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "533267223585"
                }
            }
        }
    ]
}
