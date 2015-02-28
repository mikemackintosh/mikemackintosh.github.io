# Mike Mackintosh

[![https://travis-ci.org/mikemackintosh/mikemackintosh.github.io.svg](https://travis-ci.org/mikemackintosh/mikemackintosh.github.io.svg)](https://travis-ci.org/mikemackintosh/mikemackintosh.github.io)

View the site at http://www.mikemackintosh.com

# Publish A Page
Using this sick little system, you can easily create a new post:

```shell
rock post "This is my posts title"

````

### Create Post Task Source from `Rakefile`

```ruby
desc "Begin a new post in #{CONFIG['posts']}"
task :post do
  abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])
  title = ENV['ROCK_ARG1'] || "new-post"
  tags = ENV["tags"] || ""
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
  rescue => e
    puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
    exit -1
  end
  filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/-/,' ')}\""
    post.puts "category: derp"
    post.puts "tags: #{tags}"
    post.puts "---"
  end
end # task :post
```

# Publishing A Post

To publish a post, I can simply run: `rock publish <keyword>`

### Publish Task Source from `Rakefile`

```ruby
task :publish do
  Find.find("./_drafts/") do |path|
    if path.include?(ENV['ROCK_ARG1'])
      puts "Publishing: #{File.basename(path)}"
      %x[mv #{path} "./_posts/"]
      %x[git add #{path} && git commit -m "publishing post #{File.basename(path)} && git push"]
      exit!
    end
  end

  puts "No matching posts found"

end
```