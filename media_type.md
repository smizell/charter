# Minimal Media Type

After talking with Mark and having the light bulb go off, I wanted to write down some thoughts on a simple media type that allows for building off of existing JSON blobs. I also wanted to add some ideas.

## Just adding Meta

This is where the the media type shines that Mark has been talking about, where adding `meta` anywhere makes it a link. This is super simple. I'm going to use Coffeescript since it's easier to type.

```coffeescript
firstName: 'John'
lastName: 'Doe'
self:
  meta:
    href: '/customers/1'
orders:
  meta:
    href: '/customers/1/orders'
addresses:
  meta:
    href: '/customers/1/addresses'
```

I think this is super helpful for Apiary's situation, where we can throw in hypermedia without messing up the current JSON blobs. I do have three concerns here.

First, as Mark has pointed out, it requires inspecting every element to see if it has `meta`. This is not a horrible thing, but it does requiring a lot inspecting.

Second and related to the first, it means that links can exist anywhere in the blob, which is kind of cool, but kind of scary at the same time, as you could get some really confusing messages.

```coffeescript
book:
  author:
    email: 'john@example.com'
    self:
      meta:
        href: '/author/1'
```

I think this borders on being a strength and a weakness! I can't decide if it's a lot of freedom, or a bad pattern.

Third (and kind of related), it mixes attributes and transitions together. This feels strange to me, and seems like it could be mixing too much together.

## Maybe add transitions, too?

So I had a thought–what if we added a special transitions object as well?

```coffeescript
firstName: 'John'
lastName: 'Doe'
transitions:
  self:
    meta:
      href: '/customers/1'
  orders:
    meta:
      href: '/customers/1/orders'
  addresses:
    meta:
      href: '/customers/1/addresses'
```

This addresses my concerns above, especially saving you from having to inspect everything for meta. It's really just a thought, so please think through pros and cons of this as well.

## Taking it further

Just thinking out loud. Not really wanting to chase too many rabbits, but did want to explore some things. I think we can cover just about all of the hypermedia elements here.

### Mutable attributes

After Mark and I talked about forms and how to deal with them, I had this thought of considering attributes of unsafe actions be considered mutable (meaning, you can change them and send them). If it's PUT, just send everything back minus the `meta` object.

```coffeescript
firstName: 'John'
lastName: 'Doe'
transitions:
  edit:
    meta:
      href: '/customers/1'
      method: 'PUT'
    firstName: 'John'
    lastName: 'Doe'
```

### Presentation

As Mark brought up, we can add a presentation object to `meta` add presentation information as well.

```coffeescript
firstName: 'John'
lastName: 'Doe'
transitions:
  edit:
    meta:
      href: '/customers/1'
      method: 'PUT'
      presentation:
        firstName:
          label: 'First Name'
        lastName:
          label: 'Last Name'
    firstName: 'John'
    lastName: 'Doe'
```

#### Bonus: LISP

I've been thinking about JSON and LISP-like syntax. Proceed here with caution :) This is just thinking out loud.

```coffeescript
firstName: 'John'
lastName: 'Doe'
transitions:
  edit:
    meta:
      href: '/customers/1'
      method: 'PUT'
      presentation:
        firstName: ['span', class: 'firstName', '~firstName']
        lastName: ['span', class: 'lastName', '~lastName']
    firstName: 'John'
    lastName: 'Doe'
```

### Queries

```coffeescript
firstName: 'John'
lastName: 'Doe'
transitions:
  searchOrders:
    meta:
      href: '/customers/1/orders'
      queryParams:
        month: ''
        year: ''
```

### Embedded Transitions

The transition should always be assumed to be a partial representation unless it is specified it is a fully embedded transition.

```coffeescript
order_count: 25
transitions:
  order: [
    meta:
      href: '/order/4'
      embedded: true
    totaL_amount: 100.00
  ]
```

### Attributes as transitions

Yikes... but, just a thought?

```coffeescript
meta:
  semantics:
    homepage:
      type: 'transition'
      meta:
        encType: ['application/json']
name: 'John Doe'
email: 'john@example.com'
homepage: 'http://example.com'
```

or

```coffeescript
meta:
  semantics:
    avatar:
      type: 'transclude-image'
name: 'John Doe'
email: 'john@example.com'
avatar: 'http://example.com/john.jpg'
```
