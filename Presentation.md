Easy profiler, is a compile-time, manual instrumentation profiler that offers simplicity, ease of use and efficiency.

Standard profilers work by means of an automatic-data collection mechanism. To profile a certain code, you are required to profile the whole application then deal with the interpretation of a final complex output.

While this is approach can be helpfulin certain difficult circumtances, most developers' situations are rather simple and only touche code portions that they wrote, or understand and want to profile individually.

With an automatic profiler, the goal of a quick analysis and interpretation of a particular code interaction is really difficult to achieve : you have to look into the verbose measures, extract the only relevant ones,export them, apply certain transformation in a spreadsheet program (which is sometimes not possible) then visualize the plot.

That is why Easy Profiler works in a different way : at the expense of easy to use instrumentation routines, you are providing the profiler with the target features to track. Output is then collected via a separate viewer where you can instantly apply any analysis operation combining the features measures obtained and visualize the resulting plot as it should be displayed.

If you make many test runs, you can then compare the different sets of results,. Particularly you can visualize the effect of your optimization on separate interactions. For instance, : how much does the region B  contributes to the execution of A after optimization ? or  "where does the thread A spend most of his work among the differents tasks T1, T2,..Tn after my code changes ?",etc.
The strong hypothesis with Easy profiler is that you are not going to consider performance effects made by infrastructure code. For the most cases this hypothesis yields itself because your decisions will not allow you to change the type of API you are calling within your custom code, but only try to optimize your custom code, ie. the custom logic that you added upon the infrastructure code.

The benefit of using Easy Profiler is that you can model and profile code interactions, thus you can tackle performance defects due to architecture choices, not due to particular slow functions.

The output is clean and you do'nt have to be intimidated by thousands of lines of calls reports to obsolete system functions that you never heard of and which you are never supposed to change or alter !