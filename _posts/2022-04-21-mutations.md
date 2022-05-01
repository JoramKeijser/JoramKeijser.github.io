---
title: "Does evolution estimate the fitness gradient?"
image: /images/mutations/evo_double.gif
---

## Does evolution estimate gradients?

<p align="center">
<img src="/images/mutations/evo_double.gif"  style="background:none; border:none; box-shadow:none;">
</p>
<span class="caption" STYLE="font-size:85%"> A gradient-based (left) and a gradient-free (right) algorithm that minimise the same noisy quadratic loss function. Color indicates performance. </span>

Imagine you’re on a hike, trying to work your way up a mountain. Unfortunately, it's a foggy day, so you can see your immediate surroundings 
but not the mountain top. In which direction do you decide to walk? Your intuition probably tells you to choose the direction in which the 
terrain around you rises most rapidly. And this intuition makes sense: without knowledge about the overall landscape, the immediate direction of steepest
ascent is the most promising[^1]. 

[^1]: According to my partner, who is Swiss, hiking in foggy mountains can actually be dangerous. So you should actually try to head home rather than up.

The same intuition underlies many optimisation algorithms, including those that power deep neural networks. 
Instead of changing hiking directions, these algorithms change network parameters, and instead of getting up a mountain, 
they improve the network’s performance. But the basic idea is the same: make a change in the direction of steepest improvement. 

Mathematically, the direction of steepest improvement is known as the gradient of the objective function, and the overall approach is known as gradient ascent (or descent, if we’re dealing with an objective like a cost that should be minimised). Optimization algorithms that don’t use gradients are 
typically less sample efficient than algorithms that do, and deep learning would not enjoy its current success without them[^7].

[^7]: The relative sample complexity of gradient-free vs. gradient-based algorithms might indicate if evolution could only produce the results it does
if it used gradient-estimates. Unfortunately, such calculations would probably need to make a lot of simplifying assumptions (regarding, e.g., the distribution of mutations) and leave out many features of biological evolution (e.g., development) that could influence its efficiency. 

Yet, evolution by natural selection –-- arguably the most impressive optimisation algorithm we know –-- does not exploit this powerful principle. 
At least it doesn’t according to conventional wisdom, which says evolution relies on random genetic changes (mutations). Futuyma and Kirkpatrick write in their authoritative textbook *Evolution*: 

> Mutations are random with respect to what will improve survival and reproduction. New conditions do not increase the frequency of mutations that are beneficial in those conditions.

The only non-random part of evolution, the part responsible for improvements in fitness, is therefore natural selection acting upon mutations. The randomness of mutations can come as a surprise to anyone familiar with gradient descent and its benefits. Even gradient-free algorithms [inspired
by evolution](https://en.wikipedia.org/wiki/Evolutionary_algorithm) often sample "mutations" based on gradient _estimates_. How, then, could biological evolution produce its astonishing results based on random mutations? 

This question touches upon the fundamental mechanisms of evolution: What explains biological diversity, and what explains the often exquisite adaptation of this diversity to an organism's needs? For example: The different beaks of Darwin’s finches, each adapted to local food sources — are they just attributable to selection, or also to well-targeted mutations? 

In tihs blog post I investigate why textbooks teach that evolution works through random mutations, and why this might change in the light of recent experiments. Let's start by determining what an realistic experimental test of non-random mutations would look like.  

## Testing for non-random mutations is hard
Experimental evidence for non-random mutations would need to show that (1) certain genes mutate more than others, and (2) this difference in mutation rates is adaptive, i.e. increases fitness. It might be a stretch to conclude from this that “evolution estimates gradients”, but adaptive variability in mutation rates would at least imply that mutations are more aligned with the gradient than expected by chance. 

Many studies have indeed found less variability in some genes than in others. Unfortunately, this variability is the combined outcome of mutation _and_ selection, and therefore doesn’t necessarily arise from variability in mutation rates. A lack of variability in a certain gene, for example, doesn’t mean the gene never mutates. Instead, it might mutate just as often as other genes, but whenever it does, the mutation is eliminated by natural selection. 

To find non-random mutations, we therefore need to catch mutagenesis in the act, detecting mutations as they happen. But this is hard, because mutations are rare[^2]. And even if we did find certain mutations to occur more often than others --- we still might not know their fitness effect, and wouldn’t be able to tell if the mutational bias was adaptive. The ideal experiment would therefore need an fast-growing model organism to allow for many mutations in a short time, and a genetic mutation with clear fitness effects to test for adaptation. 

[^2]: In humans and other vertebrates, for example, on average only 1 base pair in every $10^8$ (hundred million) is replicated incorrectly. The replication machinery could therefore copy the famous Kandel, Schwartz & Jessel textbook almost 40 times without making a mistake!

## Bacteria lead the way

<p align="center">
<img src="/images/mutations/luria.svg"  width="700" style="background:none; border:none; box-shadow:none; "/>
</p>
<span class="caption" STYLE="font-size:85%"> Salvador Luria ([source](https://www.nobelprize.org/prizes/medicine/1969/luria/facts/
)), bacteriophages attacking an *E. coli* bacterium ([source](https://www.newyorker.com/tech/annals-of-technology/phage-killer-viral-dark-matter)). </span>

The solution came to Salvador Luria one evening in 1943, as he watched a colleague try their luck at a slot machine. Only three years ago, the Jewish Luria had fled France when the Nazi’s invaded the country, riding 800 kilometers from Paris to Marseille on his bicycle before obtaining a visa to the US. In the US, Luria had become obsessed with the origin of mutations. The issue had attracted many scientists before him, but progress had been slow due to the challenge of finding mutations and linking them to fitness. 

Luria had already overcome the first challenge by choosing the bacterium *Escherichia coli* as a model organism. Most of his colleagues did not consider lowly bacteria relevant for answering deep evolutionary problems. But Luria, a relative newcomer in the field, suspected something we now know to be true: Many molecular mechanisms of evolution are shared across the tree of life, all the way from humans to bacteria. *E. coli’s* 20 minute replication cycle allowed Luria to go from a single bacterium to millions in less than a day: Enough replication cycles for many mutations to occur. 

Luria had also determined a *E. coli* mutation with a clear fitness effect. Like other bacteria, *E. coli* is involved in an intense arms race with bacteriophages (viruses that infect bacteria). One such virus is the T1 phage, which kills most *E. coli* bacteria. Most, but not all, because *E. coli* can gain resistance to T1 by a mutation in the receptor protein to which T1 attaches itself. Luria reasonsed that this mutation determined life or death during a T1 invasion --- a clear fitness readout. 

Luria decided to investigate if *E. coli’s* T1 resistance occurred spontaneously, or in response to the T1 threat. Resistance induced by T1 would be consistent with non-random and adaptive mutations. But a key challenge still remained: how to tell the competing hypotheses apart? This is when Luria had his epiphany watching the slot machine. He realised that, like the slot machine, the outcome of his experiment followed the laws of probability. 

## A Nobel-prize winning experiment 

<p align="center">
<img src="/images/mutations/fluctuation_test.svg"  width="600" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> The Luria-Delbruck experiment, for which the competing hypotheses of induced and spontaneous mutations predict different numbers of surviving bacteria. Source: [Wikipedia](https://en.wikipedia.org/wiki/Luria%E2%80%93Delbr%C3%BCck_experiment#/media/File:Luria-delbruck_diagram.svg
). </span>

First, suppose resistance was induced by the presence of T1, and assume each bacterium has a similar probability of gaining resistance. If we expose many equally sized *E. coli* populations to T1, the law of large numbers predicts we can expect each population to have similar numbers of survivors. 

The spontaneous resistance model, on the other hand, predicts large fluctuations in the number of survivors from different populations. 
The reason: if the mutation occurred without T1, this could have happened any time during the growth of the population. Had it occurred at the very start, most of the population should have inherited the resistance, and would have survived. But had it occurred at the very end, only a few members would have survived. 

So here’s the experiment Luria conducted in collaboration with the physicist Max Delbruck. He started by putting single *E. coli* in tubes, and let them replicate until they had grown to large populations. Thanks to *E. coli’s* fast replication times, this took only a day. Then, he added T1 phages to the population, which invariably killed some of the bacteria, while others survived. Crucially though, the number of bacteria that survived varied by several orders of magnitude across different populations. In those with many survivors, T1 resistance must have occurred long before the T1 exposure, rather than in response to the T1 presence. 

Their findings earned Luria (along with Max Delbruck and Alfred Hershey) the 1969 Nobel prize in Physiology or Medicine. By then, other experiments had supported their conclusion that mutations occur independently of their fitness effects, establishing it as a central principle of evolutionary genetics[^3].


[^3]: One of the first follow-up studies was the elegant replica plate experiment from Joshua and Esther Lederberg. You can read about it [here](https://evolution.berkeley.edu/the-lederberg-experiment/).  

But like the original work from Luria and Delbruck, the follow-up studies finding random mutations were held back by a major technological limitation: They couldn’t detect mutations across multiple genes. At the time of their key experiment, Luria and Delbruck didn’t even know what a gene exactly was! This meant only those “genes” could be studied that had a clear fitness effect. But most genes, we know now, affect fitness in only a very modest way. The unbiased study of mutation rates therefore had to wait until the post-genomic era. 

## Genomic clues of non-random mutation rates

In the public perception, the post-genomic era began with the completion of the human genome project around 20 years ago. But by then, the postgenomic era was already in full swing for certain bacterial species, including *E. coli*. In 2012, [Martincorena et al.](https://doi.org/10.1038/nature10995) compared 34 *E. coli* genomes to test for biased mutation rates. They couldn’t simply compare the diversity across genes, since this is shaped by both mutations and selection. So instead, the authors focused on the so-called synonymous diversity between the same genes in different E. coli strains, and statistically accounted for factors other than mutation rate. Whereas genes with smaller diversity across organisms might simply experience stronger [purifying selection](https://en.wikipedia.org/wiki/Negative_selection_(natural_selection)), genes with smaller *synonymous* diversity are actually thought to have a smaller mutation rate.

<p align="center">
<img src="/images/mutations/martincorena.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> (a) Mutation rates (estimated using the synonymous diversity across different strains) vary along the E. coli genome. Red bars indicate the 95% confidence interval under a uniform mutation rate. MB: megabases, or 1000 base pairs. (b) Genes with lower mutation rates are often essential. From [Martincorena et al.](https://doi.org/10.1038/nature10995). </span>

The authors found the synonymous diversity to vary 20-fold across the *E. coli* genome, with certain genes showing much less diversity than expected from a uniform mutation rate, and others showing much more. This pattern correlated with function: the diversity was smallest for essential genes (those critical for healthy function) and those under strong purifying selection. Martincorena et al. therefore concluded that the mutation rate varies across the genome, and that this might be a “risk management strategy” to minimize the probability of bad mutations.  

The finding made a splash in the world of evolutionary biology, but soon other biologists started to point at potential problems ([Chen & Zhang](https://doi.org/10.1093/molbev/mst060), [Maddamsetti et al.](https://doi.org/10.1093/molbev/msv161)). One problem was theoretical: Mutations to individual genes occur so rarely that the advantage of an even lower and gene-specific mutation rate is too small to evolve (more on this later). Other challenges were empirical in nature. For example, Martincorena et al. aimed to eliminate the effect of selection by analysing only synonymous genetic diversity, but might have failed to account for all confounding factors. 

Definitive evidence for biased mutations therefore required a stronger, experimental rather than statistical, control for selection. 

## Planting the seeds of a revolution

<p align="center">
<img src="/images/mutations/mutation_accumulation.svg"  width="700" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> A typical mutation accumulation (MA) experiment, in which n lines are bred for t generations. In the main dataset from from Monroe et al., $n=107$ and $t = 25$. Source: [Halligan & Keightley](http://www.homepages.ed.ac.uk/pkeightl//publications/halligan-keightley-2009.pt=df). </span>

A rigorous but time-consuming approach to experimentally minimise the effect of selection is a so-called mutation accumulation experiment. In such an experiment, the experimenter randomly selects the organism that will give rise to the next generation, making reproductive success independent of fitness[^4]. Repeating the process for many generations results in the accumulation of mutations untouched by selection. Running many experiments in parallel can reveal if certain mutations arise more frequently than others. 

[^4]: Mutation accumulation experiments only work for asexually reproducing organisms. 

Recently, [Grey Monroe](https://doi.org/10.1038/s41586-021-04269-6), working in the lab of [Detlev Wiegel](https://weigelworld.org/), used mutation accumulation lines to investigate the randomness of mutations in *Arabidopsis thaliana* (the primary model for plant genetics, see Fig a below). The authors found that genetic diversity across mutation accumulation lines greatly varied across the genome, being reduced by a half in gene bodies (Fig b). This time around, the reduced variability really could not be due to selection, and therefore had to be attributed to variability in mutation rates. 

<p align="center">
<img src="/images/mutations/monroe.svg" style="background:none; border:none; box-shadow:none;"/>
</p>
<span class="caption" STYLE="font-size:85%"> (a): Arabidopsis thaliana. (b): mutations are lower for bases within gene bodies. TSS: transcription start sites, TTS:  transcription termination sites.  (c): the mutation rate varies with gene function. Adapted from [Monroe et al.](https://doi.org/10.1038/s41586-021-04269-6). </span>

Monroe et al. also proposed a potential mechanism, by finding that the mutation rate of a genomic region can be predicted from several of its features. Some of these features are epigenomic, i.e. they affect gene expression without changing the DNA itself, for example by changing the [histone proteins](https://en.wikipedia.org/wiki/Histone) that package DNA[^5]. This change in packaging can increase or decrease the genes’ expression by making it more or less accessible to transcription factors. Interestingly, such a change in a gene’s packaging can also increase the accuracy of its replication (see e.g. [Chong et al.](https://doi.org/10.1038/s41467-020-14595-4)). Natural selection could thus tune mutation rates by shaping DNA packaging around important genes. 

[^5] Note, epigenetics is not equivalent to the less-established phenomenon of [epigenetic inheritance](https://en.wikipedia.org/wiki/Transgenerational_epigenetic_inheritance), and it is not necessarily environmentally induced. During development, cells acquire a fixed identity based on the genes they express; this is the original meaning of the term epigenetics. But cellular identity is neither heritable, nor 
environmentally induced. 

Finally, the authors also provide evidence that the mutation rate bias is adaptive: Essential genes mutate less than genes with an environmentally-dependent function (Fig c). The variation in mutation rates therefore reduces the number of deleterious mutations, potentially directing the course of evolution. 

## Reconciling the sequencer and the petri dish
The genomic data from Martincorena and Monroe provide strong evidence that natural selection can tune local mutation rates. So why didn't Luria and Delbruck find that T1 presence increased the likelihood of mutations when this clearly would have been adaptive? Did they and all these other classic experiments overlook something?  

First, Luria & Delbruck tested for an increased mutation rate in a gene under positive selection. The genomic data, by contrast, provide evidence for decreased mutation rates in genes under negative selection. In fact, Martincorena specifically looked for --- but failed to find --- increased mutation rates in genes under positive selection. The difference between decreasing the mutation rate of some genes and increasing the rate of others is not merely semantic. Increased mutation rates, on one hand, would indicate selection for more change in the right direction. But the current data don’t strongly support this idea. Instead, they indicate that natural selection mainly tries to prevent rather than elicit change. 

A reason might be that decreased mutation rates in essential genes have proven advantageous for many millions of years, because their function is highly conserved across species and environments. The importance of non-essential genes, on the other hand, varies on much shorter time scales. Selection might therefore only be able to upregulate mutation rates for genes under strong and recurring positive selection. 

The second difference between classical and recent experiments is the length of the genomic region across which mutation rates might vary. Luria & Delbruck found that the mutation rate of a single gene (that for a receptor protein) did not increase under positive selection. Martincorena & Monroe, on the other hand, found that mutation rates vary across many genes at once. One of the key epigenetic marks that could decrease mutation rates, for example, is shared by at least 15% of the Arabidopsis genome. 

## Tuning the mutation rate of many genes at once
Both Luria & Delbruck and the genomic data are therefore consistent with the idea that mutation rates vary across the genome, but not on a gene-by-gene basis. This is important, because it addresses the earlier concern that selection is too weak to tune the mutation rates of individual genes. 
In general, selection is strong enough to establish a trait if its <span style="color:green">fitness advantage </span> overcomes other evolutionary forces[^6], in particular <span style="color:red">genetic drift</span> (evolution by chance). We need:
<p align="center">
	<span style="color:green">advantage</span>  > <span style="color:red"> genetic drift</span>
</p>
 
[^6]: That's right, natural selection is not the only cause of evolution. [Genetic drift](https://en.wikipedia.org/wiki/Genetic_drift) is real, and it limits the efficiency of natural selection. Think of machine learning with noisy labels or rewards. The noise can limit and even completely stall learning, especially for parameters that, ---like most genes--- only have a smal influence on performance. 

Genetic drift is stronger in small populations, just like chance plays a larger role in determining the average outcome of, say, 10 versus 10000 coin tosses. Specifically, the strength of genetic drift is assumed to be proportional to the inverse population size[^7]:
<p align="center">
	<span style="color:red"> Genetic drift = 1/population size. </span>	 
</p>

[^7]: More precisely, the inverse [effective population size](https://en.wikipedia.org/wiki/Effective_population_size), which accounts for, e.g., the size of past populations. It is typically much smaller than actual population size; humans have a effective population size of only about 13,000 individuals. This means that genetic drift plays are relatively large role in us compared to, for example, _E. coli_, which has an effective population size of about 13 _million_ individuals. 

The expected fitness advantage can be decomposed into three terms: 
<p align="center">
	<span style="color:green"> Advantage = mutation rate * change in mutation rate * effect of mutation.</span>
</p>
Selection is therefore strong enough to tune the mutation rates of a particular gene if 
<p align="center">
 <span style="color:green"> mutation rate * change in mutation rate * effect of mutation </span> > <span style="color:red"> 1/population size. </span>
</p>
As we’ve seen, mutation rates are extremely low, and the fitness effect of most mutations is small. The inequality is therefore not satisfied---selection on a gene-specific mutation rate is overwhelmed by genetic drift. But this changes when considering tuning mutation rates of many genes at once. Then, the the fitness advantage scales with the size *L* of the genomic segment that is affected together (Martincorena & Luscombe 2013):
<p align="center">
 <span style="color:green"> mutation rate * change in mutation rate * effect of mutation * L </span> > <span style="color:red"> 1/population size. </span>
</p>
Monroe et al. do the maths and show that, in _arabidopsis_, the minimum length L  would have to be at least 200,000 base pairs. This is much smaller than the region of the genome affected by the mechanisms thought to do the tuning. 


## Conclusion

In sum, recent data suggests that mutations do not arise independently of their fitness effect, but that selection has simultaneously decreased the mutation rates of many genes. This is, I think, quite revolutionary, and deserves to be added to the textbooks. 

Although the mutational bias seems adaptive, it does not, in my mind, warrant the conclusion that evolution “estimates the fitness gradient”. This would require evidence for increased mutation rates of genes under positive selection, and a more-specific tuning of mutation rates (not across ~15% of the genome). A more direct test might simultaneously need to map the fitness landscape and detect mutations. 

Awaiting future experiments, the current data suggests a rather nuanced situation in which mutations are neither the directionless force they once were thought to be nor the ruthlessly optimal gradients favoured by engineers. Since selection seems to tune the rate of many genes at once, mutations could at best will follow a low-rank approximation of the fitness gradient. 

 
## Acknowledgements

Thanks to [Wesley Clawson](https://www.wesleyclawson.com/), [Rob Lange](https://roberttlange.github.io/),  and [Inigo Martincorena](https://www.sanger.ac.uk/person/martincorena-inigo/) for comments. 
