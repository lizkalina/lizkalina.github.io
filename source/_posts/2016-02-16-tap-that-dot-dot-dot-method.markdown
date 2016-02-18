---
layout: post
title: "TAP THAT...method"
date: 2016-02-16 22:37:40 -0500
comments: true
categories: ["Flatiron&nbsp;School","#slackbeef","tap"]
---


Why #tap you may ask? This method made me have a ‘Ruby, you rock' moment. I first stumbled upon it during the ‘Advanced Class Methods Lab,’ which had us write methods to create new instances of a Song class, modify that instance and return it. Although my code passed the tests, it had a bad code smell. I couldn’t say what exactly was wrong, but it just felt off. I brought this up with Ian who suggested I look into the tap method. I had no idea what that was so I did a little research, refactored my code with tap and celebrated with a ‘YASSSSS.’

{% img center https://media.giphy.com/media/kGZ4jJguXT5C0/giphy.gif 1928 620 %}
<br>
#What can #tap do for you?
Let's take a quick look at a before and after of my 'Advacned Class Methods Lab.' Both sets of code get the job done. In my earlier code, there was a line for instantiating the object, then a few lines of operations on that object and then a final lines returning that same object. Another blogger aptly called this 'sandwich code.'' After refactoring, I could do creation, operations and return all in one line!

###BEFORE #tap
```ruby
  def self.create
    song = self.new
    song.save
    song
  end

  def self.new_by_name(song_name)
    song = self.new
    song.name = song_name
    song
  end
```

###AFTER #tap
```ruby
 def self.create
    self.new.tap{|song| song.save}
  end

  def self.new_by_name(song_name)
    self.new.tap{|song| song.name = song_name}
  end
```
###side by side from GitHub
![github code](/images/tap-method/github-example.png)

<br>
#How does #tap work though? 

The [official Ruby documentation](http://ruby-doc.org/core-2.3.0/Object.html#method-i-tap) says:

>tap{|x|...} → obj
>Yields self to the block, and then returns self. The primary purpose of this method is to “tap into” a method chain, in order to perform operations on intermediate results within the chain.

###So essentially, the #tap method definition is:
```ruby
def tap
  yield(self)  
  self  
end  
```
###and when #tap is called:
```ruby
an_object.tap do |object|
  # do stuff with an_object
end # ===> return an_object
```

Self is the object tap is being called on (i.e. 'an_object'). When tap runs, self first yields to the block (i.e. the code between do and end). And finally self (with block operations completed) will be returned.

###Going back to my Song example this would mean:

* self is Song.new
* yields to {|song| song.name = song_name} passing in Song.new
* returns Song.new with song.name set to song_name

<br>
#Using #tap to seed data
Let’s go through one more example from the seed.rb file during Jeff’s CLI lecture… 

```ruby
courses = ['Math', 'Physics', 'English', 'History'].map do |course_name|
  Course.new.tap do |course|
    course.name = "#{course_name}"
    teacher = Teacher.new("Mr. #{course_name}")
    teacher.name = "Mr. #{course_name} teacher"
    course.teacher = teacher
  end
end
```

This is more complex than the Song example, but really the purpose is the same. First, the map method goes through each item in the array with school subjects, creating a new instance of the Course object for each subject. #tap is called on each new Course object. Operations happen: A name is assigned to the Course, a new instance of the Teacher object is created and assigned as the teacher for the Course. After all that, the four new Course objects (with attributes assigned) is returned to tap, which is then returned to map. So the 'courses' variable now equals:

```ruby
[<Course:0x007f9e39047e80 @name="Math", @teacher=<Teacher:0x007f9e39047db8 @name="Mr. Math teacher">>, 
<Course:0x007f9e39047d68 @name="Physics", @teacher=<Teacher:0x007f9e39047cc8 @name="Mr. Physics teacher">>, 
<Course:0x007f9e39047c78 @name="English", @teacher=<Teacher:0x007f9e39047bd8 @name="Mr. English teacher">>, 
<Course:0x007f9e39047b88 @name="History", @teacher=<Teacher:0x007f9e39047ae8 @name="Mr. History teacher">>]
```

###This allowed us to quickly create a ton of sample data with minimal lines of code!

Takeaways: #tap is pretty awesome! It’s great for assigning properties to an object and overriding method returns. In simple cases like my Song example, it’s really just for convenience, but for more complex examples like the CLI seed file, it can actually lead to cleaner, clearer code. Watch out though, #tap should not be used to make your code look super fancy or obscure what the code is actually doing. Other interesting use cases still TBD.

{% img center https://media.giphy.com/media/3oEdv5S8Th6b9gsNqM/giphy.gif %}
#<center>damn, that sandwich can tap</center>
<!-- external-url: http://ruby-doc.org/core-2.3.0/Object.html#method-i-tap


<script src="https://gist.github.com/lizkalina/7a037a413995deeb4144.js"></script> -->

<!-- 
!['yas, queen'](https://media.giphy.com/media/kGZ4jJguXT5C0/giphy.gif "yas, queen") -->
