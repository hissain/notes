### Installation Docker-Desktop

``` sudo apt-get update ```

Download docker-desktop and install with, 

``` sudo apt-get install ./docker-desktop-amd64.deb ```

### Installation of Docker
``` sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo systemctl start docker
sudo systemctl enable docker
```

### Working with Docker/ Docker-Desktop

``` sudo docker pull qdrant/qdrant ```

``` sudo docker run -d --name qdrant -p 6333:6333 qdrant/qdrant ```

### Start/Attach/Stop

``` sudo docker start qdrant ```

``` sudo docker attach qdrant ```

``` sudo docker stop qdrant ```
