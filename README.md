[![Build Status](https://secure.travis-ci.org/samuelkadolph/activerecord-retry.png?branch=master)](http://travis-ci.org/samuelkadolph/activerecord-retry)
[![Gem Version](https://badge.fury.io/rb/activerecord-retry.png)](http://badge.fury.io/rb/activerecord-retry)
[![Dependency Status](https://gemnasium.com/samuelkadolph/activerecord-retry.png)](https://gemnasium.com/samuelkadolph/activerecord-retry)
[![Code Climate](https://codeclimate.com/github/samuelkadolph/activerecord-retry.png)](https://codeclimate.com/github/samuelkadolph/activerecord-retry)

# activerecord-retry

activerecord-retry adds query retrying to `ActiveRecord` for reads and transactions on specific errors.

## Description

Retries reads and transactions when an `ActiveRecord::StatementInvalid` error occurs that matches a list of errors. Default list
of errors includes errors related to failover situations to allow for graceful failovers during a request and to attempt
prevention of data loss for temporary issue.

## Installation

If you're using rails, add this line your application's Gemfile:

```ruby
gem "activerecord-retry", require: "active_record/retry/rails"
```

Otherwise add this line to your application's Gemfile:

```ruby
gem "activerecord-retry"
```

And then execute:

```
$ bundle install
```

Or install it yourself as:

```
$ gem install activerecord-retry
```

## Usage

```ruby
require "active_record/base"
require "active_record/retry"

class User < ActiveRecord::Base
  include ActiveRecord::Retry
  self.retries = [2, 4, 8, 16]
end

User.transaction do
  User.first.update_attributes(name: "Sam") # Oh noes, server went away but that's okay since we'll retry 4 times over 30 seconds
end
```

## Contributing

Fork, branch & pull request.
