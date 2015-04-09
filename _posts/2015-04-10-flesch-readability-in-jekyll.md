---
layout: post
title: "Testing Flesch-Kincaid Readability in Jekyll"
category: "Ruby"
tags: ruby flesch kincaid readability jekyll blog
---

### Introduction

One of the things I loved most about the Yoast SEO plugin for WordPress was the score it would give me on the readability of my content. This easily allowed me to adapt to the target audiences reading abilities. When I migrated to Jekyll, I lost a lot of the SEO benefits, so I took a stab at implementing it back in.

I did some quick Googling and stumbled upon the `Lingua` gem. This little treasure has a lot of features, one of which allows you to calculate the Flesch-Kincaid Readability of text. To add this functionality to Jekyll, I created a simple `Rake` task to let me know if a post needed to be reworded.

## Keeping It Flesch With Lingua

There is a gem that exists that handles a lot of this functionality called `lingua`. So let's get that added to your list of dependencies.

In your `Gemfile`, add:

    gem 'lingua'

Once it's added, run `bundle install`. 

If you are not using bundler, you can simple install with `gem`:

    gem install 'lingua'

One you've grabbed your dependencies, it's time to update your projects `Rakefile`.


## Creating Your Rakefile Task

Below is a quick proof of concept I threw together, which will grab all your posts in `_posts/` and calculate the Flesch reading score. I took the liberty of translating the scores from their numerical ranking to whom has that ability to read. This ranges between everyone and a PhD student.

In your `Rakefile`, add the following:


```ruby
require 'lingua'

task :readability do
  
  # Create a simple function to do this
  def calculate_score(text)
    # Remove the frontmatter, and codeblocks, because this will impact the score
    frontmatter = /(---)((?:(?:\r?\n)+(?:\w|\s).*)+\r?\n)(?=---\r?\n)(.*?)/x
    fenced_code = /`{3}(?:(.*$)\n)?([\s\S]*)`{3}/mx
    nested_code = /((?:^(?:[ ]{4}|\t).*$(?:\r?\n|\z))+)/x

    parsed = text.gsub(frontmatter, "#{$3}").gsub(fenced_code, "").gsub(nested_code, "")
    score = Lingua::EN::Readability.new( parsed ).flesch
    if score > 100
      return score, "an Elementary Schooler"
    elsif score.between?(80,100)
      return score, "a Middle Schooler"
    elsif score.between?(50,80)
      return score, "a High Schooler"
    elsif score.between?(30,50)
      return score, "an Average Adult"
    elsif score.between?(0,30)
      return score, "a College Level Student"
    else
      return score, "a PhD Academic"
    end
  end

  # Get all posts in ./_posts
  Find.find("./_posts/") do |post|
    if File.file?(post)
      score, level = calculate_score(File.read(post))
      puts "#{post} has a score of #{score}, which is suitable for #{level}"
    end
  end
end
```

To run this new task, you can use the following command:

    rake readability

Here is a small snippet after I ran it on my `_posts/` directory

    FAILED: ./_posts/2012-02-14-dark-dreamweaver-theme--aurza.md has a score of -14.631031353135285, which is suitable for a PhD Academic
    FAILED: ./_posts/2012-02-14-dreamweaver-theme-generator.md has a score of 4.533732394366211, which is suitable for a College Level Student
    FAILED: ./_posts/2012-09-05-null-pointer-exception-when-calling-getwritabledatabase.md has a score of 10.74755555555555, which is suitable for a College Level Student
    FAILED: ./_posts/2012-11-09-regular-expression-pattern-parsing-ifconfig.md has a score of 7.8300000000000125, which is suitable for a College Level Student
    FAILED: ./_posts/2013-12-29-creating-bootable-installer-for-osx-mavericks.md has a score of -11.032681159420264, which is suitable for a PhD Academic
    FAILED: ./_posts/2014-05-19-barnyard2-mysql-alert-view-for-snort.md has a score of 91.8459941520468, which is suitable for a Middle Schooler

Awesome, but that's a little difficult to read.


## Taking It A Step Further

This output is cool, we just calculated our Flesch score. In addition to it not being visually pleasing, it does not fit into a simple work flow, such as `rake test` or `jekyll build`. Since I use continuous integration to build and deploy my blog, I wanted to have it fail if the reading level was too low, or too high. My old build script was simply `jekyll build`. 

The following `rake` task can be used to cause the `jekyll build` to fail using the shell `&&` operator:

```ruby
task :readability do
  
  # Create a simple function to do this
  def calculate_score(text)
    # Remove the frontmatter, and codeblocks, because this will impact the score
    frontmatter = /(---)((?:(?:\r?\n)+(?:\w|\s).*)+\r?\n)(?=---\r?\n)(.*?)/x
    fenced_code = /`{3}(?:(.*$)\n)?([\s\S]*)`{3}/mx
    nested_code = /((?:^(?:[ ]{4}|\t).*$(?:\r?\n|\z))+)/x

    parsed = text.gsub(frontmatter, "#{$3}").gsub(fenced_code, "").gsub(nested_code, "")
    score = Lingua::EN::Readability.new( parsed ).flesch
    if score > 100
      return score, "an Elementary Schooler"
    elsif score.between?(80,100)
      return score, "a Middle Schooler"
    elsif score.between?(50,80)
      return score, "a High Schooler"
    elsif score.between?(30,50)
      return score, "an Average Adult"
    elsif score.between?(0,30)
      return score, "a College Level Student"
    else
      return score, "a PhD Academic"
    end
  end

  # Get all posts in ./_posts
  Find.find("./_posts/") do |post|
    if File.file?(post)
      score, level = calculate_score(File.read(post))
      if score > 80 || score < 15
        puts "FAILED: #{post} has a score of #{score}, which is suitable for #{level}"
        
        # Fail the build
        exit 1
      end
    end
  end
end
```

And then run it:

    rake readability && jekyll build
  

My updated build script now returns this:

    FAILED: ./_posts/2012-01-10-php-539-released.md has a score of 81.727311827957, which is suitable for a Middle Schooler

Once the command fails, you'll know which post fails, and which direction you need to reword the document. If `rake readability` succeeds, it will build my Jekyll site.

## Conclusion

By calculating the Flesch score of your content, you can better the experience for your audience by helping them understand what you're trying to say. Another benefit of using this score is to identify run-on and incomplete sentences since they greatly impact the score in either direction.

This post has a Flesch score of ``.
