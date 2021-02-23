---
layout: post
title: Quick, easy and modern CLI tools with Ruby
date: 2021-02-22 20:16 -0600
category: Scripting
tags: [ruby]
---

2020 was a different year for obvious reasons, and for the first time ever, I saw myself writing a lot of Ruby (because of work) and choosing Ruby over Python for quick scripts. This is something I never thought was going to happen (I really love Python).

So what's the reason for the change? It's simple: You can make really cool and modern CLI applications with Ruby really easy, and even better, without needing additional libraries.

Here's a quick tutorial on how to quickly create CLI tools and scripts using only Ruby.

<!--more-->

Let's start with an example: If you need to write a CLI tool with Python, you can just `import sys` and parse `sys.argv` yourself:

```python
import sys

print("The first argument is: ", sys.argv[1])
```

And then just:
```bash
$ python python_test.py 1
The first argument is:  1
```

However, most Unix CLI tools look like this:
```bash
aws ec2 run-instances --image-id ami-xxxxxxxx --count 1 --instance-type t2.micro
```

So parsing every argument manually, if possible, is tedious, so you'll probably want to use something like [Click](https://click.palletsprojects.com/):

```python
# This snippet was taken from Click's homepage
import click

@click.command()
@click.option('--count', default=1, help='Number of greetings.')
@click.option('--name', prompt='Your name',
              help='The person to greet.')
def hello(count, name):
    """Simple program that greets NAME for a total of COUNT times."""
    for x in range(count):
        click.echo('Hello %s!' % name)

if __name__ == '__main__':
    hello()
```

But that's an external dependency, so you'll probably have to create a `requirements.txt` and install the dependency on every machine you'll want to run your script.

## Now let's take a look at Ruby

With Ruby you can use the `optparse` package (Or gem? I'm still new to the Ruby world) without installing any additional gem. You just need to `require optparse` and initialize an `OptionParser` object:

```ruby
require 'optparse'

@options = {}
OptionParser.new do |opts|
  opts.banner = "Usage: ruby_cli.rb [options]"
  opts.on("-n", "--number NUMBER", "The number of times you want the loop to run") do |number|
    @options[:number] = number
  end
  opts.on("-h", "--help", "Prints this help") do
    puts opts
    exit
  end
end.parse!
```

And the you'll have a handy `@options` hash that will hold the parameters provided by the user, that you can use later for your script's logic:
```ruby
require 'optparse'

@options = {}
OptionParser.new do |opts|
  opts.banner = "Usage: ruby_cli.rb [options]"
  opts.on("-n", "--number NUMBER", "The number of times you want the loop to run") do |number|
    @options[:number] = number
  end
  opts.on("-h", "--help", "Prints this help") do
    puts opts
    exit
  end
end.parse!

@options[:number].to_i.times do 
  puts "This is a Ruby CLI Tool!"
end
```

```bash
$ ruby ruby_cli.rb --number 5 # Long notation
This is a Ruby CLI Tool!
This is a Ruby CLI Tool!
This is a Ruby CLI Tool!
This is a Ruby CLI Tool!
This is a Ruby CLI Tool!

$ ruby ruby_cli.rb -n 5 # Short-hand notation
This is a Ruby CLI Tool!
This is a Ruby CLI Tool!
This is a Ruby CLI Tool!
This is a Ruby CLI Tool!
This is a Ruby CLI Tool!
```

You even get a *professional-looking* help dialog!
```bash
$ ruby ruby_cli.rb --help # Long notation
Usage: ruby_cli.rb [options]
    -n, --number NUMBER              The number of times you want the loop to run
    -h, --help                       Prints this help

    $ ruby ruby_cli.rb -h # Short-hand notation
Usage: ruby_cli.rb [options]
    -n, --number NUMBER              The number of times you want the loop to run
    -h, --help                       Prints this help
```

And that's it! The `optparse` gem is bigger than that, and can do all sorts of useful things. For more, please [refer to the OptionParser documentation](https://ruby-doc.org/stdlib-2.7.1/libdoc/optparse/rdoc/OptionParser.html).

That's it for today! Thank you for reading and happy coding!