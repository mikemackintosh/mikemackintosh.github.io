---
layout: post
title: DamnSimpleWhitelist for Ruby
tags: ip network ruby whitelist blacklist
category: networking
---

This class was created to be exactly what it's called, damn simple. 

You can pass it whitelists, in the `WHITELIST_IPS` constant (which is an array), or in a file, `ENV['WHITELIST_FILE']` (and it defaults to `/etc/whitelist`).

**Note:** It does require a 3rd-party library, `ipaddress`. 

## Install 
`gem install ipaddress`

## damnsimplewhitelist.rb

    require 'ipaddress'

    # Create Whitelist
    class DamnSimpleWhitelist
      WHITELIST_FILE = ENV['WHITELIST_FILE'] || '/etc/whitelist'
      WHITELIST_IPS = ['10.0.0.0/8']
      @@ips = Array.new

      def initialize()
        @@ips = WHITELIST_IPS.inject(Array.new) {|ips,ip| @@ips.push(IPAddress ip) }
        if File.exists?(WHITELIST_FILE)
          @@ips.push(*parse_whitelist(WHITELIST_FILE))
        end
      end

      def all
        @@ips
      end

      def has?(ipaddr)
        @@ips.any? {|ip| ip.include?(ipaddr) }
      end

      private
      
      def parse_whitelist(whitelist)
      
        whitelisted = Array.new
        File.readlines(whitelist).each do |line|
          line.chomp!
          begin
            whitelisted.push(IPAddress line)
          rescue
          end
        end

        whitelisted
      
      end

    end

## Usage:

It's really simple to use:

    w = DamnSimpleWhitelist.new
    if w.has? 10.1.1.1
        # Allowed!
    end

If you want to get all whitelisted IP addresses, you can use the `#all` method. 
