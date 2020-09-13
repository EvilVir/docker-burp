# Burp Client in Docker @ arm32v7

Forked from great images made by Peter Schiffer (go there if you're looking for Fedora AMD64/ARM64 images of all components):

https://hub.docker.com/r/pschiffe/burp-server/  
https://hub.docker.com/r/pschiffe/burp-client/  
https://hub.docker.com/r/pschiffe/burp-ui/  

## Burp Client

To backup data and send them to the Burp server, you need Burp client. Usage of this image is pretty simple. With `BURP_SERVER` and `BURP_SERVER_PORT` env vars you can specify an address and port of the Burp server, client password goes in `BURP_CLIENT_PASSWORD` env var and everything you need to backup just mount to `/tobackup` directory in the container. Be aware, however, that you might need to use the `--security-opt label:disable` option when accessing various system directories on the host. It's also possible to link to Burp server container with alias `burp` and I would recommend setting the container hostname to something meaningful, as the client will be identified with its hostname in the Burp server configuration.

### Persistent data

`/etc/burp` persistent directory in the container stores Burp client configuration.

### Example

```
docker run -d --name burp-client \
  -e BURP_SERVER=some-server.host \
  -e BURP_CLIENT_PASSWORD=super-secret \
  -v burp-client-conf:/etc/burp \
  -v /etc:/tobackup/somehost-etc:ro \
  -v /home:/tobackup/somehost-home:ro \
  -v some-docker-vol:/tobackup/some-docker-vol:ro \
  --hostname $HOSTNAME \
  --security-opt label:disable \
  strangefog/burp-client:arm32v7
```

Once this container is created, it will backup the specified data and exit. After that, it's recommended to start the container ~ every 20 minutes with `docker start burp-client`, so it can check with the server and backup new files if scheduled. The schedule is determined by the server, not the client.

### Data recovery

You may also use it to recover data from a distant server.
You just have to provide arguments as you would to a regular burp client CLI.

for example:

```
docker run -d --name burp-client \
  -e BURP_SERVER=some-server.host \
  -e BURP_CLIENT_PASSWORD=super-secret \
  -v burp-client-conf:/etc/burp \
  -v /etc:/tobackup/somehost-etc \
  --hostname $HOSTNAME \
  --security-opt label:disable \
  strangefog/burp-client:arm32v7 \
  -al /tobackup/somehost-etc
```

