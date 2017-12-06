---
layout: post
title:      "How I used Flash for error messages in Sinatra"
date:       2017-12-06 18:00:46 +0000
permalink:  how_i_used_flash_for_error_messages_in_sinatra
---


I struggled to find a way to alert my user when they tried something that my App doesn't permit. I knew that I could use Flash, but didn't know how.

On my first attempt I followed the example in the Playlister lab to a letter. I added rack-flash3 to Gemfile then added the folowing code:

At the top of my controller I added the requirment and use:

```
require 'rack-flash'

class HouseController < ApplicationController
  use Rack::Flash
```

Then added the Flash message in my delete action to let users know that they can't delete the Primary house:

```
flash[:message] = "*** CAN'T DELETE PRIMARY ***"
```

Finally I put a condition statement in show.erb to display the message if a user tried to delete the Primary house:

```
<% if flash.has?(:message) %>
  <%= flash[:message] %>
<% end %>
```

This all worked pretty well, but I ran into a few wrinkles. I needed another error message so I dropped the Flash condition in again, placed the Flash message in the right place, and set up the appropriate controller. Turns out I was asleep at the wheel because now my error message appeared in two different places. 

What I needed to do was to change the Flash message - yeah I know, should have seen that a mile away. So I woke up and reworked the Falsh message variable.

In my items/new post action I put:

```
flash[:room_error] = "*** FIRST CREATE A ROOM ***"
```

then in show.erb:

```
<%= flash[:room_error] %>
```

Now my messages were in the right place an unique. Almost perfect.

They were not prominent enough though. I needed to style them to make them more noticable.
I put a div with an id around them and added styling to my css file. Nothing happened. So I tried inline:

```
<div>
    <% if flash.has?(:room_error) %>
      <h3 style="color:red;"><%= flash[:room_error] %></h3>
    <% end %>
    </div>
```
and it worked!

I still don't know why the styling in the css file didn't work. If anyone reads this and knows why please let me know.
For now my error messages are working and almost impossible to miss.


