# Zammad API Client (Ruby)

## API version support
This client supports Zammad version 1.0 API.

## Installation

The Zammad API client can be installed using Rubygems or Bundler.

```ruby
gem install zammad_api
```

or add the following to your Gemfile

```ruby
gem 'zammad_api'
```

## Available objects

* user
* organization
* group
* ticket
* ticket_state
* ticket_priority

## Usage

### create instanze

```ruby
client = ZammadAPI::Client.new(
  url: 'http://localhost:3000/',
  user: 'user',
  password: 'some_pass'
)
```

## Resource management

Individual resources can be created, modified, saved, and destroyed.

### create object

with new and save
```ruby
group = client.group.new(
  name: 'Support',
  note: 'Some note',
);
group.save

group.id # id of record
group.name # 'Support'
```

with create
```ruby
group = client.group.create(
  name: 'Support',
  note: 'Some note',
);

group.id # id of record
group.name # 'Support'
```

### fetch object

```ruby
group = client.group.find(123)
puts group.inspect
```
### update object

```ruby
group = client.group.find(123)
group.name = 'Support 2'
group.save
```

### destroy object

```ruby
group = client.group.find(123)
group.destroy
```

## Collection management

A list of individual resources.

### all

```ruby
groups = client.group.all

group1 = groups[0]
group1.note = 'Some note'
group1.save

groups.each {|group|
  p "group: #{group.name}"
}
```

### search
```ruby
groups = client.group.search(query: 'some name')

group1 = groups[0]
group1.note = 'Some note'
group1.save

groups.each {|group|
  p "group: #{group.name}"
}
```

### all with pagination (beta)

```ruby
groups = client.group.all

groups.page(1,3) {|group|
  p "group: #{group.name}"

  group.note = 'Some new note, inclued in page 1 with 3 per page'
  group.save
}

groups.page(2,3) {|group|
  p "group: #{group.name}"

  group.note = 'Some new note, inclued in page 2 with 3 per page'
  group.save
}
```

### search with pagination (beta)
```ruby
groups = client.group.search(query: 'some name')

groups.page(1,3) {|group|
  p "group: #{group.name}"

  group.note = 'Some new note, inclued in page 1 with 3 per page'
  group.save
}

groups.page(2,3) {|group|
  p "group: #{group.name}"

  group.note = 'Some new note, inclued in page 2 with 3 per page'
  group.save
}
```

## Examples

create ticket
```ruby
ticket = client.ticket.create(
  title: 'a new ticket #1',
  state: 'new',
  priority: '3 normal',
  article: {
    content_type: 'text/plain', # or text/html
    body: 'some body'
  }
)

ticket.id # id of record
ticket.number # 'Support'
```

list of all new or open
```ruby
tickets = client.ticket.search(query: 'state:new OR state:open')

ticket[0].id # id of record
ticket[0].number # 'Support'

tickets.each {|ticket|
  p "ticket: #{ticket.number} - #{ticket.number}"
}
```