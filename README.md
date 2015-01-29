CoherenceFramework
===============
This codebase now includes several basic coherence models:


<h2>Entity-based coherence models: </h2> Code for 1)entity grid experiment and 2)entity graph experiment.
Both are multilingual. They currently work for French and German.
(Although following recent updates need tested for the multilingual initialisation, particularly the EntityGridExtractor which takes
ptb trees as input. Entry point for EntityGridFramework should still be fine.)

<h2>Syntax-based coherence models: </h2>Code for a syntax-based model, similar to the 1)local coherence model of A.Louis(Louis and Nenkova, 2012) based on syntactic patterns ,
in addition to 2)our own adaptation of it, which is a fully generative model based on IBM1. We learn a
probability distribution over the alignments to better learn the patterns, instead of a uniform distribution.

=====================================================
<b>Our objective</b> was to test existing models on an Machine Translation output, an entirely different, more challenging context than the one they are generally used in.
Previously they have been used to assess coherence monolingually, in a clear-cut task (eg selecting best order from
shuffled sentences of a coherent text) whereas lack of coherence can be caused in other more subtle ways. 

We investigate these models for the task of measuring the coherence of machine translation output. 
This is a very different scenario: ﬁrstly, it is more subtle as the sudden breaks in transitions or shifts of focus apparent in the 
traditional monolingual test scenario are absent;
and secondly, as the machine translated output MT sentences alone may be incoherent. It may contain other textual issues, such as ungrammatical fragments.
And given the way translations are generated by standard MT systems, on a sentence-by-sentence basis, several coherence-related phenomena 
spanning sentence boundaries can be lost, leading to incoherent document translations 
(such as incorrect co-referencing, inadequate discourse markers, and lack of lexical cohesion). 

 
===============================================

<b>Entity grids</b> are constructed by identifying the discourse entities in the documents
under consideration, and constructing a 2D grids whereby each column corresponds to the entity,
i.e. noun, being tracked, and each row represents a particular sentence in the document. Once all occurrences of nouns 
and the syntactic roles they represent in each sentence (Subject (S), Object (O), or other (X)) are extracted, 
an entity transition is deﬁned as a consecutive occurrence of an entity, with given syntactic roles. These are computed 
by examining the grid vertically for each entity. 

<b>Entity graphs</b> project the entity grid into a graph format, using a bipartite graph. They
 capture the same entity transition information as the entity grid experiment, although they
only track the occurrance of entities, avoiding the nulls of the other, and additionally can track
cross-sentenial references. The graph tracks the presence of all entities, taking all nouns in the document as discourse
entities,and connections to the sentences they occur in. 
The coherence of a text in this model is measured by calculating the average outdegree
of a projection, summing the shared edges (ie of entities leaving a sentence) between 2 sentences.

<b>Syntax models </b>
Louis and Nenkova (2012) create a coherence model based on syntactic patterns. 
They take the syntax patterns extracted from documents marked up with parse trees,
The local model holds that in a coherent text, consecutive sentences will exhibit syntactic regularities. 
Particular patterns may prove typical to speciﬁc discourse types and identify the ‘intentional discourse structure'. 
We examine the syntactic structure of sentence pairs, to establish any patterns. 
This done by computing the most frequent syntactic productions that occur in adjacent sentences.
We initially work with parse tree productions, invsetigating pairs of syntactic items.
 
 
  
===========================
Running the code:

EntityExperiments:
java -classpath  DiscourseFramework.jar nlp/framework/discourse/EntityExperiments "C:\\SMT\\datasets\\corpus-PE\\corpus\\test\\" "English" "false" "false" "2"

java -classpath  DiscourseFramework-1.0.jar:. nlp/framework/discourse/EntityExperiments "/home/ksmith/workspace/EntityFramework/python/discourse/data/test" "English" "false" "false"



stanford-corenlp-3.3.0.jar  
stanford-corenlp-3.3.0-models.jar
Entity Grid

java -classpath  DiscourseFramework.jar nlp/framework/discourse/EntityGridFramework inputfile 
"C:\\SMT\\datasets\\wmt14\\output\\graph_model\\graph\\test\\docs\\" "English" "false" "false" 

Entity Graph