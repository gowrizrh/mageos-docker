Docker Setup for MageOS

Shell into the container
```sh
docker compose exec php bash
```

Example Install Command

```bash
bin/magento setup:install \
--cleanup-database \
--use-secure=1 \
--use-secure-admin=1 \
--base-url=https://mageos.test:8080/ \
--db-host=db \
--db-name=magento2 \
--db-user=magento2 \
--db-password=magento2 \
--backend-frontname=admin \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=password1 \
--language=en_AU \
--currency=AUD \
--timezone=Australia/Adelaide \
--use-rewrites=1 \
--search-engine=elasticsearch7 \
--elasticsearch-host=es \
--elasticsearch-port=9200 \
--elasticsearch-index-prefix=magento2 \
--elasticsearch-timeout=15 \
--amqp-host=mq \
--amqp-port=5672 \
--amqp-user=guest \
--amqp-password=guest
```

# Configure Redis for caching
```sh
bin/magento setup:config:set --cache-backend=redis --cache-backend-redis-server=redis --cache-backend-redis-db=0
```

```sh
bin/magento setup:config:set --page-cache=redis --page-cache-redis-server=redis --page-cache-redis-db=1
```
