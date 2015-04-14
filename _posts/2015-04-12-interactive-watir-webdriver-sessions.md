---
layout: post
title:  "Interactive Watir Webdriver Sessions"
date:   2015-04-12 23:35:04
categories: lambda
---

When creating integration tests with Watir Webdriver, I have found loading up Watir Webdriver in an interactive REPL session and interacting with the application under test to be huge productivity booster. Save this bad boy somewhere on your `PATH` (perhaps name it "webdriver") and start saving yourself some time and headache by writing your integration tests interactively:

{% highlight ruby %}
#!/bin/env ruby

# The following gems must be installed:
# - watir-webdriver
# - pry

require 'optparse'
require 'watir-webdriver'
require 'pry'

options = {
  :browser => :firefox,
  :url => 'https://www.google.com/'
}

parser = OptionParser.new do |opts|
  opts.on('-h', '--help', 'Displays this help text.') do
    puts opts
    exit
  end

  opts.on('-b', '--browser [BROWSER]', 'Sets browser to use, defaults to Firefox.') do |browser|
    options[:browser] = browser.to_sym
  end

  opts.on('-u', '--url [URL]', 'Sets page to visit on launch, defaults to Extranet Dev.') do |url|
    options[:url] = url
  end
end

parser.parse!

@browser = Watir::Browser.new(options[:browser])
@browser.driver.manage.window.maximize
@browser.goto(options[:url])

# Start interacting with the application (type `exit` to end session)
binding.pry

@browser.close
{% endhighlight %}
