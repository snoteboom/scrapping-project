---
  tags: object orientation, oo, object-oriented, kids
  languages: ruby
---

##OBJECT ORIENTED SCRAPING

The next step in building your web application is to turn your scraping script into an object-oriented program.

We're going to use the same code you wrote to originally scrape the data points, but this time take that collection of data you made and turn it into an object.

So first thing's first, you'll need to create a class. You'll want the name of your class to reflect the type of object it is. `class ScrapedStuff` isn't explicit at all. Another developer in Germany who wants to submit a pull request and work on your open sourced application is not going to have any idea what that is and probably won't want to help. Explicit names are super important.

You'll want to think about the different instances variables you'll need. Essentially, you'll want to create instances variables for all the values in your hash when you originally scraped your data. 

Each of those calls to scrape specific data points should get encapsulated into different methods.
For example, if we were scraping my twitter account:

```RUBY
doc = Nokogiri::HTML(open('https://twitter.com/vicfriedman'))
twitter_pic = doc.css('div.profile-header-inner a img').attribute("src").value
```

We would want to encapsulate the logic to get my twitter picture into a method:
```RUBY
def get_twitter_picture
  twitter_pic = doc.css('div.profile-header-inner a img').attribute("src").value
end
```

You'll want to design your initialize method to take in only the website that you'd like to open. Imagine trying to pass it every data point for every attribute. I want to only pass the website (for this example that would be "https://www.twitter.com/vicfriedman"). Once I'm inside the initialize method, I can call various other methods to define my other instance variables.

My initialize method would look something like this:

```RUBY
def initialize(doc)
@doc = Nokogiri::HTML(open("#{doc}"))
@twitter_pic = self.get_twitter_picture
end
```

The `self.get_twitter_picture` can be confusing. Methods written in your classes can only be called on instances of your class. They're called instance methods. When we're inside that method, we use `self` to refer to the current object. `self` is a reference to that specific object. It's a confusing concept, but it's a reference to whatever object you're manipulating at a given moment in time. Throughout the course of your applications, `self` will change, so it's important to always be thinking about what object you're playing with.

In my initialize method, I'm using `self` to refer to the new instance of my class that I'm currently creating. If I just tried to call `get_twitter_picture` I would get an error:  `NameError: undefined local variable or method `get_twitter_pic' for main:Object`

You'll also have to remember to write reader and writer methods for your instance variables. 
```RUBY
def twitter_picture
  @twitter_pic
end

def twitter_picture=(twitter_pic)
  @twitter_pic = twitter_pic
end
```

If you're interested in taking readers and writers a step further, you can take a look into ruby convience methods `attr_writer`, `attr_reader` and `attr_accessor`. [Here](http://stackoverflow.com/questions/4370960/what-is-attr-accessor-in-ruby) is a good Stackoverflow post that explains their usage. A reader and writer method work great too.


