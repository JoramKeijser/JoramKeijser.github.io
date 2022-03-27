---
title: "Does evolution estimate the fitness gradient?"
---

## Does evolution estimate the fitness gradient?

<p align="center">
<img src="/images/evo_double.gif" style="background:none; border:none; box-shadow:none;">
</p>
<span class="caption" STYLE="font-size:85%"> Gradient-based optimisation is more efficient than gradient-free optimization, yet evolution by natural selection might be gradient free. </span>


Imagine you’re on a hike, trying to work your way up a mountain. Unfortunately, it is a foggy day, so you can see your immediate surroundings 
but not the mountain top. In which direction do you decide to walk? Your intuition probably tells you to choose the direction in which the 
terrain rises most rapidly. And this intuition makes sense: without knowledge about the overall landscape, the immediate direction of steepest
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

## Mutations are the fuel for evolution

Since the start of the COVID pandemic, the world has been confronted with a steady stream of new virus variants, each spreading more rapidly than its predecessors. Where do these new variants come from? Mutations - mistakes in the copying of DNA - introduce changes in the viral genome. Some of these changes give the virus a fitness advantage, for example by making it more transmissible. If this advantage is big enough, the changed genome will come to dominate. 

<p align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/f/f3/Mutation_and_selection_diagram.svg" style="background:none; border:none; box-shadow:none;">
</p>
<span class="caption" STYLE="font-size:85%"> Source: [Wikipedia](https://commons.wikimedia.org/wiki/File:Mutation_and_selection_diagram.svg). </span>


The example of COVID evolution highlights the key steps that underlie evolution more generally. First, mutations introduce variation across a population. Next, natural selection picks out the variation that is best suited to the current environment. If natural selection is a car driving up a fitness hill, then mutations are the car’s fuel. But like a car’s fuel doesn’t influence its driving direction, mutations are thought to arise independently of their fitness effect. So there’s a clear separation between the two steps of evolution: improvements in fitness are only due to selection, not to mutations. The best we could hope for is lots of mutations, so natural selection has enough variability to choose from. 

But here’s the catch: most mutations decrease an organism’s fitness. Intuitively, this is because organisms are very complex machines that have been optimised by evolution over a long period of time. And most changes to a complex, highly optimised machine diminish rather than improve its functioning. This seems to imply that mutations should occur at a very low rather than a high rate. And indeed, mutation rates are extremely low. In humans and other vertebrates, for example, on average only 1 base pair in every 100.000.000 (100 million) is replicated incorrectly. This means that the replication machinery could copy the Kandel & Schwartz textbook 40 times without making a mistake! 


<p align="center">
<img src="https://pbs.twimg.com/media/ExsPTtQWUAIbHUt?format=jpg&name=large" style="background:none; border:none; box-shadow:none;" width="400"/>
</p>
<span class="caption" STYLE="font-size:85%"> Source: [@LecyEmily](https://twitter.com/LecyEmily/status/1376710949239975939?s=20&t=yedkQIMZNGSM9oMcMT2_Xg). </span>

Mutations therefore play a paradoxical role in evolution: they are necessary for adaptation, yet should generally be prevented. A solution to this paradox could come in the form of gene-specific mutation rates. Mutation rates should be low for essential genes such as those involved in metabolism, but they should be high for genes with an environmentally- dependent function, or for DNA that is not part of a gene at all. The potential advantages of gene-specific mutation rates provide a strong theoretical argument why mutation rates should be biassed across the genome. But having a compelling theory is of course not enough - we also need to test it.  

## Testing for directed mutations is hard 
Unfortunately, testing for biassed mutation rates is hard. The ideal experiment would reveal that certain parts of the genome mutate more than others, and that this difference is adaptive. And many studies have shown that certain parts of the human genome indeed show less variation than other parts. But a lack of variation in a certain gene doesn't necessarily result from a lack of mutations. Instead, the gene could mutate just as often as other genes, but when it does, this mutation is eliminated by natural selection because it decreases fitness. 

Since natural variation is the outcome of both mutations and selection, detecting biassed mutations requires observing mutations as they happen. But this is difficult, because, as we’ve seen, mutations are very rare. And even if we did find that certain mutations occur more often than others - we still might not know their fitness effect, and therefore wouldn’t be able to tell if the mutational bias is aligned with the fitness gradient. 

Testing for directed mutations therefore requires a model organism that has a fast reproductive cycle, a mutation with clear fitness effects, and a way to determine if the mutation arose when it was beneficial. 

## Bacteria lead the way

<p align="center">
<img src="/images/luria.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> Salvador Luria ([source](https://www.nobelprize.org/prizes/medicine/1969/luria/facts/
)), bacteriophages attacking an E. coli bacterium ([source](https://www.newyorker.com/tech/annals-of-technology/phage-killer-viral-dark-matter), and a slot machine ([source](https://en.wikipedia.org/wiki/File:MK_Shangri-La.jpg)). </span>

The solution came to Salvador Luria one evening in 1943, as he was watching a colleague try their luck at a slot machine. Only three years ago, the Jewish Luria had fled France after the Nazi’s invaded the country, riding 800 kilometers from Paris to Marseille on his bicycle before obtaining a visa to the US. Since then, the 30 year old Luria had become obsessed with the origin of mutations. The issue had attracted many scientists before him, but progress had been slow due to the challenge of finding mutations and linking them to fitness. 

Luria had already overcome the first challenge by choosing the bacterium Escherichia coli as a model organism. Most of his colleagues did not consider lowly bacteria relevant for answering deep evolutionary questions. But Luria, a relative newcomer in the field, suspected something we now know to be true: many molecular mechanisms of evolution are shared across the tree of life, all the way from humans to bacteria. On top of this, E. coli’s 20 minute replication cycle allows a scientist to go from a single bacterium to millions in less than a day. That’s enough replication cycles for at least some mutations to occur. 

Luria had also determined which of E. coli’s traits to use. Like other bacteria, E. coli is involved in a perpetual and intense arms race with bacteriophages (viruses that infect bacteria). One such virus is the T1 phage, which will kill most E. coli bacteria. Most, but not all, because E. coli can gain resistance to T1 by a mutation that changes the receptor protein to which T1 attaches itself. Luria knew that E.coli could become T1 resistant, and that this resistance meant life or death during a T1 invasion - a clear fitness readout of the mutation!

Luria therefore decided to investigate if E. coli’s T1 resistance occurred spontaneously, or if it was induced by the presence of T1. If resistance was induced by T1, this would be consistent with directed evolution. But a key challenge still remained: how to tell the competing hypotheses apart? This is when Luria had his epiphany watching the slot machine. He realised that, like the slot machine, the outcome of his resistance experiments also followed the laws of probability theory. 

## A Nobel-prize winning experiment 

<p align="center">
<img src="/images/fluctuation_test.svg"  width="700" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> The competing hypotheses of induced and spontaneous mutations predict different numbers of surviving
bacteria. Source: [Wikipedia](https://en.wikipedia.org/wiki/Luria%E2%80%93Delbr%C3%BCck_experiment#/media/File:Luria-delbruck_diagram.svg
). </span>

First, suppose that resistance was induced by the presence of T1. In this case, each bacterium has a similar probability of gaining resistance, and this happens independently of other bacteria. If we expose many equally sized E. coli populations to T1, the law of large numbers predicts we can expect each population to have similar numbers of survivors. 

The spontaneous resistance model, on the other hand, predicts that the number of survivors across populations should show large fluctuations. The reason is that, if the mutation occurred before the presence of T1, this could have happened any time during the growth of the population. Had it occurred at the very start, most of the population should have inherited the resistance, and therefore would have survived. But had it occurred at the very end, only a few members would have survived. 

So here’s the experiment that Luria conducted. He started by putting single E. coli in tubes, and let them replicate until they had grown to large populations. Thanks to E. coli’s fast replication times, this took only a day. Then, he added T1 phages to the population, which invariably killed some of the bacteria, while others survived. Crucially though, the number of bacteria that survived varied by several orders of magnitude across different populations. In the colonies with many survivors, T1 resistance must have occurred long before the T1 exposure, rather than in response to the T1 presence. 

Their findings earned Luria (along with Max Delbruck and Alfred Hershey) the 1969 Nobel prize in Physiology or Medicine. By then, other experiments had supported the conclusion that mutations occur independently of their fitness effects, establishing it as a central principle of evolutionary genetics. (One of the first follow-up studies was the elegant replica plate experiment from Joshua and Esther Lederberg. You can read about it here) 

But like the original work from Luria and Delbruck, the follow-up studies finding random mutations were held back by a major technological limitation: they couldn’t detect mutations across many genes. At the time of their key experiment, Luria and Delbruck didn’t even know what a gene exactly was! This meant that only those “genes” could be studied that had a clear fitness effect. But most genes, we know now, affect fitness in only a very modest way. This meant that further study of mutation rates had to wait until the postgenomic era. 

## Genomic clues of non-random mutation rates

<p align="center">
<img src="/images/martincorena.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> (a) Mutation rates (estimated using the synonymous diversity across different strains), vary along the E. coli genome. Red bars indicate 95% confidence interval under a uniform mutation rate. (b) Genes with lower mutation rates are more often essential From [Martincorena et al.](https://doi.org/10.1038/nature10995). </span>

## Planting the seeds of a revolution

<p align="center">
<img src="/images/monroe.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> Left: Arabidopsis thaliana. Middle: mutations are lower for bases within gene bodies. TSS: transcription start sites, TTS:  transcription termination sites.  Right: the mutation rate varies with gene function. </span>

## Reconciling Luria-Delbruck with genomic data

## Conclusion


