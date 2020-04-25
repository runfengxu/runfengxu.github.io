
---
layout:     post
title:      Paper notes: the art, science, and engineering of fuzzing: a survey
subtitle:   
date:       2020-04-25
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - security
 - fuzzing
 - paper
---



### paper review: The Art, Science, and Engineering of Fuzzing: A Survey
Problem:
 - Description of some fuzzers do not go much beyond their source code and manual page. It is easy to lose track of the design decisions and potentially important tweaks in fuzzers over time.
 - Fragmentation in the terminology used by various fuzzers.
     - test case minimization, test case reduction, crash minimization

Content:
 - fuzzing terminology,unified model
 - every stage of model fuzzer
 
### $2 System, Taxonomy, and TEST programs

2.1 FUzzing&Fuzzing test

> definition: FUzzing is the execution of the Program Under Test(PUT) using input sampled from an input space(fuzzing input space) that protrudes the expected input space of PUT.

3 remarks:
 - fuzzing space does not necessarily include input space.
 - repeated executions
 - not necessarily to be randomized.


> Fuzzing Testing: Fuzz testing is the use of fuzzing to test if a PUT voilates a security policy.

> Fuzzer: A fuzzer is a program that performs fuzz testing on a PUT.

> Fuzz Campaign: A fuzz campaign is a specific execution of a fuzzer on a PUT on a PUT with a specific security policy,

> BUg oracle:  a bug oracle is a program, perhaps as part of a fuzzer, that determins whether a given execution of the PUT violates a specific secutity policy.

>Fuzz Configuration: A fuzz configuration of a fuzz algorithm comprises the parameter value that control the fuzz algorithm.

![Screenshot%20from%202020-04-21%2014-55-17.png](attachment:Screenshot%20from%202020-04-21%2014-55-17.png)

PREPROCESS (C) → C 

 - A user supplies PREPROCESS with a set of fuzz config-urations as input, and it returns a potentially-modified set of fuzz configurations. Depending on the fuzz algo-rithm, PREPROCESS may perform a variety of actions such as inserting instrumentation code to PUTs, or measuring the execution speed of seed files.

SCHEDULE(C, t elapsed , t limit ) → conf

 - S C H E D U L E takes in the current set of fuzz configurations, the current time t elapsed , and a timeout t limit as
input, and selects a fuzz configuration to be used for
the current fuzz iteration.

I N P U T G E N (conf) → tcs

 - I N P U T G E N takes a fuzz configuration as input and
returns a set of concrete test cases tcs as output.
When generating test cases, I N P U T G E N uses specific
parameter(s) in conf. Some fuzzers use a seed in conf
for generating test cases, while others use a model or
grammar as a parameter. See § 5.

I N P U T E V A L (conf, tcs, O bug ) → B ′ , execinfos

 - I N P U T E V A L takes a fuzz configuration conf, a set of
test cases tcs, and a bug oracle O bug as input. It
executes the PUT on tcs and checks if the executions
violate the security policy using the bug oracle O bug . It
then outputs the set of bugs found B ′ and information
about each of the fuzz runs execinfos, which may
be used to update the fuzz configurations. We assume
O bug is embedded in our model fuzzer. See § 6.

C O N F U P D A T E (C, conf, execinfos) → C

 - C O N F U P D A T E takes a set of fuzz configurations C, the
current configuration conf, and the information about
each of the fuzz runs execinfos, as input. It may
update the set of fuzz configurations C. For example,
many grey-box fuzzers reduce the number of fuzz
configurations in C based on execinfos. See § 7.

C O N T I N U E (C) → { True , False }

 - C O N T I N U E takes a set of fuzz configurations C as input
and outputs a boolean indicating whether a new fuzz
iteration should occur. This function is useful to model
white-box fuzzers that can terminate when there are no
more paths to discover.

### Taxonomy

 - Black Box fuzzer
  - these techniques can
observe only the input/output behavior of the PUT, treating
it as a black-box.
 - white box fuzzer
  - white-box fuzzing [90]
generates test cases by analyzing the internals of the PUT4
and the information gathered when executing the PUT.
Thus, white-box fuzzers are able to explore the state space
of the PUT systematically.

 - grey-box fuzzer
 
 
### Instrumentation
 - **static**: during preprocessing:   Static instrumentation is often performed at compile time on either source code or intermediate code. Since it occurs before runtime, it generally imposes less runtime
overhead than dynamic instrumentation. If the PUT relies
on libraries, these have to be separately instrumented, com-
monly by recompiling them with the same instrumentation.
Beyond source-based instrumentation, researchers have also
developed binary-level static instrumentation (i.e., binary
rewriting) tools
 - **dynamic**: during inputEval.Although it has higher overhead than static instrumentation, dynamic instrumentation has the advantage that it can easily instrument dynamically linked libraries, because
the instrumentation is performed at runtime.

AFL supports static instrumentation at the source code level with a modified compiler,
or dynamic instrumentation at the binary level with the help
of QEMU
 
### in-memory fuzzing:
 - some complex application need a long time before taking input. In-memory fuzzing means take a snapshot of the PUT after initialization. to fuzz a new test case, one can then restore the memory snapshot before writing the new test case directly into memory and executing it.
 - some fuzzers perform in-memory fuzzing on a function without restoring the PUT's state after each iteration. We call that in-memory API fuzzing. e.g. AFL has an persistent mode, which repeatedly performs in-memory API fuzzing in a loop without restarting the process. It ignores potential side effects from the function being called multiple times in the same execution. 
 - Although efficient, it suffers unsound fuzzing results:bugs found from in-memory fuzzing may not be reproducible.
 
 ### Seed 

 - Selection: a common approach would be: find a minimal set of seeds that maximizes a coverage metric(called computing a minset) e.g.**AFL**’s minset is based on branch. **Honggfuzz** [204] computes coverage based on
the number of executed instructions, executed branches, and
unique basic blocks.
coverage with a logarithmic counter on each branch.
 
 - Seed Trimming: Smaller seeds are likely to consume less memory and entail higher throughput. ***AFL*** [231],
which uses its code coverage instrumentation to iteratively
remove a portion of the seed as long as the modified seed
achieves the same coverage.
 
 
### Scheduling:
> In fuzzing, scheduling means selecting a fuzz configuration for the next fuzz iteration.

#### Fuzz Configuration Scheduling (FCS) Problem
An inherent conflict: exploration vs exploitation. time can either be spent on gathering more accurate information on each configuration(exploration), or on fuzzing the configurations that are currently believed to lead to more favorable outcomes(exploitation).

 - **Black-box FCS Algorithms**
  - Householder and Foote first studies blackbox fuzzing FCS: they postulated that cinfiguration with a higher observed success rate(#bugs/#runs) should be preferred.
  - Improved on multiple fronts by Woo et al. They refined the mathematical model of black-box mutational fuzzing from **Bernoulli trials** to the **Weighted Coupon Collectors Problem with Unknown Weights** and designed **MAB mulyi-armed bandit** algorithm
  
 - **grey-box FCS algorithm**
  - richer set of information about each configuration: coverage attained. AFL is the forerunner and it is based on an evolutionary algorithm(EA): EA maintains a population of configurations, each with some value of "fitnessL. EA selects fit configurations and applies them to genetic transformations to produce offsprings which are believed more likely to be fit. 
  - as a high level approximation, AFL considers the one that contains the fastest and smallest input to be fit. it keeps a queue of configurations, and fuzz each configuration a constant number of run.
  
  
  
  
#### input generation
 - model-based fuzzers: we dont care .
 - Mutaion-based Fuzzers
  - **Bit-Flipping**: filp a fixed(or random) number of bits. Some fuzzers employ a user-configurable parameter called the mutation ratio, which determins the number of bit positions to flip for a single execution of INPUTGEN. SymFuzz showed that fuzzing performance is sensitive to the mutation ratio. And there are several approaches for finding a good mutation ratio.
  - **Arithmetic mutation**:AFL , honggfuzz contain another mutation operation where they consider a selected byte sequence as an integer, and perform simple arithmetic on that value.
  - **Block-based Mutation**,
  - **Dictionary-based Mutation**
 - White-box Fuzzers
  - **dynamic symbolic execution** to generate test cases:
  At a high level, classic symbolic execution runs a program with symbolic values as input, which represents all possible values. As it executes the PUT, it builds symbolic expressions instead of evaluating concrete values. Whenever it reaches a conditional brach instruction, it conceptually forks two symbolic interpreters, one for true branch ,another for false branch. For every path, a symbolic interpreter builds up a path formula for every branch instruction it encountered during an execution. A path formula is satisfiable if there is a concrete inputs by querying an SMT solver for a solution to a path formula. **Dynamic symbolic execution**: both symbolic execution and concrete execution operate at the same time. The idea is that the concrete execution states can help reduce the complexity of symbolic constaints. Dynamic symbolic execution is slow but an common strategy is to narrow its usage by letting the user to specify uninteresting parts of the code or by alternating bwtween concolic testing and grey-box fuzzing.
  - **Guided Fuzzing**: (1) a costly program analysis for obtaining useful information, (2) tet case generation with the guidance from the (1).
  - **PUT mutation** : patch the PUT to avoid checksum fail. After found the succeeded test case modified it on the original program using symbolic execution(T-fuzz)
  
  
  
what is symbolic execution
