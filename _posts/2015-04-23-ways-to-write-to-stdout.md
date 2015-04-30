---
layout: post
title: Story of p, puts, print and the lesser known - syswrite
tags: stream, IO
---

While developing in Ruby I have been using puts method and to some less extent p method.While debugging  or in irb console I used  'puts' mostly. I always wondered  what are all other possible ways in which I can  write to standard output ($stdout).
So here it is.

1. print
  * it can take multiple arguments and converts them to string by calling __to_s__ on the arguments passed to it. Then it writes to the stream.  
  ![alt text]({{site.url}}/images/print_console.png "print method")
2. puts
  * operated just like __print__, but ensure that newline is appended whatever is written.  
  ![alt text]({{site.url}}/images/puts_console.png "puts method")
3. p
  * it writes to the stream just like __print__
  * it calls  __inspect__ on the objects passed to it, hence it is good for debugging.
    Let's say you have a Person class

    ```ruby  
    class Person
      attr_accessor :name, :age, :address

      def initialize(name, age, address)
        @name, @age, @address = name, age, address
      end
    end  
    ```
     
    And you want to inspect the state of a person object.

    Instead of using "puts person " use "p person" if there is no to_s method defined.  
    ![alt text]({{site.url}}/images/p_console.png "p method")

    You can't call __p__ on the $stdout directly, as it is  private method. But Ruby Kernel exposes __p__, hence we can call it in irb session.

4. syswrite

  * it can also write to the stream. But it does a __low level__ write. What does that mean that you may ask? It means that it writes directly to the underlying operating system buffers.  
  ![alt text]("{{site.url}}/images/syswrite_console.png" "sysswrite")



So when we use any of the first three methods above to write to a stream, they are first written to Ruby buffers, and then passed to the underlying operating system buffers.  

 Since __syswrite__ writes directly to the underlying operating system buffers, it should not be used along with  with any of the first three methods above.

 Having said that, $stdout, just like $stdin and $stderr, is an IO stream. All three methods above are implemented in terms of __write__ method.

 And the idiomatic way of writing *single object* to stream is use __<<__ method.
