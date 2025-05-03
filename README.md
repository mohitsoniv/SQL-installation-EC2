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
#### OR (from inside the container)
```
docker exec -it sqlserver2022 /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P 'YourStrong@Passw0rd'

```
## Explanation:
#### docker run -d:
##### This command creates and runs a new Docker container in detached mode (running in the background).
#### -p 1433:1433:
##### This maps the container's SQL Server port (1433) to the same port on the host machine.
#### -e ACCEPT_EULA=Y:
##### This sets the environment variable ACCEPT_EULA to Y to accept the SQL Server license agreement.
#### -e SA_PASSWORD=<your_password>:
##### This sets the environment variable SA_PASSWORD to your desired password for the SQL Server SA account. Replace <your_password> with your actual password.
#### microsoft/mssql-server-windows-s2017:
##### This specifies the official Microsoft SQL Server image for Windows (S2017). You can find other images for different versions or Linux environments. 
## Important Notes:
#### 1. SQL Server Image:
##### Ensure you are using the correct SQL Server image for your needs (Windows or Linux, specific version).
#### 2. Port Mapping:
##### The port mapping (e.g., -p 1433:1433) allows you to connect to the SQL Server from outside the container.
#### 3. Security:
##### The SA_PASSWORD should be kept secure and should not be hardcoded in your scripts or deployment processes. Consider using secrets management tools.
#### 4. Networking:
##### You might need to configure network settings on your EC2 instance (e.g., security group rules) to allow traffic on port 1433.
#### 5. Storage:
##### You'll likely want to configure persistent storage for your SQL Server data to avoid losing data when the container is stopped or restarted. This can be achieved using Docker volumes or other storage solutions. 
## Additional Considerations:
##### 1. Before running the container, you can use this command to list available Docker images, including the Microsoft SQL Server image.
```
docker images
```
##### 2. After running the container, you can use this command to see a list of running containers and their status.
```
 docker ps
```
##### 3. This command stops a running container.
```
docker stop <container_id>
```

##### 4. This command removes a stopped container.
```
docker rm <container_id>
```
##### 5. This command displays the logs of a container, which can be helpful for troubleshooting. 
```
docker logs <container_id>
```
