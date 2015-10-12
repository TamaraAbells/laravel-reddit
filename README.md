# laravel-reddit
Laravel Reddit-like Community

Still a work in progress...

# Packages Used
1. ["intervention/image"](https://github.com/Intervention/image)
2. ["oscarotero/Embed"](https://github.com/oscarotero/Embed)

# Features
1. Login/Register
2. Create Subreddits
3. Create Posts (link and text) and grab their meta-data
4. Assign Moderators to your Subreddits with ability for mods to edit posts
5. Search Subreddits
6. Pagination

# To-Do
1. Comments
2. Sorting

# Installation
1. git clone https://github.com/Halnex/laravel-reddit projectname
2. composer install
3. php artisan migrate

Open AuthServiceProvider.php and add the following method to top of your class
```php
use Illuminate\Auth\Access\Gate;
use Illuminate\Contracts\Auth\Access\Gate as GateContract;
use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;
```
Now add the following method to the class in AuthServiceProvider.php
```php
public function boot(GateContract $gate)
{
    parent::registerPolicies($gate);

    $gate->define('update-post', function ($user, $post) {
        if ($user->id === $post->subreddit->user->id) {
            return true;
        }

        if ($user->id === $post->user_id) {
            return true;
        }
        
        if ($isModerator) {
            return true;
        }

        return false;
    });

    $gate->define('update-sub', function($user, $subreddit) {
        if($user->id === $subreddit->user->id) {
            return true;
        }

        return false;
    });    
}
```
