require 'rubygems'
require 'bundler'
require 'time'
Bundler.require

require './devdigest'

task :run do
  since    = Time.now-24*60*60
  digest   = Devdigest.run(since)
  markdown = RDiscount.new(digest)
  subject  = "Team digest - #{Time.now.strftime("%A")}"

  Pony.mail({
    :to      => ENV["EMAIL_TO"],
    :from    => ENV["EMAIL_FROM"],
    :subject => subject,
    :headers => { "Content-Type" => "text/html" },
    :body    => markdown.to_html,

    :via => :smtp,
    :via_options => {
      :address        => ENV["MAILGUN_SMTP_SERVER"],
      :port           => ENV["MAILGUN_SMTP_PORT"],
      :user_name      => ENV["MAILGUN_SMTP_LOGIN"],
      :password       => ENV["MAILGUN_SMTP_PASSWORD"],
      :authentication => :plain,
      :domain         => "heroku.com"
    }
  })

  puts "Emailed #{ENV["EMAIL_TO"]}."
end