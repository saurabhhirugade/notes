
# Docker

Options:
- a : Shows all entries
- q :Quiet mode (Only shows IDs)

Basic:
```
docker ps - List containers
docker image ls - List of all images
docker start  <ids> - Start one or more stopped containers 
docker stop <ids> - Stop one or more stopped containers
docker rm <container-id> - Remove specific container
docker rmi <image-id> - Remove specific image
docker system df - Show docker disk usage
socker system prune - Remove unused data
docker exec <container> <command> - Run the command inside the container

```
Custom:

To remove all containers

	 docker rm $(docker stop $(docker ps -aq))
To remove all images

	docker rmi $(docker image ls -aq)

# Colima

Start the colima (dafault values cpu:1, memory:4, disk:60)

	colima start --cpu 4 --memory 10 --disk 100	
Stop the default profile instance

	colima stop
Delete the default profile instance

	colima delete 
Shows current status 

	colima status

Create new instance with new profile name
```
colima start <profile>
```



