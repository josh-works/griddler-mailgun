# Griddler::Mailgun

[![Code Climate](https://codeclimate.com/github/bradpauly/griddler-mailgun.png)](https://codeclimate.com/github/bradpauly/griddler-mailgun)

This was extracted from the [griddler gem](https://github.com/thoughtbot/griddler) and is used to
parse emails forwarded to your application from [mailgun](http://mailgun.com/).

**IMPORTANT:** Please use version 1.0.1 or higher. There was a bug in version 1.0.0.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'griddler', '~> 1.1.0'
gem 'griddler-mailgun', '~> 1.0.3'
```

Then execute:

```
bundle install
```

## Usage

[Griddler](http://griddler.io/) comes with a route that, when used, handles the params passed to your application and then sends a Griddler::Email to EmailProcessor#process. Read [a blog post about it](http://robots.thoughtbot.com/handle-incoming-email-with-griddler) on the Thoughtbot blog.

#### Getting mailgun to POST to your application

Mailgun uses [routes](http://documentation.mailgun.com/user_manual.html#routes) to filter incoming emails and forward them to your app. A route is made up of a filter and an action. [The filter](http://documentation.mailgun.com/api-routes.html#filters) matches emails based on its properties and [the action](http://documentation.mailgun.com/api-routes.html#actions) determines what happens to the matched email.

For example, to forward any email sent to an example.net address to http://example.com/email_processor
the following route would be used:

Filter Expression:

```
match_recipient(".*@example.net")
```

Action:

```
forward("http://example.com/email_processor")
```

#### Notes on testing

If you want to test the functionality of the route, via `rspec` or `minitest` or in Postman, please note that the `to`, `from`, `bcc` and `cc` params you post _must_ be capitalized:

This would _not_ work:

```json
{
	"recipient": "recipient@domain.com",
	"from": "this wont work <whywontyouwork@working.foo>",
	"to": "to@domain.com",
	"subject": "griddler-mailgun",
	"body-plain": "This is some text body",
	"body-html": "Supports <em>HTML</em> as well."
}
```

On the other hand, this would:

```json
{
	"recipient": "recipient@domain.com",
	"From": "this wont work <whywontyouwork@working.foo>",
	"To": "to@domain.com",
	"subject": "griddler-mailgun",
	"body-plain": "This is some text body",
	"body-html": "Supports <em>HTML</em> as well."
}
```
You can explore the expected fields and capitalizations in the [`Griddler::Mailgun::Adapter#normalize_params`](https://github.com/bradpauly/griddler-mailgun/blob/master/lib/griddler/mailgun/adapter.rb#L15) method. 


## More Information

* [mailgun](http://www.mailgun.com)
* [mailgun API - Routes](http://documentation.mailgun.com/api-routes.html)
* [mailgun User Maunual - Routes](http://documentation.mailgun.com/user_manual.html#routes)

## Credits

Griddler::Mailgun was extracted from Griddler by Brad Pauly.

Griddler was written by Caleb Thompson and Joel Oliveira.

## License

Griddler::Mailgun is Copyright © 2014 Brad Pauly. It is free
software, and may be redistributed under the terms specified
in the LICENSE file.
