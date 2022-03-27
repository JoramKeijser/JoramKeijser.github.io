## Does evolution estimate gradients?
Well, does it? 

<img src="/images/evo_double.gif" style="background:none; border:none; box-shadow:none;">
<span class="caption" STYLE="font-size:85%"> Optimisation is most efficient when it generates candidate solutions using gradient information, which is what gradient-descent does. Yet evolution by natural selection isn't traditionally believed to exploit this idea. </span>


Imagine you’re on a hike, trying to work your way up a mountain. Unfortunately, it is a foggy day, so you can see your immediate surroundings 
but not the mountain top. In which direction should you decide to walk? Your intuition probably tells you to choose the direction in which the 
terrain rises most rapidly. And this intuition makes sense: without knowledge about the overall landscape, the immediate d. irection of steepest
ascent is the most promising[^1]. 

[^1]: According to my partner, who is Swiss, hiking in foggy mountains can actually be dangerous. So you should probably head home rather than up.

The same intuition underlies many optimisation algorithms, including those that power deep neural networks. 
Instead of choosing a hiking direction, the algorithm chooses network parameters,  and instead of trying to get up a mountain, 
it tries to improve the network’s performance. But the basic idea is the same: make a change in the direction of steepest improvement. 
Mathematically, this direction is known as the gradient of the objective function, and the overall approach is known as gradient ascent 
(or descent, if we’re dealing with an objective like a cost that should be minimised). Optimization algorithms that don’t use the gradient are 
typically less efficient than algorithms that do, and deep learning would not enjoy its current success without it. 

Yet, evolution by natural selection – arguably the most impressive optimisation algorithm we know – does not exploit this powerful principle. 
At least it doesn’t according to the textbook knowledge in the field of evolutionary biology, which says that genetic mutations arise independently of 
their fitness effects. Such undirected or “random" mutations can come as a surprise to anyone familiar with gradient descent and its benefits. 
As Yann LeCun (not exactly just anyone when it comes to gradient descent) recently [wrote on Twitter](https://twitter.com/ylecun/status/1463140027517517824?s=20&t=SHbHtaUJuXu5LNQquuTsSA): 

> "People who think evolution works through random mutations and selection need to explain how intelligent life appeared using nothing else. 
> Clearly, any optimization process is more efficient if it uses some sort of gradient estimation."

Here, we will investigate why many biologists do believe that evolution works through random mutations, 
but also why recent experiments might overturn this idea. We will start by familiarising ourselves with the general role of mutations 
in the evolutionary process. 
