# AfterDo [![Build Status](https://travis-ci.org/PragTob/after_do.png?branch=master)](https://travis-ci.org/PragTob/after_do)[![Code Climate](https://codeclimate.com/github/PragTob/after_do.png)](https://codeclimate.com/github/PragTob/after_do)

AfterDo is simple gem, that allows you to execute a specified block after specified method of a class are called. If the class extends `AfterDo` you can simply do this by `MyClass.after :some_method do puts 'whatever you want?' end`

This shall not be done to to alter behavior or something but rather to fight cross-cutting concerns such as logging. E.g. with logging you litter all your code wit logging statements - that concern is spread over many files. With AfterDo you could put all the logging in one file.

AfterDo has no external runtime dependencies and the code is just a bit more than 60 lines (blank lines included).

## Installation

Add this line to your application's Gemfile:

    gem 'after_do'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install after_do

## Usage

With AfterDo you can do simple things like putting something out everything a method is called as in this example:

```ruby
class Dog
  def bark
    puts 'Woooof'
  end
end

Dog.extend AfterDo
Dog.after :bark do puts 'I just heard a dog bark!' end

dog = Dog.new
dog2 = Dog.new

dog.bark
dog2.bark

# Output is:
# Woooof
# I just heard a dog bark!
# Woooof
# I just heard a dog bark!

```

As another example: If you have an activity and want to activity to be saved every time you change it, but you don't want to mix that persistence concern with what the activity actually does you could do something like this:

```ruby
persistor  = FilePersistor.new
Activity.extend AfterDo
Activity.after :start, :pause, :finish, :resurrect,
             :do_today, :do_another_day do |activity|
  persistor.save activity
end
```

Doesn't that seem a lot drier then calling some save method manually after each of those in addition to separating the concerns?

## Is there a before method?

No not yet, I didn't have a use case for it yet. If you have one please let me know, it is relatively easy to add.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
