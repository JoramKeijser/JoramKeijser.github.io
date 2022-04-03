---
title: "Does evolution estimate the fitness gradient?"
---

## Does evolution estimate the fitness gradient?

<p align="center">
<img src="/images/mutations/evo_double.gif" style="background:none; border:none; box-shadow:none;">
</p>
<span class="caption" STYLE="font-size:85%"> Gradient-based optimization is more efficient than gradient-free optimization. Both optimization algorithms minimise the same quadratic loss function. Color indicates performance or (inverse) fitness </span>


Imagine you’re on a hike, trying to work your way up a mountain. Unfortunately, it is foggy, so you can see your immediate surroundings 
but not the mountain top. In which direction do you decide to walk? Your intuition probably tells you to choose the direction in which the 
terrain around you rises most rapidly. And this intuition makes sense: without knowledge about the overall landscape, the immediate direction of steepest
ascent is the most promising[^1]. 

[^1]: According to my partner, who is Swiss, hiking in foggy mountains can actually be dangerous. So you should probably head home rather than up.

The same intuition underlies many optimisation algorithms, including those that power deep neural networks. 
Instead of choosing a hiking direction, the algorithm chooses network parameters,  and instead of trying to get up a mountain, 
it tries to improve the network’s performance. But the basic idea is the same: make a change in the direction of steepest improvement. 
Mathematically, this direction is known as the gradient of the objective function, and the overall approach is known as gradient ascent 
(or descent, if we’re dealing with an objective like a cost that should be minimised). Optimization algorithms that don’t use the gradient are 
typically less efficient than algorithms that do, and deep learning would not enjoy its current success without it. 

Yet, evolution by natural selection – arguably the most impressive optimisation algorithm we know – does not exploit this powerful principle. 
At least it doesn’t according to conventional wisdom in the field of evolutionary biology. For example, Futuyma and Kirkpatrick write in their authoritative textbook *Evolution*: 

> Mutations are random with respect to what will improve survival and reproduction. New conditions do not increase the frequency of mutations that are beneficial in those conditions.

The randomness of mutations can come as a surprise to anyone familiar with gradient descent and its benefits. 
As Yann LeCun (not exactly just anyone when it comes to gradient descent) recently [wrote on Twitter](https://twitter.com/ylecun/status/1463140027517517824?s=20&t=SHbHtaUJuXu5LNQquuTsSA): 

> People who think evolution works through random mutations and selection need to explain how intelligent life appeared using nothing else. 
> Clearly, any optimization process is more efficient if it uses some sort of gradient estimation.

Here, we will investigate why textbooks still teach that evolution works through random mutations, but also why this might change in the light of recent experiments. We will start by reviewing the general role of mutations in the evolutionary process. 

## Mutations are the fuel for evolution

Since the start of the COVID pandemic, the world has been confronted with a steady stream of new virus variants, each spreading more rapidly than its predecessors. Where do these new variants come from? Mutations - mistakes in the copying of DNA - introduce changes in the viral genome. Some of these changes give the virus a fitness advantage, for example by making it more transmissible. If this advantage is big enough, the changed genome will come to dominate. 

<p align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/f/f3/Mutation_and_selection_diagram.svg" style="background:none; border:none; box-shadow:none;">
</p>
<span class="caption" STYLE="font-size:85%"> Source: [Wikipedia](https://commons.wikimedia.org/wiki/File:Mutation_and_selection_diagram.svg). </span>


The example of COVID evolution highlights the key steps that underlie evolution more generally. First, mutations introduce variation across a population. Next, natural selection picks out the variation that is best suited to the current environment. If natural selection is a car driving up a fitness hill, then mutations are the car’s fuel. But like a car’s fuel doesn’t influence its driving direction, mutations are thought to arise independently of their fitness effect. This introduces a clear separation between the two steps of evolution: improvements in fitness are only due to selection, not to mutations. The best we could hope for is lots of mutations, so that natural selection has enough variability to choose from. 

But here’s the catch: most mutations actually decrease an organism’s fitness. Intuitively, this is because organisms are very complex machines that have been optimised by evolution over a long period of time. And most changes to a complex, highly optimised machine diminish rather than improve its functioning. This seems to imply that mutations should occur at a very low rather than a high rate. And indeed, mutation rates *are* extremely low. In humans and other vertebrates, for example, on average only 1 base pair in every $10^8$ (hundred million) is replicated incorrectly. This means that the replication machinery could copy the famous Kandel, Schwartz & Jessel textbook almost 40 times without making a mistake![^2]

[^2]: Here I assumed an error rate of 1 in 10^8 base pairs or characters, and 2,536,716 characters per book (estimated from the page count). This translates to an error rate of about 1 in 40 books - impressively accurate. But the replication machinery isn't just accurate, it is also very fast, with a speed on the order of 1000 base pairs per second. This means the machinery could copy the entire Kandel textbook in just over 7 minutes! 

<p align="center">
<img src="/images/mutations/kandel.svg"  width="800" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> Mutation rates are larger for organisms with a large genome, but even the mammalian replication machinery is so accurate that it could copy the Kandel textbook without any errors. Bp: base pair. (a) From Futuyma & Kirkpatrick, *Evolution*, 4th edition. </span>

Mutations therefore play a paradoxical role in evolution: they are necessary for adaptation, yet should generally be prevented. A solution to this paradox could come in the form of gene-specific mutation rates. Mutation rates should be low for essential genes such as those involved in metabolism, but they should be high for genes with an environmentally- dependent function, or for DNA that is not part of a gene at all. The potential advantage provides a strong theoretical argument why mutation rates should be biased across the genome. But having a compelling theory is of course not enough - we also need to test it.  

## Testing for directed mutations is hard 
Unfortunately, testing for biased mutation rates is hard. The ideal experiment would reveal that (1) certain parts of the genome mutate more than others, and (2) this difference is adaptive. Although many studies have shown that some genes show less variation than others, this doesn't imply a lack of mutations. Instead, a less variable gene could mutate just as often as other genes, but when it does, the mutation is eliminated by natural selection because it decreases fitness. 

Since natural variation is the outcome of both mutations and selection, detecting biased mutations requires observing mutations as they happen. But this is difficult because, as we’ve seen, mutations are very rare. And even if we did find that certain mutations occur more often than others - we still might not know their fitness effect, and therefore wouldn’t be able to tell if the mutational bias is aligned with the fitness gradient. 

Testing for directed mutations therefore requires a model organism that has a fast reproductive cycle, a mutation with clear fitness effects, and a way to determine if the mutation arose when it was beneficial. 

## Bacteria lead the way

<p align="center">
<img src="/images/mutations/luria.svg" style="background:none; border:none; box-shadow:none; "/>
</p>
<span class="caption" STYLE="font-size:85%"> Salvador Luria ([source](https://www.nobelprize.org/prizes/medicine/1969/luria/facts/
)), bacteriophages attacking an *E. coli* bacterium ([source](https://www.newyorker.com/tech/annals-of-technology/phage-killer-viral-dark-matter). </span>

The solution came to Salvador Luria one evening in 1943, as he was watching a colleague try their luck at a slot machine. Only three years ago, the Jewish Luria had fled France after the Nazi’s invaded the country, riding 800 kilometers from Paris to Marseille on his bicycle before obtaining a visa to the US. Since then, the 30 year old Luria had become obsessed with the origin of mutations. The issue had attracted many scientists before him, but progress had been slow due to the challenge of finding mutations and linking them to fitness. 

Luria had already overcome the first challenge by choosing the bacterium *Escherichia coli* as a model organism. Most of his colleagues did not consider lowly bacteria relevant for answering deep evolutionary questions. But Luria, a relative newcomer in the field, suspected something we now know to be true: many molecular mechanisms of evolution are shared across the tree of life, all the way from humans to bacteria. On top of this, *E. coli’s* 20 minute replication cycle allows a scientist to go from a single bacterium to millions in less than a day. That’s enough replication cycles for at least some mutations to occur. 

Luria had also determined which of *E. coli’s* traits to study. Like other bacteria, *E. coli* is involved in a perpetual and intense arms race with bacteriophages (viruses that infect bacteria). One such virus is the T1 phage, which kills most *E. coli* bacteria. Most, but not all, because *E. coli* can gain resistance to T1 by a mutation that changes the receptor protein to which T1 attaches itself. Luria knew that *E.coli* could become T1 resistant, and that this resistance meant life or death during a T1 invasion - a clear fitness readout of the mutation!

Luria therefore decided to investigate if *E. coli’s* T1 resistance occurred spontaneously, or if it was induced by the presence of T1. If resistance was induced by T1, this would be consistent with directed mutations. But a key challenge still remained: how to tell the competing hypotheses apart? This is when Luria had his epiphany watching the slot machine. He realised that, like the slot machine, the outcome of his resistance experiments also followed the laws of probability theory. 

## A Nobel-prize winning experiment 

<p align="center">
<img src="/images/mutations/fluctuation_test.svg"  width="700" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> The Luria-Delbruck experiment, for which the competing hypotheses of induced and spontaneous mutations predict different numbers of surviving bacteria. Source: [Wikipedia](https://en.wikipedia.org/wiki/Luria%E2%80%93Delbr%C3%BCck_experiment#/media/File:Luria-delbruck_diagram.svg
). </span>

First, suppose that resistance was induced by the presence of T1. In this case, each bacterium has a similar probability of gaining resistance, and this happens independently of other bacteria. If we expose many equally sized *E. coli* populations to T1, the law of large numbers predicts we can expect each population to have similar numbers of survivors. 

The spontaneous resistance model, on the other hand, predicts that the number of survivors across populations should show large fluctuations. The reason is that, if the mutation occurred before the presence of T1, this could have happened any time during the growth of the population. Had it occurred at the very start, most of the population should have inherited the resistance, and therefore would have survived. But had it occurred at the very end, only a few members would have survived. 

So here’s the experiment that Luria conducted, in collaboration with the physicist Max Delbruck. He started by putting single *E. coli* in tubes, and let them replicate until they had grown to large populations. Thanks to *E. coli’s* fast replication times, this took only a day. Then, he added T1 phages to the population, which invariably killed some of the bacteria, while others survived. Crucially though, the number of bacteria that survived varied by several orders of magnitude across different populations. In those with many survivors, T1 resistance must have occurred long before the T1 exposure, rather than in response to the T1 presence. 

Their findings earned Luria (along with Max Delbruck and Alfred Hershey) the 1969 Nobel prize in Physiology or Medicine. By then, other experiments had supported the conclusion that mutations occur independently of their fitness effects, establishing it as a central principle of evolutionary genetics[^3].


[^3]: One of the first follow-up studies was the elegant replica plate experiment from Joshua and Esther Lederberg. You can read about it [here](https://evolution.berkeley.edu/the-lederberg-experiment/).  

But like the original work from Luria and Delbruck, the follow-up studies finding random mutations were held back by a major technological limitation: they couldn’t detect mutations across multiple genes. At the time of their key experiment, Luria and Delbruck didn’t even know what a gene exactly was! This meant that only those “genes” could be studied that had a clear fitness effect. But most genes, we know now, affect fitness in only a very modest way. The unbiased study of mutation rates therefore had to wait until the postgenomic era. 

## Genomic clues of non-random mutation rates

In the public perception, the postgenomic era began with the completion of the human genome project around 20 years ago. But by then, the postgenomic era was already in full swing for certain bacterial species. One of them is *E. coli*, whose genome was sequenced in 1997. In 2012, [Martincorena et al.](https://doi.org/10.1038/nature10995) used this data by comparing *E. coli* genomes to test for biased mutation rates. They couldn’t simply compare the diversity across genes, since this is shaped by both mutations and selection. So instead, they focused on synonymous diversity: those variations in DNA that do not affect the gene product[^4]. Whereas genes with smaller diversity across organisms might simply be under stronger purifying selection, genes with smaller *synonymous* diversity actually must have a smaller mutation rate.

[^4]: The technical definition of synonymous definition is different, but supposed to measure the same thing. See the original paper from [Martincorena et al.](https://doi.org/10.1038/nature10995). 

<p align="center">
<img src="/images/mutations/martincorena.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> (a) Mutation rates (estimated using the synonymous diversity across different strains) vary along the E. coli genome. Red bars indicate the 95% confidence interval under a uniform mutation rate. MB: megabases, or 1000 base pairs. (b) Genes with lower mutation rates are often essential. From [Martincorena et al.](https://doi.org/10.1038/nature10995). </span>

The authors found that the synonymous diversity varies 20-fold across the *E. coli* genome, with certain genes showing much less diversity than expected from a uniform mutation rate, and others showing much more. And this pattern correlated with function: the diversity was smallest for essential genes (those critical for healthy function) and those under strong [purifying selection](https://en.wikipedia.org/wiki/Negative_selection_(natural_selection)). Martincorena et al. therefore concluded that the mutation rate varies across the genome, and that this might be an “evolutionary risk management strategy”. 


This finding made a splash in the world of evolutionary biology, but soon other biologists started to find problems ([Chen & Zhang](https://doi.org/10.1093/molbev/mst060),  [Maddamsetti et al.](https://doi.org/10.1093/molbev/msv161)). One problem was theoretical: mutations to individual genes occur so rarely that the advantage of an even lower and gene-specific mutation rate is too small to evolve (more on this later). Other challenges were empirical in nature. For example, Martincorena et al. suggested but did not provide evidence for the mechanism underlying non-random mutations. Finally, they aimed to eliminate the effect of selection by analysing only synonymous genetic diversity, but might have failed to account for all fitness effects. 

Definitive evidence for biased mutations therefore required a stronger, experimental rather than statistical, control for selection. 

## Planting the seeds of a revolution

<p align="center">
<img src="/images/mutations/mutation_accumulation.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> A typical mutation accumulation (MA) experiment, in which n lines are randomly bred for t generations. Source: [Halligan & Keightley](http://www.homepages.ed.ac.uk/pkeightl//publications/halligan-keightley-2009.pdf). </span>

A rigorous but time-consuming approach to experimentally minimise the effect of selection is a so-called mutation accumulation experiment. In such an experiment, the experimenter randomly selects the organism that will give rise to the next generation, making reproductive success independent of fitness[^5]. Repeating this process for many generations results in the accumulation of mutations untouched by selection. Doing this in parallel for several lines allows us to see if certain mutations arise more frequently than others. 

[^5]: Note that this only workds for asexually reproducing organisms. 

Recently, [Grey Monroe](https://doi.org/10.1038/s41586-021-04269-6), working in the lab of [Detlev Wiegel](https://weigelworld.org/), used mutation accumulation lines to investigate the randomness of mutations in *Arabidopsis thaliana* (the primary model for plant genetics). The authors found that genetic variability across mutation accumulation lines greatly varied across the genome, being reduced by a half in gene bodies. This time around, the reduced variability really could not be due to selection, and therefore had to be attributed to bias in the mutation rate. 


<p align="center">
<img src="/images/mutations/monroe.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> (a): Arabidopsis thaliana. (b): mutations are lower for bases within gene bodies. TSS: transcription start sites, TTS:  transcription termination sites.  (c): the mutation rate varies with gene function. Adapted from [Monroe et al.](https://doi.org/10.1038/s41586-021-04269-6) </span>

That still leaves open the mechanism through which the bias could occur. Monroe et al. also addressed this question by showing that the mutation rate of a genomic region can be predicted from several of its features. Some of these features are epigenomic, i.e. they affect gene expression without changing the DNA itself, for example by changing the [histone proteins](https://en.wikipedia.org/wiki/Histone) that package DNA. This change can increase or decrease the genes’ expression by making it more or less accessible to transcription factors. Interestingly, such a change in a gene’s packaging can also increase the accuracy of its replication (see e.g. [Chong et al.](https://doi.org/10.1038/s41467-020-14595-4)). This suggests that natural selection could tune mutation rates by shaping DNA packaging around important genes. 

Finally, the authors also show evidence that the mutation rate bias is adaptive: essential genes mutate less than genes with an environmentally-dependent function. This means that the variation in mutation rate reduces the occurrence of deleterious mutations, and could direct the course of evolution just as well as natural selection. 


## Reconciling the textbook and recent data
The genomic data from Martincorena and Monroe provide strong evidence that natural selection can tune local mutation rates. So why didn't Luria and Delbruck find that T1 presence increased the likelihood of mutations when this clearly would have been adaptive? 
Did they and all these other classic experiments overlook something? Perhaps, but several alternative explanations seem more pertinent. 

The first difference is that Luria & Delbruck tested for an increased mutation rate in a gene under positive selection. The genomic data, by contrast, provide evidence for decreased mutation rates in genes under negative selection. In fact, Martincorena specifically looked for - but failed to find - increased mutation rates (estimated using the synonymous diversity) in genes under positive selection.  The difference between decreasing the mutation rate of some genes and increasing the rate of others is not merely semantic. Increased mutation rates, on one hand, indicate selection for more change in the right direction - exactly what we would want in order to conclude that mutations are directed along the fitness gradient. But the current data don’t strongly support this idea. Instead, they show decreased mutation rates, indicating that natural selection mainly tries to prevent rather than elicit change. Evolution, as they say, is conservative. 

An explanation might be that decreased mutation rates in essential genes have proven advantageous for many millions of years, because their function is highly conserved across species and environments. The importance of non-essential genes, on the other hand, varies on much shorter time scales. Selection might therefore only be able to selectively upregulate mutation rates for genes that are very relatively strong and recurring positive selection. 

The second difference between classical and recent experiments is in the length of the genomic region across which mutation rates might vary. Luria & Delbruck found that the mutation rate of a single gene (that for the receptor protein) did not increase under positive selection. Martincorena & Monroe, on the other hand, found that mutation rates vary across many genes at once. One of the key epigenetic marks that might decrease mutation rates, for example, is shared by at least 15% of the Arabidopsis genome. 

Both Luria & Delbruck and the genomic data are therefore consistent with the idea that mutation rates vary across the genome, but typically not on a gene-by-gene basis. This is an important point because it addresses a major theoretical argument against the existence of biased mutation rates, namely: the selective advantage of mutation rates that vary on a per-gene basis is too small to evolve. This argument, and why it doesn’t apply to the recent findings, can be made more precise using concepts from the field of population genetics.

## Quantifying the advantage of mutational bias

Let's start with a simple case: the fitness effect of a decreased mutation rate in a *single base pair*. Let's also assume this base pair typical in the sense that it is under purifying selection, i.e. a mutation decreases fitness, so a decreased mutation rate increases fitness. This increase is then the product of two factors: the change in the mutation rate and the average fitness effect of a mutation: 

* The change in the mutation rate, in turn, is the product of the original mutation rate *U* and the fractional change in mutation rate *f*. Here *U* =  10^(-8), and *f* = 0.9 (a two-thirds decrease for essential genes). 
* The change in fitness is quantified using the so-called [selection coefficient](https://en.wikipedia.org/wiki/Selection_coefficient), which is the average decrease in offspring from a mutation. From the data: *s* = 0.01; a point-mutation leads, on average, to average decrease in offspring of 1%. 

In general, therefore, the advantage of a mutational bias is smaller for a low baseline mutation rate, and it becomes bigger with the strength of the mutational bias and the fitness effect of mutations. Given the data, the increase in fitness from a mutational bias is therefore equal to

<center> U x f x s = 10^(-8) x  0.9 x 0.01 =  9 x 10^(-11). </center>

That sounds small, but is it too small? In general, only those traits can evolve by natural selection if their selective advantage is strong enough to overcome other evolutionary forces, in particular [genetic drift](https://en.wikipedia.org/wiki/Genetic_drift) (evolution by chance). The strength of genetic drift is typically measured by the inverse of the [effective population size](https://en.wikipedia.org/wiki/Effective_population_size), because chance plays a bigger role in smaller populations. The effective population size of *Arabidopsis* is 300,000, such that genetic drift has a strength of 1/300000 = 3.33 x 10^(-6) -- several orders of magnitude above the selective advantage. Selection is therefore overwhelmed by drift, and it cannot create a base pair specific mutation rate. 

Things look very different, however, if we consider a mutation rate that varies across *multiple base pairs*, because this has a fitness advantage proportional to 

<center> 9 x 10^(-11) x L, </center>

with *L* the number of bases that share the same mutation rate modification. The advantage is strong enough to overcome genetic drift if, roughly, *L* is at least 200,000 base pairs, or 1.5% of the Arabidopsis genome - much smaller than the region with strong epigenetic marks observed by Monroe.  

The upshot is that genetic drift limits the efficiency of natural selection, such that traits can only be selected for if their fitness advantage is large enough. The advantage of a base- or even gene-specific mutation rate is not large enough, at least not single base pairs or even single genes. But the advantage of a mutational bias that covers many base pairs at once is large enough to evolve, consistent with the data from Monroe et al. 


## Conclusion: are mutations directed along the fitness gradient?
In sum, recent data suggests that mutations do not arise independently of their fitness effect, but that selection has simultaneously decreased the mutation rates of essential genes. This is, I think, quite revolutionary, and deserves to be added to the textbooks. Moving forward, it will be exciting to look for mutational bias in different species, and to causally test the molecular mechanisms proposed by Monroe and others. 

All this intellectual upheaval does not, in my understanding, constitute sufficient evidence to conclude that evolution “estimates the fitness gradient”. Perhaps future evidence will, but we’ve several reasons why it might not. Genetic drift limits the efficiency of natural selection, such that only those traits can evolve that offer a sufficiently strong fitness benefit. A gene-specific mutation rate might not make this cut, because its advantage is not as large as we initially might have thought. 

This conclusion runs counter to our departing point that evolution "should" estimate the fitness gradient. This contradiction doesn’t, of course, imply that biology should dispose of normative theory altogether. It does, however, show that normative theory should be kept in check by data, and more generally, that interdisciplinary interactions should start with an open and curious mind. 

Here, we have considered the possibility that genetic mutations shape the course of evolution. I might use a future blog post to explore how a similar role could be played by processes above the single gene level, such as [development](https://en.wikipedia.org/wiki/Evolutionary_developmental_biology) and [ecology](https://en.wikipedia.org/wiki/Evolutionary_ecology). 



## Acknowledgements

