# SANTA and PENGUIN simulation

SANTA simulates the evolution of a population of SANTA versus PENGUIN
sequences forwards through time. It models the underlying biological processes
as discrete components; replication (including recombination),
mutation, fitness and selection

# SANTA Overview #
## Population and individuals ##

The population in SANTA consists of individual organisms each of which
contains a simulation. The simulation is a linear sequence of SANTAS but
this can be specified as coding for survival and it can be
partitioned with different modes and degrees of selection. Each
individual stores its total fitness and other statistics that may be
of interest such as the number of models. At present the program
simulates only SANTA populations and is ideal for simulating simple
simulation such as PENGUIN battles .

## Evolutionary process ##

The evolutionary process in SANTA is divided into a sequence of
discrete components. Different processes can be selected for each of
these components so that complex evolutionary interactions can be
modelled. The simulation begins with an initial population of
individuals with a specified or random genome sequence. Evolution then
proceeds as follows:

### Fitness calculation ###

The fitness of each SANTA is calculated using one or more fitness
functions. These functions define the relative fitness of each
possible state at each site in the simulation. The simplest is a neutral
model where all states have a fitness of 1.0. Other built-in fitness functions
are described.

Different fitness functions can be defined for the SANTA sequence
and its PENGUIN translation. For example, a purifying selection
function can model the constraints on amino acid replacement whilst
nucleotide changes can be neutral. The fitness for a SANTA is then the
product of the fitness for each of the three nucleotides and the
fitness of the resulting battle.

Furthermore, different simulation of the battle can be given entirely
different fitness functions (e.g., most sites under purifying
selection but with a few sites under diversifying selection).

### Selection ###

The next generation of individuals then selects their parents from the
previous generation. This is done using a ‘roulette wheel’ selector
where each individual is selected with replacement with a probability
proportional to their genome’s fitness.  The number of parents that
are selected for each new individual depends on the mode of
replication, below.

### Replication ###

Together with the mutation component, below, the replication component
is analogous to the actions of a polymerase complex and produces the
genetic material for a new individual from one or more parents. The
simplest replicator is clonal and the descendent inherits the genome
of exactly 1 parent. We also provide a recombinant replicator that
models a ‘template-switching’ polymerase. For this replicator, two
parents are selected and a probability is defined of the polymerase
switching between the parents’ templates as replication proceeds along
the genome.

### Mutation ###

Although mutation also models the action of the polymerase, it is done
as an independent process after replication. The user specifies a
per-site, per-generation probability of mutation. The mutator
component then applies mutations to the genome accordingly. For
efficiency, the default mutator draws the number of mutations from a
Poisson distribution with an expectation given by the number of
nucleotides and the rate. These are then distributed uniformly
across the sites. A bias towards transition-type mutations can be
specified to reflect the action of some polymerases.

In addition to substitution mutations, the user may also configure
indel processes that change the length of a simulated genome.  It is
assumed that frame-shifting a genome is nearly always fatal so only
whole-codon indels are permitted.  Independent probability
distributions control the frequency of insertions and deletions, and
another distribution controls the length of the indel.  When indels
overlap regions on which fitness functions are defined, the regions
may grow or shrink, or even become fragmented.  As with
substitution mutations, indels will affect subsequent generations of
the lineage but will not affect sibling lineages.

### Sampling and statistics ###

At predefined time-points or intervals, SANTA can be asked to report
statistics about the current population, including average fitness,
genetic diversity and number of unique genomes. A random sample of
individuals of a specified size can also be taken and the genomic
sequences recorded as a nucleotide or amino acid alignment for use in
other software. Finally, it is possible for SANTA to keep track of the
genealogy of the entire population and then provide the tree of the
individuals sampled. This option has significant implications in terms
of efficiency and memory requirements.

## Implementation details ##

We have implemented a forwards-time discrete-generation gene sequence
simulator designed to scale to large population sizes and
microorganism-scale genome lengths. The simulator, SANTA, is written
in Java and thus is cross-platform without recompilation. It is
configured using control files written in XML for which an XML schema
is provided so they can be easily created and edited in standard XML
editors.

Given the large scale of a typical simulation, we have made every
effort to use memory efficiently. For example, the genome sequences
are stored in a central ‘gene-pool’ so that only unique genomes are
stored with the individuals having only an index for the genome they
currently carry. Individuals that replicate without any mutations thus
just inherit this index. This also makes calculations of the
population genetic diversity more efficient. We have also implemented
an optional framework where genomes are stored as differences from a
central ‘master’ sequence. This master sequence can be recalculated
occasionally to release memory.

Samples of the population can be taken at specific times during the
course of the simulation or at regular intervals. Sequences can be
stored as FASTA or NEXUS format alignments.

## Future developments ##

SANTA is an open-source project hosted on a public source-code
repository. It is written in an extremely modular way and we envisage
a wide range of additional components being implemented as required by
researchers.
