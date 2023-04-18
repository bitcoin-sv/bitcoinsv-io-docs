---
description: Run your own Peer Channels Service
---

# Setup & Usage

The default Peer Channels host for using the wallet is run by the Bitcoin Association and can be used at `https://infra.bitcoinsv.io`

However, you can run your own Peer Channels server, using the docker compose below, and updating your Wallet environment variable `PEERCHANNELS_HOST` to match the domain you're hosting this service from.

```yaml
version: "3.7"
 
services:
  peer-channels-db:
    container_name: peer-channels-db
    image: postgres
    volumes:
      - peer-channels-volume:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: postgres
    networks:
      - peer-channels-network

  peer-channels-api:
    container_name: peer-channels
    image: bitcoinsv/spvchannels:latest
    command: -startup
    ports:
      - "${HTTPSPORT}:443"
    links:
      - peer-channels-db:peer-channels-db
    depends_on:
      - peer-channels-db
    volumes:
      - ./config/:/config/:ro
    environment:
      - AppConfiguration:DBConnectionString=Server=spvchannels-db;Port=5432;User Id=channels;Password=channels;Database=channels;
      - AppConfiguration:DBConnectionStringDDL=Server=spvchannels-db;Port=5432;User Id=channelsddl;Password=channels;Database=channels;
      - AppConfiguration:DBConnectionStringMaster=Server=spvchannels-db;Port=5432;User Id=postgres;Password=postgres;Database=channels;
      - AppConfiguration:NotificationTextNewMessage=${NOTIFICATIONTEXTNEWMESSAGE}
      - AppConfiguration:MaxMessageContentLength=${MAXMESSAGECONTENTLENGTH}
      - AppConfiguration:ChunkedBufferSize=${CHUNKEDBUFFERSIZE}
      - AppConfiguration:TokenSize=${TOKENSIZE}
      - AppConfiguration:CacheSize=${CACHESIZE}
      - AppConfiguration:CacheSlidingExpirationTime=${CACHESLIDINGEXPIRATIONTIME}
      - AppConfiguration:CacheAbsoluteExpirationTime=${CACHEABSOLUTEEXPIRATIONTIME}
      - AppConfiguration:FirebaseCredentialsFilePath=/config/${FIREBASECREDENTIALSFILENAME}
      - ASPNETCORE_ENVIRONMENT=Production
      - ASPNETCORE_NPGSQLLOGMANAGER=${NPGSQLLOGMANAGER}
      - ASPNETCORE_URLS=https://+:443
      - ASPNETCORE_HTTPS_PORT=${HTTPSPORT}
      - ASPNETCORE_Kestrel__Certificates__Default__Password=${CERTIFICATESPASSWORD}
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/config/${CERTIFICATEFILENAME}
    networks:
      - peer-channels-network
      
volumes:
  peer-channels-volume:

networks:
  peer-channels-network:
```

