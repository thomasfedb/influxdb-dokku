# InfluxDB for Dokku

A dead simple InfluxDB deployment for your friendly local Dokku PaaS.

## Quick Start

### Prerequisites

1. [Install Dokku](https://dokku.com/docs/getting-started/installation/) on the intended host

   ```bash
   wget https://raw.githubusercontent.com/dokku/dokku/v0.27.7/bootstrap.sh;
   sudo DOKKU_TAG=v0.27.7 bash bootstrap.sh
   ```

2. Install the [dokku-letsencrypt](https://github.com/dokku/dokku-letsencrypt) plugin

   ```bash
   sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
   dokku letsencrypt:cron-job --add
   ```

### Deployment

1. Create the app and attach a data directory

   ```bash
   dokku apps:create influxdb
   dokku storage:ensure-directory influxdb-data
   dokku storage:mount influxdb /var/lib/dokku/data/storage/influxdb-data:/var/lib/influxdb2
   ```

2. Configure port mapping, set a domain, and enable SSL.

   ```bash
   dokku proxy:ports-add influxdb http:80:8086
   dokku domains:set influxdb influxdb.your.domain.tld
   ```

3. Deploy repository to Dokku

   ```bash
   git remote add production dokku@host.your.domain.tld:influxdb
   git push production main
   ```
