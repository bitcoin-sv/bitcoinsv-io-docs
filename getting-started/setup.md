---
description: Run a LiteClient Wallet with Docker Compose
cover: ../.gitbook/assets/Dark Banner_Setupv2.png
coverY: 0
---

# 🛠 Setup

The quickest way to get set up is with [docker compose](https://docs.docker.com/compose/). Below are the Docker compose yaml files for each Configuration. You can just copy paste the content into a directory, taking care to replace any `domain.tld` with your own domain name. There is only one component which requires a special DNS record, the Paymail Server.&#x20;

<details>

<summary>Wallet &#x26; Headers Only</summary>

```yaml
version: "3.7"

services:
  payd:
    restart: always
    image: libsv/payd:0.1.13
    environment:
      DB_DSN: "file:paydb/wallet.db?cache=shared&_foreign_keys=true"
      DB_SCHEMA_PATH: "migrations"
      MAPI_CALLBACK_URL: "https://domain.tld/api/v1/proofs" # replace domain with yours
      MAPI_CALLBACK_HOST: "https://domain.tld"
      MAPI_MINERURL: "https://mapi.taal.com"
      MAPI_TOKEN: "Bearer mainnet_3af382fadbc448b15cc4133242ac2621" # get credentials from https://console.taal.com
      MAPI_MINERNAME: "TAAL"
      TRANSPORT_SOCKETS_ENABLED: "false"
      DPP_HOST: "http://dpp:8445"
      WALLET_NETWORK: "mainnet"
      # Peer Channels Service provided by Bitcoin Association.
      # Not possible to run without Peer Channels without modifying code.
      PEERCHANNELS_HOST: "infra.bitcoinsv.io"
      PEERCHANNELS_PATH: "peerchannels"
      PEERCHANNELS_TLS: 'true'
    ports:
      - 8443:8443
    networks:
      - bitcoin-lite-client
    volumes:
      - bitcoin-lite-client-data:/data

  headersv:
    restart: always
    image: bitcoinsv/block-headers-client:2.0.2
    container_name: headersv
    volumes:
      - bitcoin-lite-client-data:/tmp/jcl
    environment:
      - SPRING_PROFILES_ACTIVE=bsv-mainnet
      - _JAVA_OPTIONS=-XX:+UseContainerSupport -XX:MaxRAMPercentage=75 -Dlogging.level.ROOT=DEBUG
    ports:
      - 8080:8080
    networks:
      - bitcoin-lite-client

networks:
  bitcoin-lite-client:
    name: bitcoin-lite-client
    external: false

volumes:
  bitcoin-lite-client-data:
    external: false
```

</details>

<details>

<summary>With DPP Proxy</summary>

```yaml
# add to the existing compose yml

services:
  dpp-proxy:
    container_name: dpp-proxy
    image: bitcoinsv/dpp-proxy
    environment:
      TRANSPORT_MODE: hybrid
      SERVER_FQDN: 'https://your-domain.tld' # add real domain
      SERVER_HOST: "dpp-proxy"
      SERVER_PORT: ":8445"
    ports:
      - "8445:8445"
    networks:
      - lite-client
    extra_hosts:
      - "host.docker.internal:host-gateway"
```

</details>

<details>

<summary>With Peer Channels</summary>

```yaml
# add to the existing compose yml
 
services:
  peer-channels-db:
    container_name: peer-channels-db
    image: postgres
    volumes:
      - peer-channels-volume:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: postgres
    networks:
      - lite-client

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
      - lite-client
      
volumes:
  peer-channels-volume:

```

</details>

<details>

<summary>With Paymail Server</summary>

Requires DNS records and inital database migration - details on the dedicated documentation page: [Paymail Server](../toolbox-components/paymail-server/).

```yaml
# add to existing componse file

services:
  paymail:
    image: bitcoinsv/paymail-server
    restart: always
    environment:
      DB_DSN: "file:paydb/paymail.db?cache=shared&_foreign_keys=true"
      DB_SCHEMA_PATH: "migrations/paymail"
      DOMAIN_TLD: "yourdomain.tld"
      PAYMAIL_ROOT: "https://yourdomain.tld"
      PAYD_HOST: "payd"
      PAYD_PORT: ":8443"
    volumes:
      - bitcoin-lite-client-data:/paydb
    ports:
      - "8446:8446"
    networks:
      - bitcoin-lite-client

```

</details>

