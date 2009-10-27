require "rubygems"
require "domain"
require "rubyredis"
gem "lorem", :version => ">=0.1.2"
require "lorem"
require "digest/md5"

def redis
  $redis ||= RedisClient.new(:timeout => nil)
end

namespace :redis do
  desc "Seed Redis database"
  task :seed do
    
    # Number of users
    users = ENV["users"].to_i || 100
    
    # Number of posts per user
    posts = ENV["posts"].to_i || 200
    
    # Number of each users followers
    followers = ENV["followers"].to_i || 10
    
    users_data = []
    
    1.upto users do |i|
      user = User.create "user#{i}", "user#{i}"
      1.upto posts do |j|
        Post.create user, Lorem::Base.new('chars', 100).output
      end
      users_data << user
    end

    users_data.each do |user|
      followers_data = (users_data.reject { |u| u.id == user.id }).sort_by { rand }
      1.upto followers do
        user.follow followers_data.shift
      end
    end
  end
end