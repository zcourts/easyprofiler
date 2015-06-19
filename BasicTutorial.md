Jeff >> Hello Bob,
Bob>> Hello Boss
Jeff>> We have an annoying button here Bob. I repeated this use process in our application multiple times : creating a document template is fine,storing and reading are OK too. All is fine until I choose to export the document and visualize the result. It takes at least 12 seconds before the result is finally created and rendered.
Rob>> You should see with Dick, he coded that part.
Jeff>> As you know, Dick is on a vacation today.Furthermore I have a demo to present in two days in front of a major client.
Rob>> OK boss I'll check that out but frankly I cannot garantee you that I can solve this.
Jeff>> Try do your best.


Bob was able to identify the code part responsible for the slow button :

![http://lh4.ggpht.com/_5sK5LKdcP34/TCZjv4jOp-I/AAAAAAAAAfA/ua60Ix5YGVw/b1.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZjv4jOp-I/AAAAAAAAAfA/ua60Ix5YGVw/b1.png)

There appear to be 4 separate stages in this code, making it very likely that the problem cause is lying somewhere in one of them, other than because of a common code that is causing a contributional effect.

Bob needs to employ a simple feature keyword in instrumentation : activity, and mark its inner events with checkpoints :

![http://lh4.ggpht.com/_5sK5LKdcP34/TCZjv2p6RWI/AAAAAAAAAfE/Cppob2WSg7E/s576/b2.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZjv2p6RWI/AAAAAAAAAfE/Cppob2WSg7E/s576/b2.png)

> the target code portion is named "my region" and was surrounded by startProfilingRegion and flushProfilingRegion.

Now it is time to run the application and create the resulting profiling data. The execution produces an xml data file in the directory C:/profiling output. Bob, can open the data :

![http://lh3.ggpht.com/_5sK5LKdcP34/TCZjv8ktfgI/AAAAAAAAAfI/g9uK3HjY6Zo/b3.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZjv8ktfgI/AAAAAAAAAfI/g9uK3HjY6Zo/b3.png)


select the test that is available in the opened file :

![http://lh4.ggpht.com/_5sK5LKdcP34/TCZjv7H6gXI/AAAAAAAAAfM/vRrp4tlqPG8/b4.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZjv7H6gXI/AAAAAAAAAfM/vRrp4tlqPG8/b4.png)

Then choose the activity : "visualize document" and press the timeline analysis button :

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZjwDyjMuI/AAAAAAAAAfQ/RLIcMdwWKx0/b5.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZjwDyjMuI/AAAAAAAAAfQ/RLIcMdwWKx0/b5.png)

The resulting chart :

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZkWZDK_jI/AAAAAAAAAfU/9iPlC8OzH4M/b6.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZkWZDK_jI/AAAAAAAAAfU/9iPlC8OzH4M/b6.png)

Jeff >> Rob, what is this chart ?
Bob>>I am profiling Dick's code.
Jeff>> So you are visualizing the execution time for each function ?
Bob>>Not exactly. Its like a timeline here with absolute values. Do you see the bottom labels ? OK look at the duration spent in each stage :

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZkWqT1cGI/AAAAAAAAAfY/c7DoHabphRQ/b7.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZkWqT1cGI/AAAAAAAAAfY/c7DoHabphRQ/b7.png)

Jeff>>I see ..So here the document is first loaded, then we apply that XSLT trasform before we render it.
Bob>>Exactly.
Jeff>>..This is really strange. if it is not because of the way how the data is being loaded, how it comes that the other operation stages are processing fast the same flow of incoming data ?
Bob>> This is going to bring us back to an inconvenient discussion.
Jeff>>Why ?
Bob>>You remember the first meeting when we all discussed predesign options and strategies for our application ?
Jeff>> Ah, ok, you mean that SAX Vs DOM issue you raised with Dick about which to consider for data loading..Ok it seems I have to see with Dick then.

> The next day. Dick is playing chess at work instead of doing an actual work.As usual.

Jeff>> Good Morning Dick, what are you doing ?
Dick>> Ah, I'm writing specs of our next application vesrion.


Jeff>> I see, well I need you to go back to past code and try to optimize that code part that opens the document in memory before it is exploited. It is causing a performance issue.
Dick>> OK Boss.

![http://lh5.ggpht.com/_5sK5LKdcP34/TCZkWleA7BI/AAAAAAAAAfc/UnI7kShSBIA/b8.png](http://lh5.ggpht.com/_5sK5LKdcP34/TCZkWleA7BI/AAAAAAAAAfc/UnI7kShSBIA/b8.png)

There appear to be 4 separate stages in this code, making it very likely that the problem cause is lying somewhere in one of them, other than because of a common code that is causing a contributional effect.

Bob needs to employ a simple feature keyword in instrumentation : activity, and mark its inner events with checkpoints :

![http://lh5.ggpht.com/_5sK5LKdcP34/TCZkW9kXeVI/AAAAAAAAAfg/jJ9ruQmho0k/s576/b9.png](http://lh5.ggpht.com/_5sK5LKdcP34/TCZkW9kXeVI/AAAAAAAAAfg/jJ9ruQmho0k/s576/b9.png)

the target code portion is named "my region" and was surrounded by startProfilingRegion and flushProfilingRegion.

Now it is time to run the application and create the resulting profiling data. The execution produces an xml data file in the directory C:/profiling output. Bob, can open the data :

![http://lh3.ggpht.com/_5sK5LKdcP34/TCZkW7xNfAI/AAAAAAAAAfk/YyM7MHFydnw/b10.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZkW7xNfAI/AAAAAAAAAfk/YyM7MHFydnw/b10.png)


> select the test that is available in the opened file :
![http://lh4.ggpht.com/_5sK5LKdcP34/TCZk7ovPfrI/AAAAAAAAAfo/xdDHgCNrC5E/b11.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZk7ovPfrI/AAAAAAAAAfo/xdDHgCNrC5E/b11.png)


Then choose the activity : "visualize document" and press the timeline analysis button :

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZk7n6FDVI/AAAAAAAAAfs/21zczCkKAW8/b12.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZk7n6FDVI/AAAAAAAAAfs/21zczCkKAW8/b12.png)


> The resulting chart :
![http://lh6.ggpht.com/_5sK5LKdcP34/TCZk7gshoCI/AAAAAAAAAfw/IGCGuzdrutA/b13.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZk7gshoCI/AAAAAAAAAfw/IGCGuzdrutA/b13.png)

Jeff >> Rob, what is this chart ?
Bob>>I am profiling Dick's code.
Jeff>> So you are visualizing the execution time for each function ?
Bob>>Not exactly. Its like a timeline here with absolute values. Do you see the bottom labels ? OK look at the duration spent in each stage :

![http://lh3.ggpht.com/_5sK5LKdcP34/TCZk78LJGMI/AAAAAAAAAf0/ZM0jRGWNQWU/b14.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZk78LJGMI/AAAAAAAAAf0/ZM0jRGWNQWU/b14.png)

Jeff>>I see ..So here the document is first loaded, then we apply that XSLT trasform before we render it.
Bob>>Exactly.
Jeff>>..This is really strange. if it is not because of the way how the data is being loaded, how it comes that the other operation stages are processing fast the same flow of incoming data ?
Bob>> This is going to bring us back to an inconvenient discussion.
Jeff>>Why ?
Bob>>You remember the first meeting when we all discussed predesign options and strategies for our application ?
Jeff>> Ah, ok, you mean that SAX Vs DOM issue you raised with Dick about which to consider for data loading..Ok it seems I have to see with Dick then.

> The next day. Dick is playing chess at work instead of doing an actual work.As usual.

Jeff>> Good Morning Dick, what are you doing ?
Dick>> Ah, I'm writing specs of our next application vesrion.
Jeff>> I see, well I need you to go back to past code and try to optimize that code part that opens the document in memory before it is exploited. It is causing a performance issue.

Dick>> OK Boss.



Dick>>Jeff, I need to come and see this data.
Jeff>>What the hell is this ?
Dick>> This is the profiling report from the IDE. You see this function strcpy : it is evry where. Look here and there, examine this calls graph. We can't avoid that Jeff. Data copy is an essential part of our code.
Jeff>>I do'nt understand what you are talking about and I do'nt know what these low level results imply. Bob showed me another result yesterday and I insist you revise the way you load the document data.

..

Dick>> Bob, what's going on ?
Bob>> It's not my fault, optimize your code befor you go on vacation !
Dick>>Fine.

..

Dick>> Jeff ! Look they are 3 seconds now !
Jeff>> Really ? How did you do that ?
Dick>> I changed the PDF printing code that was developed by Bob.

Jeff>>Bob : I need the two charts now with that Profiler from the Potato website, I need to check if Dick is not fooling us.

A second test with Easy Profiler has resulted in an additional test file that is opened along the first. It is now possible to select the two tests, then choose to compare the two timelines of the activity :"visualize document".

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZk7xU8gRI/AAAAAAAAAf4/skfweIiGJXM/b15.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZk7xU8gRI/AAAAAAAAAf4/skfweIiGJXM/b15.png)


Jeff>> Look at how Dick is so upset of Bob ! Ha ha ha.