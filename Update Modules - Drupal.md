# Update Modules

## To update core, all outdated modules and theme, update the db, clear the cache:
```
composer update "drupal/*" --with-all-dependencies
drush updb
drush cr
```

## To update specific module or update things one at the time, see below.

Use Composer's built-in command for listing packages that have updates available:

`composer outdated "drupal/*"`

For a minor-version update to a given Drupal module/project, simply use the following:

`composer update drupal/modulename --with-all-dependencies`

For a major-version upgrade (such as 1.x to 2.x), update alone isn't enough: you must instead require the new major version explicitly. 

`composer require drupal/modulename:^2.0`

Finally, run :
```
drush updb
drush cr
drush cex 
```
