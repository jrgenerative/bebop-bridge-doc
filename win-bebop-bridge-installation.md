# Install Bebop Bridge on Windows

## Pull Latest Images

```
REM pull latest images
docker pull jrgenerative/bebop-bridge-client
docker pull jrgenerative/bebop-bridge-service
```

## Start Bebop Bridge Services

```
REM Start stuff
start docker run --name bridge-service -p 43210:43210/udp -p 4000:4000 jrgenerative/bebop-bridge-service
start docker run --name bridge-client -p 8080:8080 jrgenerative/bebop-bridge-client
```

## Stop Bebop Bridge Services

```
REM Stop stuff
docker stop -t 5 bridge-service
docker rm -f bridge-service
docker stop -t 5 bridge-client
docker rm -f bridge-client
```

## Clean-up Orpahn Images

TODO
