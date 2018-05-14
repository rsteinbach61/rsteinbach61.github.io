---
layout: post
title:      "Using an API in my Rails Project "
date:       2018-05-14 21:37:52 +0000
permalink:  using_an_api_in_my_rails_project
---


“What should we watch tonight?” This is a common question in my household. Often followed by wasted time scrolling through Netflix, Amazon, or something like them. So I decided to create a Watchlist application to keep track of interesting programs I want to watch someday. Now when the question comes I can fire up Watchlist and pick any of the programs or films I already know I want to watch. Genius.

Building the Watchlist application:

I thought I would tap into the Amazon or Netflix or IMDB library to get details about shows and movies. They surely have APIs, right? Nope, they don’t. I discovered a cool alternative OMDB. 

[OMDB](http://http://www.omdbapi.com/) is an API only database of film and series programming details. Beautiful, but now I have to figure out how to actually use it.

The first thing I needed was an API key from OMDB. That was pretty easy, just go to their site and ask them for a key.  OK, I have the key, now what do I do with it?

I need to build the API request into my show view for any film or series I add to my watchlist, but I had no idea how to do this; so time for Google. Here is what I learned:

Create an app/services directory, then put show_details.rb into it.

Next build a class in show_details.rb with a method that initializes two variables, one for a show, and one for the type of show (series or film). 

```
class Details

    def initialize(show, type)
      @show = show.gsub(" ","+")  
      @type = type
    end
```

Add another method to retrieve show details using a get request and the variables initialized above to build a URL that queries the OMDB API and saves the result in a variable called response. 
```
def get_details
      response = RestClient::Request.execute( method: :get, url: 'http://www.omdbapi.com/?t=' + @show +  '&type=' + @type + '&plot=full&apikey=OMDB_KEY')
```
 	 
Parse the results from OMDB in held in response using JSON.parse. OMDB offers either JSON or XML as formats for their results. JSON is their default so I went with it.

```
JSON.parse(response)
  	end	
end

```
      
In the show action, initialize a details object with the title and type of the show being added, and set @details equal to the return value from the get_details method which should be the parsed JSON results from OMDB.

```
details = Details.new(@show.show_title, @show.show_type)
@details = details.get_details
```

Use @details in the show.html.erb view to list out the details of the show found or an error message if no matches were found. 

```
<h4> Show Details from OMDB </h4>
  <% if @details["Response"] == "False" %>
    	<p><%= @details["Error"] %></p>
  <% else %>
 		  <p>Title: <%= @details["Title"] %></p>
  		<p>Year: <%= @details["Year"] %></p>
			<p>Rated: <%= @details["Rated"] %></p>
			<p>Released <%= @details["Released"] %></p>
  		<p>Runtime: <%= @details["Runtime"] %></p>
		  <p>Genre: <%= @details["Genre"] %></p>
 		<p>Director: <%= @details["Director"] %></p>
 		<p>Writer: <%= @details["Writer"] %></p>
 		<p>Actors: <%= @details["Actors"] %></p>  		<p>Type: <%= @details["Type"] %></p>  		<p>Plot: <%= @details["Plot"] %></p>
   <% end %>
```

Show Details from OMDB:

Title: Dragnet
Year: 1951–1959
Rated: N/A
Released 16 Dec 1951
Runtime: 30 min
Genre: Crime, Drama, Mystery
Director: N/A
Writer: Jack Webb
Actors: Jack Webb
Type: series
Plot: "The story you are about to see is true", "Just the facts, ma'am", "We were working the day watch" - phrases which became so popular as to inspire much parody - set the realistic tone of this early police drama. The show emphasized careful police work and the interweaving of policemen's professional and personal lives.
Done.




