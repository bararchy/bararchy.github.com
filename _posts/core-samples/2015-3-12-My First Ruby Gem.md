---
layout: post
category : experiences
tags : [ruby, development]
---

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
  
* Ruby REST client implementation
  * <a href="https://github.com/rest-client/rest-client"> rest-client </a>
*   API Docs from the server developer


<br>
now what ?<br>

Create a basic Ruby method to contact the server and output some data:

~~~
# Lets require the needed gem

require 'rest-client'

# Now, lets build a basic call using the supplied API

def iVerifyUserAccount
  RestClient::Request.execute(
    method: :post,
    url: 'https://isafe-t.com/ui_api/login.aspx',
    headers: {
      servletRequestID: 'MethodRequest',
      BusinessLogic: "{Username:'', Password:'', RoleID: '00003', ExtensionID: 'mobile', iVerifyUserAccount: ['test', '1234', true]}"
    }
  )
end
~~~
{: .language-ruby}

<br>
Running the above example resulted in the right output, nice ! looks like Ruby is super easy ha ? <br>
So... what now ? apparently roboticly recreate each method for each call to the API <br>
This what the code looked like:
<a href="https://github.com/bararchy/safe-t-rest/blob/eb974ed1977c817aa131af772bd8c309b443eb64/lib/safe-t-rest.rb"> Very ugly code </a> <br>
As I'm not a expert or even formally educated programmer, for me this was it.<br>
I thought to myself, Yey ! project done, let ask the guys at ruby-talk for inputs (and inputs I got..)<br>
I got allot of responses but basically this was it:

* Respect Ruby Conventions !
  * Use 2 space for indentation (I used 4)
  * Name methods with snake_case (I used original method names such as "iVerifyUserAccount")
* Optimize Code !
  * Get rid of the repeating code (reuse string, methods, trim the code)
  * Use initializer to get instance data (my .new didn't allow any arguments to be passed)
* Documents !
* Tests !

<br>
So lets go to work !<br>


* Create one method which sends the required changing string

~~~
def send_request(request_string)
  RestClient::Request.execute(
    method: :post,
    url: @url,
    headers: {
      servletRequestID: 'MethodRequest',
      BusinessLogic: "{Username:'', Password:'', RoleID: '#{@role_id}', ExtensionID: '#{@extension_id}', #{request_string}}"
    }
  )
end
~~~
{: .language-ruby}

* Create each unique method to use this new "send_request" method

~~~
def iVerifyUserAccount
  send_request("iVerifyUserAccount: ['#{@user_name}', '#{@password}', true]")
end
~~~
{: .language-ruby}

* Nope... Apply Ruby conventions ! 

~~~
def verify_user_account
  send_request("iVerifyUserAccount: ['#{@user_name}', '#{@password}', true]")
end
~~~
{: .language-ruby}

Ohh yeah ! much better !!! <br>

* Let the user initialize with arguments

~~~
def initialize(config_hash={})
  @extension_id = config_hash[:extension_id]
  @user_name = config_hash[:user_name]
  @password = config_hash[:password]
  @url = config_hash[:url]
  @role_id = config_hash[:role_id]
end
~~~
{: .language-ruby}

* now.. now.. this is shaping up to be a nice looking code right here:
<a href="https://github.com/bararchy/safe-t-rest/blob/master/lib/safe-t-rest.rb"> Nicely Done Ruby Code :) </a>

# Thanks again for all your help and support !