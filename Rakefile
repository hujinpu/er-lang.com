require "rubygems"
require "bundler"
Bundler.setup

## TABLE OF CONTNETS
# PREVIEW
# GENERATE
# WATCH
# DEPLOY
# MISC

## PREVIEW
desc "preview the site in a web browser"
task :preview => [:generate, :start_serve, :watch]

## GENERATE
desc "Generate site files only"
task :generate_site do
  puts "\n\n>>> Generating site files <<<"
  system "jekyll --pygments"
end

desc "Generate styles only"
task :generate_style do
  puts ">>> Generating styles <<<"
  system "juicer merge --force _site/stylesheet/master.css"
end

desc "Generate js only"
task :generate_js do
  puts ">>> Generating js <<<"
  system "juicer merge -i --force _site/js/master.js"
end

desc "generate website in output directory"
task :generate => [:clean, :generate_site, :generate_style, :generate_js] do
  puts "Site Generating Complete!\n\nRefresh your browser"
end

## WATCH
def rebuild_site(relative)
  puts "\n\n>>> Change Detected to: #{relative} <<<"
  IO.popen('rake generate_site'){|io| print(io.readpartial(512)) until io.eof?}
  puts 'Update Complete'
end

desc "Watch change and regenerate when it changes"
task :watch do
  require 'fssm'
  puts "Change Detected"
  FSSM.monitor do
    path "#{File.dirname(__FILE__)}/" do
      update {|base, relative| rebuild_site(relative)}
      delete {|base, relative| rebuild_site(relative)}
      create {|base, relative| rebuild_site(relative)}
    end
  end
end

## Serve
desc "start up an instance of serve on the output files"
task :start_serve => :stop_serve do
  cd "_site" do
    print "Starting serve..."
    system("serve 4000 > /dev/null 2>&1 &")
    sleep 1
    pid = `ps auxw | awk '/bin\\/serve\\ 4000/ { print $2 }'`.strip
    ok_failed !pid.empty?
    system "open http://localhost:4000" unless pid.empty?
  end
end

desc "stop all instances of serve"
task :stop_serve do
  pid = `ps auxw | awk '/bin\\/serve\\ 4000/ { print $2 }'`.strip
  if pid.empty?
    puts "Serve is not running"
  else
    print "Stoping serve..."
    ok_failed system("kill -9 #{pid}")
  end
end

## DEPLOY
desc "Deploy Amazon s3 Using s3Sync"
task :deploy do
  system('s3sync -rpv _site/ er-lang.com:')
end

## MISC
desc "Given a title and category as arguments, create a new post file"
task :write, [:title, :category] do |t, args|
  filename = "#{Time.now.strftime('%Y-%m-%d')}-#{args.title.gsub(/\s/, '_').downcase}.markdown"
  path = File.join("_posts", filename)
  if File.exist? path; raise RuntimeError.new("Won't clobber #{path}"); end
  File.open(path, 'w') do |file|
    file.write <<-EOS
---
layout: post
category: #{args.category}
title: #{args.title}
date: #{Time.now.strftime('%Y-%m-%d %k:%M:%S')}
---
EOS
    end
    # puts "Now open #{path} in an editor."
    `mate #{path}`
end

desc "remove files in output directory"
task :clean do
  puts ">>> Removing output <<<"
  Dir["_site/*"].each { |f| rm_rf(f) }
end

def ok_failed(condition)
  if (condition)
    puts "OK"
  else
    puts "FAILED"
  end
end
