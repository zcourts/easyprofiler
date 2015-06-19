_In this tutorial we try to profile an acceptor-reactor server implementation. First we explain the code situation before we provide an instrumentation solution with the aim of helping to understand how Easy Profiler can be used to describe a particular code model, how to gather performance data and how to perform analysis/compare operations that aid in interpretation._

The server consist of a :

Listening thread : this threads waits for connexions requests and accepts them.
Pool of worker thread that process asynchrnous events that are queued in a common FIFO queue.
and of course the main thread that starts the listener, initializes the queue and creates the pool and waits for a "termination" signal.
> To model threads, the following instrumentation routines are given :

threadRun("threadName");

threadPause("threadName");

threadRun marks the event of entering a new running state, while threadPause is used to indicate that the thread is entering a blocking state. This is not a strict use-case. You are not obliged to provide the profiler with the exact moments : you may be interested to strip away some execution code and only attribute the job being performed by the thread to particular blocks. The profiler automatically collects the threadID and records the "runs" periods of the named thread. You will be able then to visualize the execution hostory of a particular thread, note duration irregularities, analyze the job periods against fine tasks,etc.

In our situation, we would klike to monitor the worker threads that handle incoming requests. Each worker waits for incoming events from the FIFO queue then begin processing it as soon as it comes before it attempts to dequeue a request from the queue or enter a blocking state untill something arrive :

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZmGYqSTfI/AAAAAAAAAgA/pPXUQ77vDDc/1.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZmGYqSTfI/AAAAAAAAAgA/pPXUQ77vDDc/1.png)


Obviously we would like to surround the code that begins just after the request dequeue and ends with the scope of the while cycle.

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZmGWoWWfI/AAAAAAAAAgE/zrt2f4NNRJc/2.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZmGWoWWfI/AAAAAAAAAgE/zrt2f4NNRJc/2.png)

Our server plays the role of a chess server, that routes chess moves and in-game chat messages,etc. Its good to have that high level function reflected in technical code.

To model a particular zone of code performing a fine task which can be called multiple times and also executed concurrently by multiple threads, the following routines are provided :

beginTask("taskName").

endTask("taskName").

For our situation a major interest is to highlight the decoding process and measure its effect. this is done by instrumenting the ::decodeRequest itself (not the external references to it) :

![http://lh3.ggpht.com/_5sK5LKdcP34/TCZmGUrHnwI/AAAAAAAAAgI/K_QvoYTcJjg/3.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZmGUrHnwI/AAAAAAAAAgI/K_QvoYTcJjg/3.png)

Now we need to highlight each type of request that a worker thread can process. Dispatching a request passes through the dispatch function which has a class factory object that creates the particular service handler for the received request id and passes the request to that handler by calling the virtual ::execute member function :

![http://lh3.ggpht.com/_5sK5LKdcP34/TCZmGqSeIPI/AAAAAAAAAgM/AbgtnMYwLrs/4.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZmGqSeIPI/AAAAAAAAAgM/AbgtnMYwLrs/4.png)

Thereby, to instrument the code we only need to surround the ::execute member code with beginTask,endTask with the according task name. eg.
![http://lh4.ggpht.com/_5sK5LKdcP34/TCZmGs9XyjI/AAAAAAAAAgQ/pvGuX2XqlVw/5.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZmGs9XyjI/AAAAAAAAAgQ/pvGuX2XqlVw/5.png)

or
![http://lh6.ggpht.com/_5sK5LKdcP34/TCZmT_0E-6I/AAAAAAAAAgU/tJKJMtf8mqU/6.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZmT_0E-6I/AAAAAAAAAgU/tJKJMtf8mqU/6.png)

Finally we alter the main thread's code as follows :

![http://lh4.ggpht.com/_5sK5LKdcP34/TCZmT81O69I/AAAAAAAAAgY/G4ziRF6BCrg/7.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZmT81O69I/AAAAAAAAAgY/G4ziRF6BCrg/7.png)

this will be considered as the life duration for the server and will be used for later lanalysis. Now we are ready to test and collect the results :
![http://lh3.ggpht.com/_5sK5LKdcP34/TCZmUKzYT6I/AAAAAAAAAgc/Tu06MKD0Qn0/8.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZmUKzYT6I/AAAAAAAAAgc/Tu06MKD0Qn0/8.png)

We can visualize the activity of a particular worker in term of durations spent for each new incoming request. All we have to do is to go to the analysis tab, select our opened test, then set the focus to the desired worker, then select the appropriate analysis button.
![http://lh4.ggpht.com/_5sK5LKdcP34/TCZmUGNB7CI/AAAAAAAAAgg/wXFN_xQPEjQ/9.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZmUGNB7CI/AAAAAAAAAgg/wXFN_xQPEjQ/9.png)

![http://lh5.ggpht.com/_5sK5LKdcP34/TCZmUHamLrI/AAAAAAAAAgk/nAMb7lvdfWc/10.png](http://lh5.ggpht.com/_5sK5LKdcP34/TCZmUHamLrI/AAAAAAAAAgk/nAMb7lvdfWc/10.png)

Thus we can have an idea of the average time spent by a worker to process a request. We know that each request processing is preceded by a decoding task. We can see this if we choose a worker in the "selection" ribbon category and the task "decoding" in the "worker" category :

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZmZB7HgSI/AAAAAAAAAgo/XetBbD9hz1U/11.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZmZB7HgSI/AAAAAAAAAgo/XetBbD9hz1U/11.png)

chart :
![http://lh4.ggpht.com/_5sK5LKdcP34/TCZmZMrh4cI/AAAAAAAAAgs/Q6IJBB2iikE/12.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZmZMrh4cI/AAAAAAAAAgs/Q6IJBB2iikE/12.png)

The same can go with the remaing tasks. One can try to see if the time-contribution does commesurate with the task role otherwise make a decising to focus on the optimization of that task. In the following chart, total times were computed and compared to each other :
![http://lh6.ggpht.com/_5sK5LKdcP34/TCZmZFD_ySI/AAAAAAAAAgw/bIeSz37ck48/13.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZmZFD_ySI/AAAAAAAAAgw/bIeSz37ck48/13.png)

Analyzing the execution history of a certain task regardless of the worker that performs it, can give information about the behavior, the time-cost of that task and even can help detect the cause of repetitive bottlenecks that may arise in the run history of the worker processing.

![http://lh3.ggpht.com/_5sK5LKdcP34/TCZmZR5nXZI/AAAAAAAAAg0/PCAK536wABI/14.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZmZR5nXZI/AAAAAAAAAg0/PCAK536wABI/14.png)

chart :
![http://lh4.ggpht.com/_5sK5LKdcP34/TCZmZXDEbbI/AAAAAAAAAg4/2bSLOyv-Nm4/15.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZmZXDEbbI/AAAAAAAAAg4/2bSLOyv-Nm4/15.png)

Now its good to relate the workers execution or tasks execution to the total lifetime ie the activity :

![http://lh5.ggpht.com/_5sK5LKdcP34/TCZmombY0jI/AAAAAAAAAg8/1WzrYJjd3p0/16.png](http://lh5.ggpht.com/_5sK5LKdcP34/TCZmombY0jI/AAAAAAAAAg8/1WzrYJjd3p0/16.png)

Next we see if our worker was busy or not :

![http://lh5.ggpht.com/_5sK5LKdcP34/TCZmo7vjf6I/AAAAAAAAAhA/NUDC-O-Evso/17.png](http://lh5.ggpht.com/_5sK5LKdcP34/TCZmo7vjf6I/AAAAAAAAAhA/NUDC-O-Evso/17.png)

in addition we can sum execution times for all tasks then represent them in a pie :

![http://lh6.ggpht.com/_5sK5LKdcP34/TCZmo-a8VqI/AAAAAAAAAhE/03Y9t8IyGjI/18.png](http://lh6.ggpht.com/_5sK5LKdcP34/TCZmo-a8VqI/AAAAAAAAAhE/03Y9t8IyGjI/18.png)

Of course Easy profiler allow you to show the data in different ways :

![http://lh3.ggpht.com/_5sK5LKdcP34/TCZmo13d5vI/AAAAAAAAAhI/Xf_pT97AfxU/19.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZmo13d5vI/AAAAAAAAAhI/Xf_pT97AfxU/19.png)

the same past data in a horizontal columns :

![http://lh4.ggpht.com/_5sK5LKdcP34/TCZmpBGpiTI/AAAAAAAAAhM/zYXXdjJV7J0/20.png](http://lh4.ggpht.com/_5sK5LKdcP34/TCZmpBGpiTI/AAAAAAAAAhM/zYXXdjJV7J0/20.png)