# SQL-installation on EC2 by Mohit Soni (mohitsoniv)
## Resourses 
### * Atleast 1 vCPU & 2 GB RAM require for SQL server on AWS Instance (EC2).

## 1. Installing Docker on Linux Ubuntu EC2 Machine 
```
sudo apt-get update
```
```
sudo apt-get install docker.io -y
```
```
sudo systemctl start docker
```
```
sudo systemctl enable docker
```
```
sudo systemctl status docker
```
## 2. Pull the latest SQL Server 2022 image
```
docker pull mcr.microsoft.com/mssql/server:2022-latest
```
## 3. Run the container
```
docker run -e "ACCEPT_EULA=Y" \
  -e "MSSQL_SA_PASSWORD=YourStrong@Passw0rd" \
  -p 1433:1433 \
  --name sqlserver2022 \
  -d mcr.microsoft.com/mssql/server:2022-latest
```
 Replace YourStrong@Passw0rd with a strong password
 ## 4. Check that the container is running
 ```
docker ps
```
## 5. Connect to the SQL Server (if sqlcmd install)
```
sqlcmd -S localhost -U sa -P 'YourStrong@Passw0rd'
```
## 6. Install sqlcmd on Ubuntu
```
# Import the Microsoft repository GPG key
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Register the Microsoft Ubuntu repository
sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list)"

# Update package lists
sudo apt update

# Install sqlcmd and tools
sudo apt install -y mssql-tools unixodbc-dev

# Optional: Add tools to PATH
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
```
### OR (from inside the container)
```
docker exec -it sqlserver2022 /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P 'YourStrong@Passw0rd'

```
