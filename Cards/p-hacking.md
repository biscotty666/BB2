up:: [[Probability and Statistics]]
tags:: #on/PnS #source/video #source/StatQuest #note/reference 

## p-hacking

Refers to mis-interpreting or mis-using p values so as to generate _false positives_.

One type of p-hacking is called the _Multiple Testing Problem_. This refers to repeating a test over and over until one gives a p-value < 0.05. By definition, in a normal distribution 5% of the data will have a p-value < 0.05 and so would be False Positives. So results should include the p-values for all the tests, not selected ones. __No cherry picking__.

One way to compensate is called the [[False Discovery Rates]].

Another type of p-hacking involves adding more observations to a sample to reduce the p-value. Adding additional observations has a very high probability of reduce the p-value to below 0.05, resulting in a false positive. So sample size should always be determined _before_ taking samples.

[[Power analysis]] can be used to determine an appropriate sample size.


---

### References
[p-hacking: What it is and how to avoid it! - YouTube](https://www.youtube.com/watch?v=HDCOUXE3HMM)
[Statistics Fundamentals - YouTube](https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9)