# Using Minio and goofys
> When you have two server, one of which is busy computing, 
> you can using Minio and goofys to set cloud storage service, 
> and using another computational source.

## 1.Minio

### 1.1 download a standalone MinIO server
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio

### 1.2 run MinIO
MINIO_ROOT_USER=highbhbj  MINIO_ROOT_PASSWORD=12345678 ./minio server --address 192.168.1.*:9000 /data/minio_oss_srv
1. once using MINIO_ROOT_USER & MINIO_ROOT_PASSWORD
2. --address to avoid public access

#### set enviornment variable
export MINIO_ACCESS_KEY=*
export MINIO_SECRET_KEY=*

#### run background
nohup ./minio server --address 192.168.1.*:9000 /data/minio_oss_srv > /data/logs/minio/minio.log 2>&1 &

### 1.3 set 9000 open
sudo ufw allow 9000
sudo systemctl restart ufw

## 2.mc

### 2.1 download mc
wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc

### 2.2 Add a Cloud Storage Service
mc alias set <ALIAS> <YOUR-S3-ENDPOINT> <YOUR-ACCESS-KEY> <YOUR-SECRET-KEY> --api <API-SIGNATURE> --path <BUCKET-LOOKUP-TYPE>
  
./mc alias set minio http://192.168.1.189:9000 highbhbj 12345678

### 2.3 using mc

#### List all buckets
./mc ls minio

#### Make a bucket
./mc mb minio/test

#### Copy Objects
./mc cp .minio minio/test

## 3.goofys
> Goofys allows you to mount an S3 bucket as a filey system.

### 3.1 download goofys
go to github https://github.com/kahing/goofys
chmod +x goofys

### 3.2 run goofys
mkdir .aws
vim credentials
[default]
aws_access_key_id = AKID1234567890
aws_secret_access_key = MY-SECRET-KEY

goofys <bucket> <mountpoint>
./goofys --endpoint http://192.168.1.189:9000/ test /home/songfeng/test
./goofys --endpoint http://192.168.1.189:9000 --debug_s3 test /home/songfeng/test (debug mode)

----------

Then you can using the data from another server at /home/songfeng/test

Enjoy~
