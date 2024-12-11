# Infomaniak

```bash
composer require socialiteproviders/infomaniak
```

## Installation & Basic Usage

Please see the [Base Installation Guide](https://socialiteproviders.com/usage/), then follow the provider specific instructions below.

## Specific configuration

Follow [this link](https://manager.infomaniak.com/v3/ng/profile/user/applications/list) to create your application's credentials at Infomaniak's.

Chose "Application" app type, specify a name and the redirect URL.

### Add configuration to `config/services.php`

```php
'infomaniak' => [
  'client_id' => env('INFOMANIAK_CLIENT_ID'),
  'client_secret' => env('INFOMANIAK_CLIENT_SECRET'),
  'redirect' => env('INFOMANIAK_REDIRECT_URI')
],
```

### Add provider event listener

#### Laravel 11+

In Laravel 11, the default `EventServiceProvider` provider was removed. Instead, add the listener using the `listen` method on the `Event` facade, in your `AppServiceProvider` `boot` method.

* Note: You do not need to add anything for the built-in socialite providers unless you override them with your own providers.

```php
Event::listen(function (\SocialiteProviders\Manager\SocialiteWasCalled $event) {
    $event->extendSocialite('infomaniak', \SocialiteProviders\Infomaniak\Provider::class);
});
```
<details>
<summary>
Laravel 10 or below
</summary>
Configure the package's listener to listen for `SocialiteWasCalled` events.

Add the event to your `listen[]` array in `app/Providers/EventServiceProvider`. See the [Base Installation Guide](https://socialiteproviders.com/usage/) for detailed instructions.

```php
protected $listen = [
    \SocialiteProviders\Manager\SocialiteWasCalled::class => [
        // ... other providers
        \SocialiteProviders\Infomaniak\InfomaniakExtendSocialite::class.'@handle',
    ],
];
```
</details>

### Usage

You should now be able to use the provider like you would regularly use Socialite (assuming you have the facade installed):

```php
return Socialite::driver('infomaniak')->redirect();
```