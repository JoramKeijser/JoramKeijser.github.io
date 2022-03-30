---
title: "Does evolution estimate the fitness gradient?"
---

## Does evolution estimate the fitness gradient?

<p align="center">
<img src="/images/evo_double.gif" style="background:none; border:none; box-shadow:none;">
</p>
<span class="caption" STYLE="font-size:85%"> Gradient-based optimisation is more efficient than gradient-free optimization, yet evolution by natural selection might be gradient free. </span>


Imagine you’re on a hike, trying to work your way up a mountain. Unfortunately, it is foggy, so you can see your immediate surroundings 
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
At least it doesn’t according to conventional wisdom in the field of evolutionary biology. For example, Futuyma and Kirkpatrick write in their authoritative textbook Evolution that: 

> Mutations are random with respect to what will improve survival and reproduction. New conditions do not increase the frequency of mutations that are beneficial in those conditions.

The randomness of mutations can come as a surprise to anyone familiar with gradient descent and its benefits. 
As Yann LeCun (not exactly just anyone when it comes to gradient descent) recently [wrote on Twitter](https://twitter.com/ylecun/status/1463140027517517824?s=20&t=SHbHtaUJuXu5LNQquuTsSA): 

> People who think evolution works through random mutations and selection need to explain how intelligent life appeared using nothing else. 
> Clearly, any optimization process is more efficient if it uses some sort of gradient estimation.

Here, we will investigate why textbooks do teach that evolution works through random mutations, but also why this might change in the light of recent experiments. We will start by reviewing the general role of mutations in the evolutionary process. 

## Mutations are the fuel for evolution

Since the start of the COVID pandemic, the world has been confronted with a steady stream of new virus variants, each spreading more rapidly than its predecessors. Where do these new variants come from? Mutations - mistakes in the copying of DNA - introduce changes in the viral genome. Some of these changes give the virus a fitness advantage, for example by making it more transmissible. If this advantage is big enough, the changed genome will come to dominate. 

<p align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/f/f3/Mutation_and_selection_diagram.svg" style="background:none; border:none; box-shadow:none;">
</p>
<span class="caption" STYLE="font-size:85%"> Source: [Wikipedia](https://commons.wikimedia.org/wiki/File:Mutation_and_selection_diagram.svg). </span>


The example of COVID evolution highlights the key steps that underlie evolution more generally. First, mutations introduce variation across a population. Next, natural selection picks out the variation that is best suited to the current environment. If natural selection is a car driving up a fitness hill, then mutations are the car’s fuel. But like a car’s fuel doesn’t influence its driving direction, mutations are thought to arise independently of their fitness effect. This introduces a clear separation between the two steps of evolution: improvements in fitness are only due to selection, not to mutations. The best we could hope for is lots of mutations, so that natural selection has enough variability to choose from. 

But here’s the catch: most mutations decrease an organism’s fitness. Intuitively, this is because organisms are very complex machines that have been optimised by evolution over a long period of time. And most changes to a complex, highly optimised machine diminish rather than improve its functioning. This seems to imply that mutations should occur at a very low rather than a high rate. And indeed, mutation rates *are* extremely low. In humans and other vertebrates, for example, on average only 1 base pair in every $10^8$ (hundred million) is replicated incorrectly. This means that the replication machinery could copy the Kandel, Schwartz & Jessel textbook almost 40 times without making a mistake![^2]

[^2]: Here I assumed an error rate of 1 in $10^8$ basepairs or characters, and 2,536,716 characters per book (estimated from the page count). This translates to an error rate of about 1 in 40 books - impressively accurate. But the replication machinery isn't just accurate, it is also very fast, with a speed on the order of 1000 basepairs per second. This means the machinery could copy the entire Kandel textbook in just over 7 minutes! 

<p align="center">
<img src="/images/kandel.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> (a) Mutation rates are larger for organisms with a large genome, (b) but even the mammalian replication machinery is so accurate that it could copy the Kandel textbook without any errors. (a) From Futuyma & Kirkpatrick, *Evolution*, 4th edition. </span>

Mutations therefore play a paradoxical role in evolution: they are necessary for adaptation, yet should generally be prevented. A solution to this paradox could come in the form of gene-specific mutation rates. Mutation rates should be low for essential genes such as those involved in metabolism, but they should be high for genes with an environmentally- dependent function, or for DNA that is not part of a gene at all. The potential advantages of gene-specific mutation rates provide a strong theoretical argument why mutation rates should be biased across the genome. But having a compelling theory is of course not enough - we also need to test it.  

## Testing for directed mutations is hard 
Unfortunately, testing for biased mutation rates is hard. The ideal experiment would reveal that certain parts of the genome mutate more than others, and that this difference is adaptive. Although many studies have shown that some genes show less variation than others, this doesn't imply a lack of mutations. Instead, a less variable gene could mutate just as often as other genes, but when it does, the mutation is eliminated by natural selection because it decreases fitness. 

Since natural variation is the outcome of both mutations and selection, detecting biased mutations requires observing mutations as they happen. But this is difficult because, as we’ve seen, mutations are very rare. And even if we did find that certain mutations occur more often than others - we still might not know their fitness effect, and therefore wouldn’t be able to tell if the mutational bias is aligned with the fitness gradient. 

Testing for directed mutations therefore requires a model organism that has a fast reproductive cycle, a mutation with clear fitness effects, and a way to determine if the mutation arose when it was beneficial. 

## Bacteria lead the way

<p align="center">
<img src="/images/luria.svg" style="background:none; border:none; box-shadow:none; "/>
</p>
<span class="caption" STYLE="font-size:85%"> Salvador Luria ([source](https://www.nobelprize.org/prizes/medicine/1969/luria/facts/
)), bacteriophages attacking an *E. coli* bacterium ([source](https://www.newyorker.com/tech/annals-of-technology/phage-killer-viral-dark-matter). </span>

The solution came to Salvador Luria one evening in 1943, as he was watching a colleague try their luck at a slot machine. Only three years ago, the Jewish Luria had fled France after the Nazi’s invaded the country, riding 800 kilometers from Paris to Marseille on his bicycle before obtaining a visa to the US. Since then, the 30 year old Luria had become obsessed with the origin of mutations. The issue had attracted many scientists before him, but progress had been slow due to the challenge of finding mutations and linking them to fitness. 

Luria had already overcome the first challenge by choosing the bacterium *Escherichia coli* as a model organism. Most of his colleagues did not consider lowly bacteria relevant for answering deep evolutionary questions. But Luria, a relative newcomer in the field, suspected something we now know to be true: many molecular mechanisms of evolution are shared across the tree of life, all the way from humans to bacteria. On top of this, *E. coli’s* 20 minute replication cycle allows a scientist to go from a single bacterium to millions in less than a day. That’s enough replication cycles for at least some mutations to occur. 

<p align="center">
<img src="/images/genomes.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> The *E.coli* bacterium, the *methanococcus* archean, and the human eukaryotes are separated by 
  hundreds of millions of years of evolution (top), yet share conserved genetic information (bottom). Source: Alberts et al. *Molecular Biology of the Cell*. </span>

Luria had also determined which of *E. coli’s* traits to study. Like other bacteria, *E. coli* is involved in a perpetual and intense arms race with bacteriophages (viruses that infect bacteria). One such virus is the T1 phage, which kills most *E. coli* bacteria. Most, but not all, because *E. coli* can gain resistance to T1 by a mutation that changes the receptor protein to which T1 attaches itself. Luria knew that *E.coli* could become T1 resistant, and that this resistance meant life or death during a T1 invasion - a clear fitness readout of the mutation!

Luria therefore decided to investigate if *E. coli’s* T1 resistance occurred spontaneously, or if it was induced by the presence of T1. If resistance was induced by T1, this would be consistent with directed evolution. But a key challenge still remained: how to tell the competing hypotheses apart? This is when Luria had his epiphany watching the slot machine. He realised that, like the slot machine, the outcome of his resistance experiments also followed the laws of probability theory. 

## A Nobel-prize winning experiment 

<p align="center">
<img src="/images/fluctuation_test.svg"  width="700" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> The competing hypotheses of induced and spontaneous mutations predict different numbers of surviving
bacteria. Source: [Wikipedia](https://en.wikipedia.org/wiki/Luria%E2%80%93Delbr%C3%BCck_experiment#/media/File:Luria-delbruck_diagram.svg
). </span>

First, suppose that resistance was induced by the presence of T1. In this case, each bacterium has a similar probability of gaining resistance, and this happens independently of other bacteria. If we expose many equally sized *E. coli* populations to T1, the law of large numbers predicts we can expect each population to have similar numbers of survivors. 

The spontaneous resistance model, on the other hand, predicts that the number of survivors across populations should show large fluctuations. The reason is that, if the mutation occurred before the presence of T1, this could have happened any time during the growth of the population. Had it occurred at the very start, most of the population should have inherited the resistance, and therefore would have survived. But had it occurred at the very end, only a few members would have survived. 

So here’s the experiment that Luria conducted. He started by putting single *E. coli* in tubes, and let them replicate until they had grown to large populations. Thanks to *E. coli’s* fast replication times, this took only a day. Then, he added T1 phages to the population, which invariably killed some of the bacteria, while others survived. Crucially though, the number of bacteria that survived varied by several orders of magnitude across different populations. In the colonies with many survivors, T1 resistance must have occurred long before the T1 exposure, rather than in response to the T1 presence. 

Their findings earned Luria (along with Max Delbruck and Alfred Hershey) the 1969 Nobel prize in Physiology or Medicine. By then, other experiments had supported the conclusion that mutations occur independently of their fitness effects, establishing it as a central principle of evolutionary genetics.[^3]


[^3]: One of the first follow-up studies was the elegant replica plate experiment from Joshua and Esther Lederberg. You can read about it [here](https://evolution.berkeley.edu/the-lederberg-experiment/).  

But like the original work from Luria and Delbruck, the follow-up studies finding random mutations were held back by a major technological limitation: they couldn’t detect mutations across multiple genes. At the time of their key experiment, Luria and Delbruck didn’t even know what a gene exactly was! This meant that only those “genes” could be studied that had a clear fitness effect. But most genes, we know now, affect fitness in only a very modest way. This meant that the unbiased study of mutation rates had to wait until the postgenomic era. 

## Genomic clues of non-random mutation rates

In the public perception, the postgenomic era began with the completion of the human genome project, around 20 years ago. But by then, the postgenomic era was already in full swing for certain bacterial species. One of them is *E. coli*, whose genome was sequenced in 1997. In 2012, [Martincorena et al.](https://doi.org/10.1038/nature10995) compared *E. coli* genomes to test for biased mutation rates. They couldn’t simply compare the diversity across genes, since this is shaped by both mutations and selection. So instead, they focused on synonymous diversity: those variations in DNA that do not affect the gene product[^4]. Whereas genes with smaller diversity across organisms might simply be under stronger purifying selection, genes with smaller *synonymous* diversity actually must have a smaller mutation rate.

[^4]: The technical definition of synonymous definition is slightly different, but conceptually similar. See the accessibly written original paper
from [Martincorena et al.](https://doi.org/10.1038/nature10995). 

The authors found that the synonymous diversity varies 20-fold across the *E. coli* genome, with certain genes showing much less diversity than expected from a uniform mutation rate, and others showing much more. And this pattern correlated with function: the diversity was smallest for essential genes (those critical for healthy function) and those under strong purifying selection. Martincorena et al. therefore concluded that the mutation rate varies across the genome, and that this might constitute an “evolutionary risk management strategy”. 

<p align="center">
<img src="/images/martincorena.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> (a) Mutation rates (estimated using the synonymous diversity across different strains) vary along the E. coli genome. Red bars indicate 95% confidence interval under a uniform mutation rate. (b) Genes with lower mutation rates are more often essential From [Martincorena et al.](https://doi.org/10.1038/nature10995). </span>

This finding made a splash in the world of evolutionary biology, but soon researchers started to point to several potential problems. One problem was theoretical: mutations to individual genes occur so rarely that the advantage of an even lower and gene-specific mutation rate is too small to evolve, especially since mutations to individual genes typically only have a modest fitness effect (see [Chen & Zhang](https://doi.org/10.1093/molbev/mst060), more on this later). Other challenges were empirical in nature (see, e.g. [Maddamsetti et al.](https://doi.org/10.1093/molbev/msv161)). For example, Martincorena et al. suggested but did not provide evidence for the mechanism underlying non-random mutations. Finally, they aimed to eliminate the effect of selection by analysing only synonymous genetic diversity, but might have failed to account for all fitness effects. 

Definitive evidence for biased mutations therefore required a stronger, experimental rather than statistical, control for selection. 

## Planting the seeds of a revolution

<p align="center">
<img src="/images/mutation_accumulation.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> A typical mutation accumulation (MA) experiment, in which n lines are randomly bred for t generations. </span>

A rigorous but time-consuming approach to experimentally minimise the effect of selection is a so-called mutation accumulation experiment. In such an experiment, the experimenter randomly selects the organism that will give rise to the next generation, making reproductive success independent of fitness. Repeating this process for many generations results in the accumulation of mutations untouched by selection. Doing this in parallel for several lines allows us to see if certain mutations arise more frequently than others. 

Recently, [Monroe et al.](https://doi.org/10.1038/s41586-021-04269-6) used mutation accumulation lines to investigate the randomness of mutations in *Arabidopsis thaliana* (the primary model for plant genetics). They found that genetic variability across mutation accumulation lines greatly varied across the genome, being reduced by a half in gene bodies. This time around, the reduced variability really could not be due to selection, and therefore had to be attributed to bias in the mutation rate. 


<p align="center">
<img src="/images/monroe.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> (a): Arabidopsis thaliana. (b): mutations are lower for bases within gene bodies. TSS: transcription start sites, TTS:  transcription termination sites.  (c): the mutation rate varies with gene function. Adapted from [Monroe et al.](https://doi.org/10.1038/s41586-021-04269-6) </span>

That still leaves open the mechanism through which the bias could occur. Monroe et al. also address this question by showing that the mutation rate of a genomic region can be predicted from several of its features. Some of these features are epigenomic, i.e. they affect gene expression without changing the DNA itself, for example by changing the histone proteins that package DNA. This change can increase or decrease the genes’ expression by making it more or less accessible to transcription factors. Interestingly, such a change in a gene’s packaging can also increase the accuracy of its replication (see e.g. [Chong et al.](https://doi.org/10.1038/s41467-020-14595-4)). This suggests that natural selection could tune mutation rates by shaping DNA packaging around important genes. 

Finally, the authors also show evidence that the mutation rate bias is adaptive: essential genes, critical for healthy function such as metabolism, mutate less than genes with an environmentally-dependent function. This means that the variation in mutation rate reduces the occurrence of deleterious mutations, and could direct the course of evolution just as well as natural selection. 


## Synthesis: how specific are mutation rates? 
The recent findings from Martincorena and Monroe provide, I think, strong evidence that evolution can tune local mutation rates. So why didn't Luria and Delbruck find that T1 presence increased the likelihood of mutations, when this clearly would have been adaptive? Did they and all these other classic experiments overlook something? Did they simply look at the wrong genes? [Perhaps](https://doi.org/10.1088/1478-3975/aa8230), but several alternative explanations seem more likely. 

## Conclusion

## Acknowledgements



