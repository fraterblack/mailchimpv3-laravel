# fraterblack/mailchimpv3-laravel
A minimal service provider to set up and use the Mailchimp APi v3 PHP library in Laravel v5.*

This service provider use Mailchimp API https://github.com/drewm/mailchimp-api. A super-simple, minimum abstraction Mailchimp API v3 wrapper, in PHP.

## Installation
You can install using Composer:

```
composer require fraterblack/mailchimpv3-laravel
```

Register the service provider in ```config/app.php``` by inserting into the ```providers``` array

```php
'providers' => [
	Fraterblack\Mailchimp\MailchimpServiceProvider::class,
]
```

To publish the default configuration file, execute the following command:

```
php artisan vendor:publish --provider="Fraterblack\Mailchimp\MailchimpServiceProvider"
```

Edit your .env file

```
MAILCHIMP_API_KEY="your-api-key-here"
```

for more info check "http://kb.mailchimp.com/accounts/management/about-api-keys#Find-or-Generate-Your-API-Key"

## How it works
This package contains a service provider, which binds an instance of an initialized Mailchimp API client to the IoC-container.

You recieve the Mailchimp API client through depencency injection already set up with your own API key.


**Usage example**

```php
class NewsletterManager
{
	protected $mailChimp;
	protected $listId = '1234567890';        // Id of newsletter list

	/**
	 * Pull the Mailchimp API instance from the IoC-container.
	 */
	public function __construct(\DrewM\MailChimp\MailChimp $mailChimp)
	{
		$this->mailChimp = $mailChimp;
	}

	/**
	 * Add a subscriber in a list
     * for more info check "http://developer.mailchimp.com/documentation/mailchimp/reference/lists/members/"
	 */
	public function addEmailToList($email)
	{
		try {
			$result = $this->mailChimp->post("lists/" . $this->listId . "/members", [
                'email_address' => $email,
                'status'        => 'subscribed',
            ]);
        } catch (\Exception $e) {
        	// do something
        }
	}

	/**
	 * Get lists
     * for more info check "http://developer.mailchimp.com/documentation/mailchimp/reference/lists/"
	 */
	public function getLists()
	{
		try {
			$result = $this->mailChimp->get("lists");
        } catch (\Exception $e) {
        	// do something
        }
	}
}

```

For more examples of usage:

MailChimp API - https://github.com/drewm/mailchimp-api

Mailchimp V3 Documentation - http://developer.mailchimp.com/documentation/mailchimp

This package is based on:

Based on https://github.com/skovmand/mailchimp-laravel