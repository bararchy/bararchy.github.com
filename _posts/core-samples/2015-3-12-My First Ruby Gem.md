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
  
* Ruby REST client implementation </li>
  * <a href="https://github.com/rest-client/rest-client"> rest-client </a>
*   API Docs from the server developer
  *   ask and you shell receive ! at lest I did... in the next mail.

<br>
now what ?<br>
Create a basic Ruby method to contact the server and output some data:

~~~ruby
# Lets require the needed gem
require 'rest-client'

# Now, lets build a basic call using the supplied API
def send_request(request_string)
  RestClient::Request.execute(
    method: :post,
    url: @url,
    headers: {
      servletRequestID: 'MethodRequest',
      BusinessLogic: "{Username:'', Password:'', RoleID: '00003', ExtensionID: 'mobile', iVerifyUserAccount: ['test', '1234', true]}"
    }
  )
end
~~~



```ruby

def testing(test)
  puts test
end

testing('hi')
=> "hi"

```