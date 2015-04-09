require "rubygems"
require 'rake'
require 'yaml'
require 'time'
require 'find'
require 'lingua'

SOURCE = "."
CONFIG = {
  'version' => "0.7.0",
  'layouts' => File.join(SOURCE, "_layouts"),
  'posts' => File.join(SOURCE, "_drafts"),
  'post_ext' => "md",
}

task :readability do

  # Create a simple function to do this
  def calculate_score(text)

    # Remove the frontmatter, and codeblocks, because this will impact the score
    #frontmatter = /(---)((?:(?:\r?\n)+(?:\w|\s).*)+\r?\n)(?=---\r?\n)(.*?)/x
    #fenced_code = /`{3}(?:(.*$)\n)?([\s\S]*)`{3}/mx
    #nested_code = /((?:^(?:[ ]{4}|\t).*$(?:\r?\n|\z))+)/x

    #parsed = text.gsub!(frontmatter, "#{$3}")
    score = Lingua::EN::Readability.new( text ).flesch
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
      #if score > 85 || score < 15
        puts "#{post} has a score of #{score}, which is suitable for #{level}"
        #exit 1
      #end
    end
  end
end

task :publish do
  Find.find("./_drafts/") do |path|
    if path.include?(ENV['ROCK_ARG1'])
      puts "Publishing: #{File.basename(path)}"
      %x[mv #{path} "./_posts/"]
      %x[git add _posts/ && git commit -m "publishing post #{File.basename(path)}" && git push]
      exit!
    end
  end

  puts "No matching posts found"

end

# Usage: rake post title="A Title" [date="2012-02-09"] [tags=[tag1, tag2]]
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
    post.puts "---\n\n"
    post.puts "### Introduction\n\n"
    post.puts "## Section Title\n\n"
    post.puts "## Section Title\n\n"
    post.puts "## Conclusion\n"
  end
end # task :post

# Usage: rake page name="about.html"
# You can also specify a sub-directory path.
# If you don't specify a file extention we create an index.html at the path specified
desc "Create a new page."
task :page do
  name = ENV["ROCK_ARG1"] || "new-page.md"
  filename = File.join(SOURCE, "#{name}")
  filename = File.join(filename, "index.html") if File.extname(filename) == ""
  title = File.basename(filename, File.extname(filename)).gsub(/[\W\_]/, " ").gsub(/\b\w/){$&.upcase}
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  
  mkdir_p File.dirname(filename)
  puts "Creating new page: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: page"
    post.puts "title: \"#{title}\""
    post.puts 'description: ""'
    post.puts "---"
  end
end # task :page

def ask(message, valid_options)
  if valid_options
    answer = get_stdin("#{message} #{valid_options.to_s.gsub(/"/, '').gsub(/, /,'/')} ") while !valid_options.include?(answer)
  else
    answer = get_stdin(message)
  end
  answer
end

def get_stdin(message)
  print message
  STDIN.gets.chomp
end

#Load custom rake scripts
Dir['_rake/*.rake'].each { |r| load r }
