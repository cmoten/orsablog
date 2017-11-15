+++
date = 2017-11-10T00:00:00  # Schedule page publish date.

title = "The Secretary Problem"
time_start = 2017-11-10T00:00:00
time_end = 2017-11-10T00:00:00
abstract = ""
abstract_short = ""
event = ""
event_url = ""
location = ""
authors = ["Cardy Moten III"]

# Is this a selected talk? (true/false)
selected = false

# Projects (optional).
#   Associate this talk with one or more of your projects.
#   Simply enter the filename (excluding '.md') of your project file in `content/project/`.
projects = ["probability-modeling"]

# Links (optional).
url_pdf = ""
url_slides = ""
url_video = ""
url_code = ""

# Does the content use math formatting?
math = true

# Does the content use source code highlighting?
highlight = true

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "headers/secretary.png"
caption = "Source: [Data Genetics](http://datagenetics.com/blog/december32012/index.html)"

+++

Being an ORSA is not unlike any other job on the command staff. That is, you will get a question to answer, and depending on your experience level and the complexity of the issue, you can sometimes give a quick response, or you may need more time to provide a thorough answer and explanation. The problem that follows in this white paper is an example of the type of problem an ORSA would deal with in their line of work. In particular, this question deals with determining the probability of an event, given specific pieces of information. The approach used to solve this problem is a method based on the [secretary problem](http://datagenetics.com/blog/december32012/index.html). The example that follows is an excellent problem to show not only an application of probability theory but how to solve a problem from multiple approaches. 

For this problem, the ORSA can derive the solution with a couple of formulas.  While it is quick just to look up a formula and give an answer, it is crucial for ORSAs to understand how to gain an intuition for the mathematics they are using, so that they can translate their intuition into an appropriate analogy for the person who is receiving the final results. To put it another way, an ORSA needs to develop an ability to translate the findings of their analysis into a lexicon understood by the consumers of the report. 

## Motivation

The US Physical Fitness Center and School (USAPFCS) is researching to select a new Army Physical Fitness Uniform. Based on their request for proposal, the USAPFCS received 50 different uniform designs that meet the requirement, and they will need to choose
the best prototype before moving to the next phase of the acquisition process. Before deciding on the material solution to develop, the Program Manager for the uniform selection team wants to know the probability of selecting the best submission out of the 50 received since they are working with a limited budget and can only proceed with one material solution to develop.

## Analysis

After some initial research, the procurement team ORSA determined the best model to use is one based on the secretary problem. That is the procurement team should:

1. Review an initial set of $k$ proposals out of the $n=50$ total proposals without making a selection. To ensure a fair assessment is given to each proposal, they are shuffled into a random order before the initial evaluation;
2. Review the remaining $n-k$ proposals and choose the first proposal that is better than the
previous set of proposals;
3. If they make it to the last proposal, then by default this will be the selected proposal.

### Analytical Solution

There are two simple formulas to find the optimal selection probability. First, the number of initial candidates to review is approximately equal to $n/e$. In other words, if you take the total number of proposals and divide it by the special constant $e$, then you will get a reasonable initial sample in which to review the remaining proposals. In this example, the initial number of candidates to review is $\left\lfloor50/e\right\rfloor = 18$.[^1] 

[^1]:The $\left\lfloor \right\rfloor$ symbol represents the floor function, and it means to take the result of the evaluated expression and round to the nearest integer that is less than or equal to the usual result of the evaluated expression. The mathematical constant, $e$, is also referred to as [Euler's number](http://www.mathsisfun.com/numbers/e-eulers-number.html). 

Now to find the probability of selecting the best candidate, all that is left is to use the formula $$-\left(\frac{k}{n}\right)\ln\left(\frac{k}{n}\right)$$

Using this formula produces the following solution:
$$
\begin{aligned}
\text{selection probability} & = -\left(\frac{k}{n}\right)\ln\left(\frac{k}{n}\right) \\\\\\
\phantom{\vspace} \\\\\\
\text{selection probability} & = -\left(\frac{18}{50}\right)\ln\left(\frac{18}{50}\right) \\\\\\
\phantom{\vspace} \\\\\\
\text{selection probability} & = 0.3677944
\end{aligned}
$$

Before going back to the procurement team with the answer, the ORSA needs to make sure they can answer the question of why this is the correct answer. One way to do this is to create a plot showing the relationship between the number of initial candidates to evaluate $k$ and the probability of optimal selection. <a href="#figure1">Figure 1</a> below shows this link, and the red dot indicates the maximum probability. Primarily, as the number of proposals in your initial rejection region increases, the chance of selecting the ideal candidate increases as well until you reached a maximum at 18 proposals. After 18 proposals, the probability of choosing the best proposal decreases.

<a id="figure1"></a> {{< figure src="/img/secplot.jpg" title="Plot showing the optimal selection probability versus the number of initial candidates reviewed." >}} 



The result should make sense because, before 18 proposals, we don't have enough information to determine which bid is indeed the best one. Along the same lines, after 18 proposals, we reviewed more proposals than necessary to decide on the optimal probability. Feeling satisfied with this logic, the ORSA would then present their findings to the procurement team, and life is good. However, there are some occasions when just giving this amount of information is not enough. For example, a procurement team member may want to know more about why 18 is the appropriate number. While they can see the graphic that shows this, their intuition is not sufficient to grasp the presented formulation. 

Now the ORSA could deal with this in various ways. One approach would involve deriving the formula, similar to the process shown by  [Data Genetics](http://datagenetics.com/blog/december32012/index.html). I would not recommend this method unless you are convening with a bunch of people that have an overwhelming enjoyment of math. To put it another way, most of the consumers of our analysis don't share that type of enthusiasm as we do. Therefore, you will need to find a way to report just enough information to build trust with the results without delving into the abstract details on a subject that your message gets lost. 

### Simulate the process to develop intuition

A better approach would involve creating a less complicated example and then gradually increase the complexity in a way that is more intuitive. A simulation, of the selection process, for instance, could guide the intuition of why the formula works. Because the simulation closely resembles the selection process, it will be easier to see how the formula reduces to its final form. 

#### A minimal example

To understand, how the simulation works, let's examine a simple example. The table below shows the results of reviewing three proposals with a strategy to evaluate and reject the first proposal and select the next proposal that scores higher than the first one. The rows indicate the final rating for each proposal, with a score of one for the worst proposal and three for the best proposal. The boldface numbers are the selected proposals by the selection strategy, and the red highlighted selections were instances of choosing the highest rated proposal. 

In essence, the simulation will create a random sample of scores that has a sample size equal to the total number of bids to evaluate. This example shows three, while the simulation we will use has a sample size of 50. After we create a random, sample, the simulation will enforce the rules of rejecting the initial number of candidates and then selecting the first proposal better than the previously seen proposals. If the simulation reaches the last proposal, then that proposal is chosen by default. After collecting all the winning bids, the simulation will compute the probability of selecting the best candidate by dividing the total number of times the highest proposal was selected divided by the total number of trials simulated. As indicated by the table, this simulation produced an optimal selection probability of $50\%$. 


|   |     |     |
|---|-----|-----|
| 1 |**2**|  3  |
| 1 | `3` |  2  |
| 2 |  1  | `3` |
| 2 | `3` |  1  |
| 3 |  1  |**2**|
| 3 |  2  |**1**|

<caption>**Table 1:** Example simulation results</caption>

 



#### Simulation results 

<a href="#figure2">Figure 2</a> displays the results of simulating the selection of candidate proposals, with an initial review sample ranging from 1 to 49 samples, for 100, 1,000, and 100,000 trials respectively. The figure also contains a plot of the analytical solution shown in <a href="#figure1">Figure 1</a>.  Thus, what the figure shows that as the number of trials increase, the simulation results will start to closely mimic the results of the analytic solution. To put this another way, the analytical formula is what the simulation would return with an infinite number of trials. Table 2 shows a summary of this described pattern.  


<a id="figure2"></a>{{< figure src="/img/simulation-results.png" title="Simulation results for 100 trials." >}}

| Number of trials|Number of inital candidates to review| Optimal probability|
|-----------------|-------------------------------------|--------------------|
| 100             | 12, 15, 21, 29                      | 0.400              |
| 1,000           | 20                                  | 0.383              |
| 100,000         | 19                                  | 0.375              |
| $\infty$        | 18                                  | 0.368              |

<caption>**Table 2:** Comparison of simulation results</caption>

## Conclusion

This white paper demonstrated how to apply a probability model towards selecting the best proposal from a pool of 50 proposals. Using a simple formula solved the problem, but we also learned that besides solving the problem, an ORSA must have a good intuitive sense of what the formula is doing. For an ORSA, that would usually involve diving into the background theory of the formula, but doing that process for a non-technical audience is usually not the best approach. A better approach would be to use a simple simulation, or similar technique, that would show what is happening underneath the hood of the formula. Ultimately, this helps to build trust with the ORSA's analysis.

## Futher learning

If you want to learn more about the secretary problem in general, then read [Thomas Ferguson's](https://www.math.upenn.edu/~ted/210F10/References/Secretary.pdf) paper on the history of this problem.

The YouTube video below presents Dr. Ria Symonds and her application of the secretary problem to selecting the best port-o-potty during a music festival.

{{< youtube ZWib5olGbQ0 >}}
