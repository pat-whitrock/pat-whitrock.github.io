---
layout: post
title: "Customizing ActionView"
subtitle: "How to prevent gross typos once and for all"
date: 2014-03-12 23:53:03 -0400
comments: true
categories: 
---

The ActionView module is an awesome part of the Rails framework. It exists to handle template lookup and rendering and provides several helpers allowing you to minimize the back-and-forth between Ruby and HTML in your ERB templates.

## ActionView::Helpers

If you’re developing a Rails application, you’ll most likely be making use of ActionView helpers within your templates. Rails will also be utilizing these helpers to create associations between different parts of your application based solely on naming conventions. ActionView is largely responsible for the power of the Rails framework that is provided by prioritizing convention over customization.

### Pluralization is awesome

ActionView has some awesome methods, but pluralize is probably the most common and immediately useful helpers. It does precisely what it seems — it provides the plural version of a given word.

```ruby
"Dunder Mifflin has #{pluralize(4, "employee")}."
	=> Dunder Mifflin has 4 employees.
"Michael Scott Paper Company has #{pluralize(1, "employee")}."
	=> Michael Scott Paper Company has 1 employee.
```

Pluralize accounts for the dynamic nature of data-driven applications. The code above will use the correct version of a given word, without having to fill our templates with sloppy, repetitive code (hooray for being DRY). Without the pluralize helper, every time we use a dynamic number, we would be forced to write some code like this:

```ruby
<p>Dundler Mifflin has <%= @employees.count %> <%=
  if @employees.count == 1
    "employee"
  else
    "employees"
  end
%></p>
```

ActionView also does a great job of handling common irregular pluralizations. For example:

```ruby
"#{pluralize(3, "woman")} work at Dundler Mifflin."
	=> "3 women work at Dundler Mifflin."
```
How convenient is that!? ActionView handles a very large number of irregular pluralizations, singularizations, even uncountables:

```ruby
"movies".singularize
	=> "movie"    # not "movy"
"species".pluralize
	=> "species"  # not "specieses"
```

### But sometimes it breaks

Unfortunately, there are some edge cases — untracked irregular pluralizations and words with multiple acceptable pluralizations, which often rely on context or personal preference. When I noticed that some common pluralizations, weren’t being caught by pluralize, I had the same thought I’m sure many programming students have had:

> Submit a pull request to Rails!

But first, I have to find which file I would find this relationship in, so I can actually make the change. And [here](https://github.com/rails/rails/blob/92f567ab30f240a1de152061a6eee76ca6c4da86/activesupport/lib/active_support/inflections.rb) it is. Turns out, the inflection rules have been frozen to prevent applications that rely on relationships won’t break.

<iframe width="770" height="480" src="//www.youtube.com/embed/WWaLxFIVX1s" frameborder="0" allowfullscreen></iframe>

### Luckily we can fix it

For example, ActionView incorrectly assumes *human* should be pluralized to *humen*. We can override this using the [irregular](https://github.com/rails/rails/blob/4e327225947b933d5434509e02e98226c581adc1/activesupport/lib/active_support/inflector/inflections.rb#L128) method:

```ruby
"human".pluralize
	=> "humen"

ActiveSupport::Inflector.inflections do |inflect|
  inflect.irregular "human", "humans"
end

"human".pluralize
 => "humans"
```

If you’re using a word with identical singular and plural spelling — or if it has pluralizations for both a collection and multiple instances and you would prefer to use the latter — you should make use of the [uncountable](https://github.com/rails/rails/blob/4e327225947b933d5434509e02e98226c581adc1/activesupport/lib/active_support/inflector/inflections.rb#L162) method:

```ruby
"candy".pluralize
 => "candies"
 
"gossip".pluralize
=> "gossips"

ActiveSupport::Inflector.inflections do |inflect|
 inflect.uncountable "candy", "gossip"
end

"candy".pluralize
 => "candy"

"gossip".pluralize
=> "gossip"
```

Unlike the `plural` method, which creates a link between the singular and plural versions of a word, `uncountable` actually adds the word (or words if multiple arguments are passed in) to an `@uncountables` array, which contains each word ActionView will consider to have equivalent singular and equal spellings. The array begins with [10 words](https://github.com/rails/rails/blob/92f567ab30f240a1de152061a6eee76ca6c4da86/activesupport/lib/active_support/inflections.rb#L68), but you can add to it as you see fit.

```ruby
@uncountables = ["equipment", "information", "rice", "money", "species", "series", "fish", "sheep", "jeans", "police"]
```

### When is customization worth it?

If you find ActionView doesn’t properly singularize or pluralize a commonly used word within your application, it is definitely in your best interest to manually create its singular and plural states. If something within your domain model that will warrant its own model, table, views and controller isn’t being properly handled by ActionView, you should manually amend ActionView to perceive the relationship as you see fit. This will allow you to stay DRY and utilize the full capacity of ActiveRecord associations.

Happy pluralizing.