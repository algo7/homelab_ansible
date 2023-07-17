# Create an Access Key via the Minio UI With the Policy Below
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*"
            ],
            "Resource": [
                "arn:aws:s3:::bucket_name",
                "arn:aws:s3:::bucket_name/*"
            ]
        },
        {
            "Effect": "Deny",
            "Action": [
                "admin:*"
            ]
        }
    ]
}
```
# Set Default S3 Access Key and Secret Key on All Elastic Search Master Nodes
```bash
# Go into the Elastic Search Container
docker exec -it <container_name_or_id> bash
# Set the default access key and secret key
/usr/share/elasticsearch/bin/elasticsearch-keystore add s3.client.default.access_key
# Enter the access key
/usr/share/elasticsearch/bin/elasticsearch-keystore add s3.client.default.secret_key
# Enter the secret key
```

# Reload Secure Settings on All Elastic Search Master Nodes
```bash
# Go to the Kibana Console
# Run the following command
POST _nodes/reload_secure_settings
{
  "secure_settings_password": "keystore_password_or_empty_if_not_set" 
}
```
# Create S3 Repository via Kibana Console
```bash
PUT _snapshot/minio
{
  "type": "s3",
  "settings": {
    "bucket": "bucket_name",
    "endpoint": "url:port",
    "compress": true,
    "protocol":"http"
  }
}
```