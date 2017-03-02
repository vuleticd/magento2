

1. Backup the database `mysqldump -u DBUSER -p DBNAME > pre_upgrade.sql` OR `bin/magento setup:backup --db`
2. Switch to maintenance mode `bin/magento maintenance:enable`
3. Update composer project `composer require magento/product-community-edition 2.1.5 --no-update`
4. Run `composer update`
5. Manually clear caches `sudo rm -rf var/cache/* && sudo rm -rf var/page_cache/* && sudo rm -rf var/generation/* && sudo rm -rf var/di/*`
6. Run Magento upgrade scripts `php bin/magento setup:upgrade`
7. Reset the permissions 
```
sudo find var vendor pub/static pub/media app/etc -type f -exec chmod g+w {} \;
sudo find var vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} \;
sudo chmod u+x bin/magento
```
8. Disable maintenance mode `bin/magento maintenance:disable`
9. Deploy `bin/magento setup:static-content:deploy`
