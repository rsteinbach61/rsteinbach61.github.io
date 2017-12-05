---
layout: post
title:      "One Cool Thing I Learned During My Sinatra Project"
date:       2017-12-05 14:47:17 -0500
permalink:  one_cool_thing_i_learned_during_my_sinatra_project
---





I wanted to allow users to upload images as part of the Home Inventory App I'm building for my Sinatra project. 

The App is designed for users who want to keep an inventory of the items in their home(s). An inventory would be useful in the event of a total loss due to some catastrophic event like fire, earthquake, flood ... Having a picture and/or receipt image would be very valuable.

I had no idea how to build an image file option into my App. Do I put the file in a DB? No! It turns out that is a bad idea due to the potential size of the files. So the better way is to put a link to the file in the DB table and store the files on a server.

Still I had no idea how to do this. Lucky for me there is Stack Overflow and Google! Here is what I learned. Maybe it will help you too.

First you need to find your photo files. I'm using a Mac so this was my first hurdle. I snapped a few pictures with my phone and then could easily find them in Photos on my MacBook, but didn't know how to access the files so I could copy or move them.

[Here is a discussion about finding Photos files on a Mac](https://apple.stackexchange.com/questions/184578/open-location-of-a-photo-in-finder-from-photos-app)

Once I knew how to find my pictures I moved them to my desktop for ease of use. Now I had to learn how to upload them into my App. 

First I built a form that allows users to browse for a file they want to upload:

```
<h3>Upload Picture or Receipt</h3>

      <form action="/items/upload" method="POST" enctype="multipart/form-data">
			
      <p><input type="file" name="file" ></p>
			
			<input type="submit" value="Upload File">
			
      </form>
```

There were two things new to me about this form:

1. To upload an image file you must use enctype="multipart/form-data" when opening your form.
2. Set the input type to "file" - this creates a button/field that opens a browsable file menu.

Here are the params from my form:

```
{"file"=>
  {"filename"=>"IMG_0395.jpg",
   "type"=>"image/jpeg",
   "name"=>"file",
   "tempfile"=>#<File:/var/folders/9r/m_s7yyt11tv9y44r7l1nvgr80000gn/T/RackMultipart20171205-98966-1a5mynw.jpg>,
   "head"=>"Content-Disposition: form-data; name=\"file\"; filename=\"IMG_0395.jpg\"\r\nContent-Type: image/jpeg\r\n"},
 "house_id"=>"51",
 "item"=>"Clock"}
```

That wasn't too hard! But now I need a controller action. I built that in my item controller: 

```
post '/items/upload' do

    @filename = params[:file][:filename] #sets the file name to string in @filename
		
    file_content = params[:file][:tempfile] #saves image file into file_content
```

First I open the POST action, then the first line gets the file name from params and saves the name into a file (@filename). Next the image file's content is saved into a file (file_content)


```
 @house = House.find(params[:house_id])
		
    @item = @house.items.find_by(name: params[:item])
		
    @item.pic_file_name = @filename
		
    @item.save
```

In the snippet above I'm creating a House and Item object, then saving the image file's name to one of the Item's attributes (pic_file_name), and ultimately to the items table in my db. I can then use it to create a link to the image file when I need it.

```
 File.open("./public/media/#{@filename}", 'wb')  do |file| 
			
			file.write(file_content.read) 
							 
end	
```

To save the image file contents I created a media sub-folder in my public folder. The code above creates a new file in my media folder using my @filename variable as the file name. The 'wb' option stands for write binary which is needed since my image file is a binary file. 

The block then writes the contents of file_content to the new file named with the @filename string. 

```
			
    redirect to(:"/items/#{@item.id}")
		
end
```

The action finishes up by redirecting to a page that displays the Item details including a link to the image file for that Item.

```
<img src="/media/<%=@item.pic_file_name%>" alt="Item Picture">
```

Problem solved!




