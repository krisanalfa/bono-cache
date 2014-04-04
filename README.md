#BonoCache
Laravel Cache for Bono PHP Framework.

##Configuration
Add these lines to your configuration file.
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

The configuration takes three settings, they are `driver`, `path`, and `prefix`.

####Driver
This option controls the default cache "driver" that will be used when using the Caching library.
Of course, you may use other drivers any time you wish.

**Supported Driver:**
- Now : `file`, `array`, `apc`
- Later: `norm`, `redis`, `memcached`


####Path
When using the `file` cache driver, we need an absolute location where the cache files may be stored.

##Cache Usage

#### Storing An Item In The Cache
```php
$app->cache->put('key', 'value', $minutes);
```

#### Using Carbon Objects To Set Expire Time
```php
$expiresAt = Carbon::now()->addMinutes(10);

$app->cache->put('key', 'value', $expiresAt); // cache will be expired at 10 minutes later
```

#### Storing An Item In The Cache If It Doesn't Exist
```php
$app->cache->add('key', 'value', $minutes);
```

The `add` method will return `true` if the item is actually **added** to the cache.
Otherwise, the method will return `false`.

#### Checking For Existence In Cache
```php
if ($app->cache->has('key'))
{
    //
}
```

#### Retrieving An Item From The Cache
```php
$value = $app->cache->get('key');
```

#### Retrieving An Item Or Returning A Default Value
```php
$value = $app->cache->get('key', 'default');

$value = $app->cache->get('key', function() { return 'default'; });
```

#### Storing An Item In The Cache Permanently
```php
$app->cache->forever('key', 'value');
```

Sometimes you may wish to retrieve an item from the cache, but also store a default value if the requested item doesn't exist. You may do this using the `$app->cache->remember` method:

```php
$value = $app->cache->remember('users', $minutes, function()
{
    return Norm::factory('User')->find();
});
```

You may also combine the `remember` and `forever` methods:

```php
$value = $app->cache->rememberForever('users', function()
{
    return Norm::factory('User')->find();
});
```

Note that all items stored in the cache are serialized, so you are free to store any type of data.

#### Removing An Item From The Cache
```php
$app->cache->forget('key');
```

## Cache Tags

> **Note:** Cache tags are not supported when using the `file` cache drivers. Furthermore, when using multiple tags with caches that are stored "forever", performance will be best with a driver such as `memcached`, which automatically purges stale records.

Cache tags allow you to tag related items in the cache, and then flush all caches tagged with a given name.
To access a tagged cache, use the `tags` method:

#### Accessing A Tagged Cache

You may store a tagged cache by passing in an ordered list of tag names as arguments, or as an ordered array of tag names.

```php
$app->cache->tags('people', 'authors')->put('Alfa', $alfa, $minutes);

$app->cache->tags(array('people', 'artists'))->put('Ganesha', $ganesha, $minutes);
```

You may use any cache storage method in combination with tags, including `remember`, `forever`, and `rememberForever`.
You may also access cached items from the tagged cache, as well as use the other cache methods such as `increment` and `decrement`:

#### Accessing Items In A Tagged Cache

To access a tagged cache, pass the same ordered list of tags used to save it.
```php
$ganesha = $app->cache->tags('people', 'artists')->get('Ganesha');

$alfa = $app->cache->tags(array('people', 'authors'))->get('Alfa');
```

You may flush all items tagged with a name or list of names.
For example, this statement would remove all caches tagged with either `people`, `authors`, or both.
So, both "Ganesha" and "Alfa" would be removed from the cache:

```php
$app->cache->tags('people', 'authors')->flush();
```

In contrast, this statement would remove only caches tagged with `authors`, so "Alfa" would be removed, but not "Ganesha".

```php
$app->cache->tags('authors')->flush();
```

##Read More
For more information about Laravel Cache, read [this](http://laravel.com/api/4.1/Illuminate/Cache/Repository.html).
