---
layout: post
title: "Param-arama: the Params Hash"
date: 2016-03-02 20:13:04 -0500
comments: true
categories: ["Flatiron&nbsp;School","http&nbsp;requests"]
---

The params hash was first introduced to me with forms. When a user submitted a form in the view, a hash with that data was (magically?) passed to the controller. We later learned that the params hash can also be populated with data from the URL string. In this post, I will go over what params are, how they are passed from the view to the controller and review how to create/manipulate/accesss the params sent.

go over what params are, how they works and how they relate to the HTTP protocol. 

###<center>form data goes in, params hash comes out?</center>
{% img center https://media.giphy.com/media/x7l3pkXcDrKPm/giphy.gif 1928 620 %}

<br>
#Query String Parameters vs. POST Data

 After doing some digging, I discovered there are officially two types of parameters. The first type can be specified in the URL string and the other can be sent via a POST request. This is based on HTTP protocol, which handles all communication between users and databases.

According to the [Ruby on Rails Guides website](http://guides.rubyonrails.org/v3.2.9/action_controller_overview.html):

>The first are parameters that are sent as part of the URL, called query string parameters. The query string is everything after “?” in the URL. The second type of parameter is usually referred to as POST data. This information usually comes from an HTML form which has been filled in by the user. It’s called POST data because it can only be sent as part of an HTTP POST request.

In both cases, the data is passed from the user to the controller as a params hash. This happens in the Request message body for POST requests and in the Request line for GET requests. Let's take a quick look at what these different request types look like.

##Form with POST Request
```ruby
<form action="http://foo.com" method="POST">
  <input name="say" value="Abracadabra">
  <input name="to" value="Gob Bluth">
  <button>Send Magic</button>
</form>
```

the HTTP request would look something like this...
```http
POST / HTTP/1.1
Host: foo.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30

say=Abracadabra&to=Gob%20Bluth
```

##Form with GET Request
```ruby
<form action="http://foo.com" method="GET">
  <input name="say" value="Abracadabra">
  <input name="to" value="David Blaine">
  <button>Send Magic</button>
</form>
```
the HTTP request would look like...
```http
GET /?say=Abracadabra&to=David%20Blaine HTTP/1.1
Host: foo.com
```
So with a GET request, the user will see the data in their URL bar, but with a POST request, they cannot. This is relevant for two main reasons. First, if you're sending sensitive info, such as passwords, you do not want this displayed in the URL. Secondly, you want to use a POST request for large data sets because some browsers limit the length of URLs they accept.

<br>
#Accessing Params from the Controller

When a person submits a form, the web server stores the data from the form in a request object, executes the action/method route specified in the form tag to the controller. From there, you can access and manipulate the params hash by simply typing params. Side note: The params hash is actually an instance of HashWithIndifferentAccess from Active Support. It behaves exactly like a hash, but you can access its elements using strings or symbols as keys.

##nested parameters

Form data will be structured based on how you set up your form in the view. Hashes are created by placing the name of the nested hash between the square brackets as the name value for an input. From the controller, that bracket value will become the key to access the user's input e.g. params[:owner_id]. You could chain additional brackets/values after the first to create a nested tree of hashes and arrays.

in the view:
```html
  <label>Choose an owner:</label>
  <% Owner.all.each do | owner | %>
  <input type="checkbox" name="pet[owner_id]" value="<%= owner.id %>" id="<%= owner.id %>">
  <%= owner.name %>
  <% end %>
```
in the controller:

**params = {"pet"=>{"name"=>"Michael", "owner_id"=>"1"}}**

##array parameters

To send an array of values to your controller for a given key, append empty brackets to the value attribute e.g. owner[pet_ids][]. The multiple values selected (via checkboxes or otherwise) are stored in the params hash in an array. Each items value is set by their associated HTML element tag value. You can access this array from the controller in the same way you would a string e.g. params[:owner][:pet_ids]. The ability to create these array is extremely useful, especially when it comes to mass assignment for creating or updating objects.  

```html
  <label>Choose the owner's pets:</label>
  <% Pet.all.each do | pet | %>
  <input type="checkbox" name="owner[pet_ids][]" value="<%= pet.id %>" id=<%= pet.id %>>
  <%= pet.name %>
  <% end %>
```
in the controller:

**params = {"owner"=>{"name"=>"Sophie", "pet_ids"=>["1","3","5"]}}**


###Helpful Resources
* [Sending Form Data](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms/Sending_and_retrieving_form_data), Mozilla Developer Network
* [Action Controller Overview](http://guides.rubyonrails.org/v3.2.9/action_controller_overview.html), RailsGuides