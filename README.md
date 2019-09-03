# MultiMail 

**MultiMail** helps you to send mails from your Laravel application from multiple different email accounts. It also offers help for sending queued, translatable or bulk mails.

## Requirments

Laravel 5.6 or above.


## Installation 

Install the package into your Laraval application with composer:

    composer require iwasherefirst2/multimail 

Configure your email clients in `config/multimail.php`: 

    'emails'  => [ 
			'email@gmail.com' => 
			       [
					'pass' => env('mail_pass_1'),
	               'username' => env('mail_username_1'),
				   'from' => "Max Musterman",
				   ],
		  'email2@gmail.com' => 
			       [
					'pass' => env('mail_pass_2'),
	               'username' => env('mail_username_2'),
				   'from' => "Alice Tagarien",
				   ],
				  
If you want to send out queued emails please install a [queue driver](https://laravel.com/docs/5.8/queues#driver-prerequisites).

You may install the database driver like this:

    php artisan queue:table
    php artisan migrate

## Usage Examples

To send a mail, one has to pass to the method `to` a string `$to` of the email, or an object `$to` that implements the `Sendable` interface.
The method from receives one of the emails specified in `config/multimail.php`. The method `send` or `queue` requires a [mailable](https://laravel.com/docs/5.8/mail#generating-mailables) as input:

    // Send Mail 
    \iwasherefirst2\MultiMail::to($to)->from('email@gmail.com')->send(new App/Mail/Invitation($user, $form));
	
	// Queue Mail 
    \iwasherefirst2\MultiMail::to($to)->from('email2@gmail.com')->queue(new App/Mail/Invitation($user));
	
One may pick a language with `locale`:
	
	// Send Mail and translate blade
    \iwasherefirst2\MultiMail::to($to)->from('email@gmail.com')->locale('en')->send(new App/Mail/Invitation($user, $form));
	
	// Queue Mail and translate blade
    \iwasherefirst2\MultiMail::to($to)->from('email2@gmail.com')->locale('de')->queue(new App/Mail/Invitation($user));
	
If variables are generated in the constructor of the email, and should also be translated, then one should use the following methods:
	
	// Send Mail and translate text inside constructor
    \iwasherefirst2\MultiMail::to($to)->from('email2@gmail.com')->locale('fr')->sendWithTranslatedConstructor('App/Mail/Invitation', [$user]);
	
	// Queue mail and translate text inside constructor
	\iwasherefirst2\MultiMail::to($to)->from('email@gmail.com')->locale('fr')->queueWithTranslatedConstructor('App/Mail/Invitation', [$user]);

If a bulk message should go out, then one may do it as follows:

   \iwasherefirst2\MultiMail::from('email@gmail.com')->bulk($list, $timeout, $frequency, function($to, $mailer){ $mailer->to($to)->send(new App/Mail/Invitation($user, $form))  });

	
	