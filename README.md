#BonoCache
Laravel Cache for Bono PHP Framework

##How to use
Add these lines to your configuration file
```php
'bono.providers' => array(
    '\\KrisanAlfa\\Cache\\Provider\\CacheProvider'
),

'cache' => array(
    'driver' => 'file',
    'path' => __DIR__ . '/../../cache',
    'prefix' => 'bono',
)
```

Now you can access `cache` by resolve them via Bono IoC like this:
```php
$app->cache->put('someName', 'someValue', $minutes);

var_dump($app->cache->get('someName'))
```
##Configuration
The configuration takes three settings, they are `driver`, `path`, and `prefix`.

###Driver
This option controls the default cache "driver" that will be used when using the Caching library. Of course, you may use other drivers any time you wish.

**Supported Driver:**
```
- Now  : "file", "array", "apc"
- Later: "norm", "redis", "memcached"
```

###Path
When using the "file" cache driver, we need an absolute location where the cache files may be stored.

###Prefix
When utilizing a RAM based store such as APC or Memcached, there might be other applications utilizing the same cache. So, we'll specify a value to get prefixed to all our keys so we can avoid collisions.

##Read More
For more information about Laravel Cache, read [this](http://laravel.com/api/4.1/Illuminate/Cache/Repository.html).
