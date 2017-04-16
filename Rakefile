require 'rake'
require 'date'
require 'pry'

desc 'generates a new post'
task :new_post, [:title] do |t, args|
  date = Time.now

  if args[:title].nil?
    puts "Please enter a title:"
    title = STDIN.gets.chomp
  else
    title = args[:title]
  end
  formatted_title = title.downcase.gsub(' ', '-').gsub(/[^\w-]/, '')

  post_title = "#{date.strftime('%Y-%m-%d')}-#{formatted_title}.markdown"
  contents = <<-STR 
---
layout: post
title: "#{title}"
date: #{date.to_s} 
comments: true
categories: 
---
  STR

  File.open("_posts/#{post_title}", "w") do |f|
    f.write(contents)
  end
  puts "New post: #{title} generated"
end
