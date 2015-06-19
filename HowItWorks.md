How does Easy Profiler work ?
We can answer this question by listing the few steps required to put Easy Profiler into action:
Integrate the instrumentation routine definition file into your project (a C++ file header)
Instrument your code using the simple API provided.
Place Profiler.dll next to your project executable.
Run your program.
Use Potato Observer to open the outputted data and analyse/visualize its content.
When you run your instrumented code, potatoProfiler DLL collects then flushes timing informations related to the only features that you indicate. When you open a profiling data file using Potato Observer, that data is then defragmented and you are ready to apply any analysis/compare operation that you wish to do.
Potato Observer has a 3 areas layout :

![http://lh3.ggpht.com/_5sK5LKdcP34/TCZgfJhoR9I/AAAAAAAAAeY/Zp6J2HZ7plc/3250565887_fc2fb9f124_o.png](http://lh3.ggpht.com/_5sK5LKdcP34/TCZgfJhoR9I/AAAAAAAAAeY/Zp6J2HZ7plc/3250565887_fc2fb9f124_o.png)

The left tree pane shows you the profiling data defragmentation results. This enables you to remember what features you indicated in the instrumented code. While it provides you raw timing information values and such information like thread handles,etc, you are not going to use those values. Instead you will apply an analysis/compare operation in order to finally observe an interaction result. The Top Ribbon, provides you with the necessary tools to apply upon your choosen features. It is possible to open many test data and have them on the left pane. Thus you can select from the compare tools and visualize interactions among tests history.
All analysis/compare operations have their results shown in the main chart area.