#!/usr/bin/env ruby

require 'optparse'
require 'rubygems'
require 'redis'

options = { :conn => { :host => "localhost", :port => 6379, :password => nil, :timeout => 5 }, 
            :nagios => {}
          }

OptionParser.new do |opt|
  opt.banner = "Usage: #{$0} command <options>"

  opt.separator ""
  opt.separator "Nagios options:"

  opt.on("-w", "--warn WARN", Integer, "Nagios warning level") { |warn| options[:nagios][:warn] = warn }
  opt.on("-c", "--crit CRIT", Integer, "Nagios critical level") { |crit| options[:nagios][:crit] = crit }

  opt.separator ""
  opt.separator "Connection options:"

  opt.on('-H', '--host [HOSTNAME]', 'Hostname (Default: "localhost")') { |o| options[:conn][:host] = o if o }
  opt.on('-p', '--port [PORT]', 'Port (Default: "6379")') { |o| options[:conn][:port] = o if o }
  opt.on('-P', '--password [PASSWORD]', 'Password (Default: blank)') { |o| options[:conn][:password] = o if o }
  opt.on('-t', '--timeout [TIMEOUT]', Integer, 'Timeout in seconds (Default: 5)') { |o| options[:conn][:timeout] = o if o }

  opt.on_tail("-h", "--help", "Show this message") do
    puts opt
    exit 0
  end
end.parse!

command = ARGV[0]

class CheckRedis

  def initialize(opts, cmd)
    @redis = Redis.new(opts[:conn])
    gather_stats(opts[:nagios], cmd)

  rescue Errno::ECONNREFUSED => e
    puts e.message
    exit 2
  rescue Errno::ENETUNREACH => e
    puts e.message
    exit 2
  rescue Errno::EHOSTUNREACH => e
    puts e.message
    exit 2
  rescue Errno::EACCES => e
    puts e.message
    exit 2
  end

  private

  def gather_stats(options, cmd)
    stats = @redis.info
    if stats[cmd]
      value = stats[cmd].to_i

      if value >= options[:crit]
        puts "CRIT: #{cmd} exceeds critical level of #{options[:crit]} : #{value} | #{cmd}=#{value};#{options[:warn]};#{options[:crit]};;"
        exit 2
      elsif value >= options[:warn]
        puts "WARN: #{cmd} exceeds warning level of #{options[:warn]} : #{value} | #{cmd}=#{value};#{options[:warn]};#{options[:crit]};;"
        exit 1
      else
        puts "OK: #{cmd} is ok | #{cmd}=#{value};#{options[:warn]};#{options[:crit]};;"
        exit 0
      end 

      # TODO: redis.info has some human_values

    else
      puts "UNKNOWN: No such key - #{cmd}"
      exit 3
    end

  end

end

CheckRedis.new(options, command)