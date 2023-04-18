---
cover: ../../.gitbook/assets/Dark Banner_HeaderSVv1.0.png
coverY: 0
---

# Setup

### Installation

You can build the docker file by either running `docker build -t headersv .` from the root directory, or `docker-compose build`. Once built, you can run either `docker run headersv` or `docker-compose up`.

**Note:** If you're not using `docker compose`, you'll also need to expose the port and mount a volume to expose the API and persist state.

### Verifying

To check that your client is serving REST API calls once running, open a browser at `http://localhost:8001/api/v1/chain/tips` (default port is specified in the `docker-compose` file.)

### Configuration

The Application has 4 default profiles.

* `bsv-mainet`
* `bsv-testnet`
* `bsv-stnnet`
* `bsv-regtest`

Switch between profiles by altering the value `spring.profiles.active=bsv-xxx` in `resources/application.yml`. Profiles can be amended to alter values such as `minPeers` and `maxPeers`.

The profiles represent the networks available to the application.

**Note:** Docker profiles can also be amended by overriding using environment variables such as those in the `docker compose` file.
