
# Commands
```
// start server
php artisan serve

// Check the env file APP_URL

// No application encryption key has been specified
php artisan key:generate


// when you don't have a database, migrate

.env > DB_CONNECTION=sqlite

php artisan migrate

// database seed
php artisan migrate:fresh --seed
```
