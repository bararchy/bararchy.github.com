---
layout: post
category : experiences
tags : [ruby, development]
---
{% include JB/setup %}


# Thanks guys !
<br>
This is my first try in blogging.<br>
I started this 'project' of a kind to write about my experience in creating my first ever Ruby GEM ! (DAM DAM DAMMM).<br>
This was a fun and educating experience, and I especially was amazed by the great feedback from people on the ruby mailing list and the ruby IRC channel on freenode.<br><br>

# How did it all start ?
<br> 
One sunny morning an email arrived from Safe-T's server developer, the email stated:<br>
Our new product <a href="http://www.safe-t.com/safe-t-box/"> Safe-T Box </a> has a new REST based API, it is your duty (mine alone...) to create a usage example of this new API using a language of your choice.<br>
As I already had some experience with ruby (automation and scripting for our servers) the choice was clear.<br>
<br>
# Planning (or the lack thereof..)
<br>
The first thing I did was thinking about What I need for this to work:<br>
<li> Ruby REST client implementation </li>
+ rest-client
* API Docs from the server developer
  [x] ask and you shell receive ! at lest I did... in the next mail.

```ruby

def testing(test)
  puts test
end

testing('hi')
=> "hi"

```