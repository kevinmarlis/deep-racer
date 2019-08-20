1. **docker ps**
2. make note of container id of which logs you want
3. **docker inspect --format='{{.LogPath}}' YOUR CONTAINER ID**
3. make not of .log filename 
4. **docker logs YOUR CONTAINER ID >& YOUR LOG FILENAME**
