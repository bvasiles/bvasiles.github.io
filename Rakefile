#############################################################################
#
# Modified version of jekyllrb Rakefile
# https://github.com/jekyll/jekyll/blob/master/Rakefile
#
#############################################################################

require 'rake'
require 'date'
require 'yaml'
require 'rake-jekyll'

Rake::Jekyll::GitDeployTask.new(:deploy) do |t|

  # Description of the rake task.
  t.description = 'Generate the site and push changes to remote repository'

  # Overrides the *author* of the commit being created with author of the
  # source commit (i.e. HEAD in the current branch).
  t.author = -> {
    `git log -n 1 --format='%aN <%aE>'`.strip
  }
  # Overrides the *author date* of the commit being created with date of the
  # source commit.
  t.author_date = -> {
    `git log -n 1 --format='%aD'`.strip
  }
  # The commit message will contain hash of the source commit.
  t.commit_message = -> {
    "Built from #{`git rev-parse --short HEAD`.strip}"
  }
  # Use 'Jekyll' as the default *committer* name (with empty email) when the
  # user.name is not set in git config.
  t.committer = 'Jekyll'

  # Deploy the built site into remote branch named 'gh-pages', or 'master' if
  # the remote repository URL matches `#{gh_user}.github.io.git`.
  # It will be automatically created if not exist yet.
  t.deploy_branch = -> {
    gh_user = ENV['TRAVIS_REPO_SLUG'].to_s.split('/').first
    remote_url.match(/[:\/]#{gh_user}\.github\.io\.git$/) ? 'master' : 'gh-pages'
  }
  # Run this command to build the site.
  t.build_script = ->(dest_dir) {
    puts "\nRunning Jekyll..."
    sh "bundle exec jekyll build --destination #{dest_dir}"
  }
  # Use the default committer (configured in git) when available.
  t.override_committer = false

  # Use URL of the 'origin' remote to fetch/push the built site into. If env.
  # variable GH_TOKEN is set, then it adds it as a userinfo to the URL.
  t.remote_url = -> {
    url = `git config remote.origin.url`.strip.gsub(/^git:/, 'https:')
    next url.gsub(%r{^https://([^/]+)/(.*)$}, 'git@\1:\2') if ssh_key_file?
    next url.gsub(%r{^https://}, "https://#{ENV['GH_TOKEN']}@") if ENV.key? 'GH_TOKEN'
    next url
  }
  # Skip commit and push when building a pull request, env. variable
  # SKIP_DEPLOY represents truthy, or env. variable SOURCE_BRANCH is set, but
  # does not match TRAVIS_BRANCH.
  t.skip_deploy = -> {
    ENV['TRAVIS_PULL_REQUEST'].to_i > 0 ||
      %w[yes y true 1].include?(ENV['SKIP_DEPLOY'].to_s.downcase) ||
      (ENV['SOURCE_BRANCH'] && ENV['SOURCE_BRANCH'] != ENV['TRAVIS_BRANCH'])
  }
  # Path of the private SSH key to be used for communication with the
  # repository defined by remote_url.
  t.ssh_key_file = '.deploy_key'
end

# CONFIG = YAML.load(File.read('_config.yml'))
# USERNAME = CONFIG["username"] || ENV['GIT_NAME']
# REPO = CONFIG["repo"] || "#{USERNAME}.github.io"
# 
# # Determine source and destination branch
# # User or organization: develop -> master
# # Project: master -> gh-pages
# # Name of source branch for user/organization defaults to "source"
# if REPO == "#{USERNAME}.github.io"
#   SOURCE_BRANCH = CONFIG['branch'] || "develop"
#   DESTINATION_BRANCH = "master"
# else
#   SOURCE_BRANCH = "master"
#   DESTINATION_BRANCH = "gh-pages"
# end
# 
# #############################################################################
# #
# # Helper functions
# #
# #############################################################################
# 
# def replace_header(head, header_name)
#   head.sub!(/(\.#{header_name}\s*= ').*'/) { "#{$1}#{send(header_name)}'"}
# end
# 
# def normalize_bullets(markdown)
#   markdown.gsub(/\s{2}\*{1}/, "-")
# end
# 
# def linkify_prs(markdown)
#   markdown.gsub(/#(\d+)/) do |word|
#     "[#{word}]({{ site.repository }}/issues/#{word.delete("#")})"
#   end
# end
# 
# def linkify_users(markdown)
#   markdown.gsub(/(@\w+)/) do |username|
#     "[#{username}](https://github.com/#{username.delete("@")})"
#   end
# end
# 
# def linkify(markdown)
#   linkify_users(linkify_prs(markdown))
# end
# 
# def liquid_escape(markdown)
#   markdown.gsub(/(`{[{%].+[}%]}`)/, "{% raw %}\\1{% endraw %}")
# end
# 
# def remove_head_from_history(markdown)
#   index = markdown =~ /^##\s+\d+\.\d+\.\d+/
#   markdown[index..-1]
# end
# 
# def converted_history(markdown)
#   remove_head_from_history(liquid_escape(linkify(normalize_bullets(markdown))))
# end
# 
# # File activesupport/lib/active_support/inflector/transliterate.rb, line 80
# def parameterize(string, sep = '-')
#   # replace accented chars with their ascii
#   # simplified from original to remove dependency
#   parameterized_string = string.dup.force_encoding('US-ASCII')
#   # Turn unwanted chars into the separator
#   # changed from original: allow A-Z
#   parameterized_string.gsub!(/[^a-zA-Z0-9\-_]+/, sep)
#   unless sep.nil? || sep.empty?
#     re_sep = Regexp.escape(sep)
#     # No more than one of the separator in a row.
#     parameterized_string.gsub!(/#{re_sep}{2,}/, sep)
#     # Remove leading/trailing separator.
#     parameterized_string.gsub!(/^#{re_sep}|#{re_sep}$/, '')
#   end
#   parameterized_string.downcase
# end
# 
# def check_destination
#   unless Dir.exist? CONFIG["destination"]
#     sh "git clone https://#{ENV['GIT_NAME']}:#{ENV['GH_TOKEN']}@github.com/#{USERNAME}/#{REPO}.git #{CONFIG["destination"]}"
#   end
# end
# 
# #############################################################################
# #
# # Post and page tasks
# #
# #############################################################################
# 
# namespace :post do
#   desc "Create a new post"
#   task :create do
#     title = ENV["title"] || "new-post"
#     begin
#       slug = parameterize(title)
#       puts slug
#     rescue => e
#       puts "Error: invalid characters in title"
#       exit -1
#     end
# 
#     begin
#       date = ENV['date'] ? Date.parse(ENV['date']) : Date.today
#     rescue => e
#       puts "Error: date format must be YYYY-MM-DD"
#       exit -1
#     end
# 
#     filename = File.join("_posts", "#{date}-#{slug}.md")
#     if File.exist?(filename)
#       puts "Error: post already exists"
#       exit -1
#     end
# 
#     header = { "layout" => "post", "title" => title }
#     content = header.to_yaml + "---\n"
# 
#     if IO.write(filename, content)
#       puts "Post #{filename} created"
#     else
#       puts "Error: #{filename} could not be written"
#     end
#   end
# end
# 
# namespace :page do
#   desc "Create a new page"
#   task :create do
#     title = ENV["title"] || "new-page"
#     begin
#       slug = parameterize(title)
#       puts slug
#     rescue => e
#       puts "Error: invalid characters in title"
#       exit -1
#     end
# 
#     folder = ENV["folder"] || "."
# 
#     filename = File.join(folder, "#{slug}.md")
#     if File.exist?(filename)
#       puts "Error: page already exists"
#       exit -1
#     end
# 
#     header = { "layout" => "page", "title" => title }
#     content = header.to_yaml + "---\n"
# 
#     if IO.write(filename, content)
#       puts "Page #{filename} created"
#     else
#       puts "Error: #{filename} could not be written"
#     end
#   end
# end
# 
# #############################################################################
# #
# # Site tasks
# #
# #############################################################################
# 
# namespace :site do
#   desc "Generate the site"
#   task :build do
#     check_destination
#     sh "bundle exec jekyll build"
#   end
# 
#   desc "Generate the site and serve locally"
#   task :serve do
#     check_destination
#     sh "bundle exec jekyll serve"
#   end
# 
#   desc "Generate the site, serve locally and watch for changes"
#   task :watch do
#     sh "bundle exec jekyll serve --watch"
#   end
# 
#   desc "Generate the site and push changes to remote origin"
#   task :deploy do
#     # Detect pull request
#     if ENV['TRAVIS_PULL_REQUEST'].to_s.to_i > 0
#       puts 'Pull request detected. Not proceeding with deploy.'
#       exit
#     end
# 
#     # Configure git if this is run in Travis CI
#     if ENV["TRAVIS"]
#       sh "git config --global user.name '#{ENV['GIT_NAME']}'"
#       sh "git config --global user.email '#{ENV['GIT_EMAIL']}'"
#       sh "git config --global push.default simple"
#     end
# 
#     # Make sure destination folder exists as git repo
#     #check_destination
#     sh "git clone https://#{ENV['GIT_NAME']}:#{ENV['GH_TOKEN']}@github.com/#{USERNAME}/#{REPO}.git"
# 
# 	sh "cd '#{REPO}'"
# 	#sh "echo 'git checkout #{SOURCE_BRANCH}'"
#     sh "git checkout #{SOURCE_BRANCH}"
# 
#     # Generate the site
#     sh "bundle exec jekyll build"
# 
# 	#sh "echo 'git fetch'"
# 	#sh "git fetch"
# 
#     #sh "echo 'git checkout #{DESTINATION_BRANCH}'"
#     Dir.chdir(CONFIG["destination"]) { sh "git checkout #{DESTINATION_BRANCH}" }
# 
# 
#     # Commit and push to github
#     sha = `git log`.match(/[a-z0-9]{40}/)[0]
#     Dir.chdir(CONFIG["destination"]) do
#       sh "git add --all ."
#       sh "git commit -m 'Updating to #{USERNAME}/#{REPO}@#{sha}.'"
#       sh "git push --quiet origin #{DESTINATION_BRANCH}"
#       puts "Pushed updated branch #{DESTINATION_BRANCH} to GitHub Pages"
#     end
#   end
# end