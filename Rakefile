require "rubygems"
require 'rake'
require 'yaml'
require 'time'
require 'fileutils'

SOURCE = "."
CONFIG = {
  'version' => "12.3.2",
  'themes' => File.join(SOURCE, "_includes", "themes"),
  'layouts' => File.join(SOURCE, "_layouts"),
  'posts' => File.join(SOURCE, "_posts"),
  'post_ext' => "md",
  'theme_package_version' => "0.1.0"
}

desc "Begin a new post in #{CONFIG['posts']}"
task :post do
  abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])
  title = ENV["title"] || "new-post"
  subtitle = ENV["subtitle"] || "This is a subtitle"
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')

  begin
    # Parse date with time handling
    raw_date = ENV['date'] ? Time.parse(ENV['date']) : Time.now
    date_str = raw_date.strftime('%Y-%m-%d-%H-%M')
    year_month = raw_date.strftime('%Y/%m')
  rescue ArgumentError => e
    puts "Error - Invalid date format: #{e.message}"
    puts "Please use format: YYYY-MM-DD [HH:MM]"
    exit -1
  end

  post_dir = File.join(CONFIG['posts'], year_month)
  FileUtils.mkdir_p(post_dir) unless Dir.exist?(post_dir)

  filename = File.join(post_dir, "#{date_str}.#{CONFIG['post_ext']}")

  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end

  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/-/,' ')}\""
    post.puts "subtitle: \"#{subtitle.gsub(/-/,' ')}\""
    post.puts "date: #{raw_date.iso8601}"  # ISO 8601 format with timezone
    post.puts "author: \"Hux\""
    post.puts "header-img: \"img/post-bg-2015.jpg\""
    post.puts "tags: []"
    post.puts "---"
  end
end
