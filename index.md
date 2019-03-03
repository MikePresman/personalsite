## The unintentional maliciousness of POST requests
### P.S. This is my first submission on my site

![alt text](https://www.google.com/url?sa=i&source=images&cd=&cad=rja&uact=8&ved=2ahUKEwj__9-d5-XgAhVM5IMKHUaWC_UQjRx6BAgBEAU&url=https%3A%2F%2Fgiphy.com%2Fgifs%2Fscooby-doo-scared-eomy8S00ljwjK&psig=AOvVaw3EIfqQlRPT6qF4uTIFipod&ust=1551696816368401)

While working on a client's website throughout this past month I have had the pleasure of working with Python's light web framework Flask. To be honest this is my first time really diving deep into the wonderful world of web development, and so far im enjoying it - except for the potential for falling into the depths of 8548434995 vulnerabilities and security threats that exist.

A big part of the website that I'm designing allows admin users to add, delete, and modify content such as gallery images, items in the Shop that are for sale or even the authorization rights of other users in the database.

Flask's documentation is great. Its concise, easy to navigate and has enough examples to point you in the right direction; that is until I started to really think about POST requests.
POST requests are how I decided to handle adding, deleting, and modifying the aforementioned. Its great because its done on the server side and relatively easy.

Unfortunately what Flask's documentation don't mention is the importance of validating from who the POST requests come from.

```markdown
if request.method == "POST":
  delete-photo-id = request.form.get("delete-photo")
  photo = Photo.query.filter_by(id = delete-photo-id).delete()
  db.session.commit()
```

The above code is simple and in theory works great.
If the web page was loaded with a POST request, grab the id of the photo you want to delete and delete it.
Great right? 
The flask documentation would lead you to believe so, as I did for a few weeks.

But as I was laying in bed I began to question the universe, and more importantly POST requests from an outside perspective. I was curious, what if I just wanted to send a POST request from anywhere on the web to my website, would that be possible?
So I did some digging and came across [cURL](https://curl.haxx.se/). "curl is used in command lines or scripts to transfer data." And as simple as it sounds, is as simple as it does.

Within 5 minutes of googling and modifying I was able to write my own POST request in my command line/terminal.

~~~
curl -d delete-image=1 http://localhost:5000/gallery
~~~

This simple one liner deleted my image from my website without having an login necessary as admin or even non-admin.
POST requests claim to hide data, and they do, but in the most naive sense.

Moral of the story: If you have a POST request, make sure you validate it with permissions by making sure if its sensitive data being manipilated then only certain people can do so.
e.g.

```markdown
if request.method == "POST":
  if user.authority_level > 0:
    delete-photo-id = request.form.get("delete-photo")
    photo = Photo.query.filter_by(id = delete-photo-id).delete()
    db.session.commit()
```

After adding that one line of code, the cURL no longer maliciouslly deleted my image and my POST was now safe



