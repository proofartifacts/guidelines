### Introduction
The goal of the proof-artifact evaluation is to recognize and promote high-quality proof artifacts, as well as verify that the mechanized formalization of a paper
* faithfully represents the definitions and theorems in the paper that were claimed to be mechanized, and
* contains complete proofs for all theorems, except for clearly marked and documented  hypotheses.

Authors should organize and document their artifact sufficiently to ensure that a reviewer can complete their assessment within one day.

Reviewers will be aiming to accept the artifact if
* it can be reviewed in the allocated time;
* it is straightforward to understand how the mechanized proofs correspond to the definitions and theorems in the paper;
* the proofs of all theorems are complete (except as explained in the paper or artifact documentation).

### Guidelines for Authors

To simplify the review process, we suggest that authors include the following in their proof artifacts:
1. Step-by-step instructions on how to build and compile the proof:
 * proofs can be precompiled and submitted on a VM;
 * list the version numbers of the proof assistant and of the libraries that the proof depends on;
 * we recommend to additionally provide instructions on how to build the proof from scratch, preferably in the form of a makefile.

2. A paper-to-artifact correspondence guide:
 * explain how each of the definitions and theorems in the paper corresponds to the formalizations in the artifact;
 * example:

|  Definition / Theorem                          | Paper                   | File   |  Name of formalization | Notation   |
|------------------------------------------------|-------------------------|--------|------------------------|------------|
|  Type system of the<br>simply typed λ-calculus |  Page 5,<br>Figure&nbsp;2  | stlc.v |  `Inductive typing`    | `G ⊢ t: T` |
|  Preservation Theorem                          |  Page 7,<br>Theorem&nbsp;1 | stlc.v |  `Theorem preservation`|            |


3. An overview of the libraries and proof frameworks that are used in the proof:
 * explain in what way the proof relies on existing frameworks;
 * the goal is to make the proof readable by a reviewer who is unfamiliar with the frameworks;
 * example: 
  
   _This type-safety proof uses the locally nameless representation for terms and cofinite quantification in typing judgements (Aydemir et al., 2008). …_

4. An outline of the proof structure and organization:
 * explain the purpose of the different proof files;
 * optionally, provide a dependency diagram of the proof files.

5. Document the definitions and important lemmas in the proof:
 * give the main theorems in the proof descriptive names that correspond to their names in the paper;
 * we suggest using tools that generate human-readable documentation (such as 
   [coqdoc](http://manpages.ubuntu.com/manpages/xenial/man1/coqdoc.1.html),
   [isabelle document](https://isabelle.in.tum.de/doc/system.pdf), 
   [Agda documentation](https://agda.readthedocs.io/en/v2.5.3/contribute/documentation.html?highlight=documentation), etc.).

6. Document all places where the proof formalization differs from the paper, for example, through simplifications or alternative definitions;
  * example (formalizing terms of the λ-calculus in Coq):
```coq
Inductive term := 
... 
(** The locally nameless representation represents
    bound variables through de Bruijn indices; therefore
    we can represent an abstraction λx.t as λt’ *)
| abs : term -> term
...
```
7. Explicitly state and justify all axioms, assumptions, and unfinished parts of the proof:
 
   _We encourage researchers to spend their time on the most important part of the work instead of reproving well-known results. At the same time, given the limited time for reviewing the artifact, it is difficult for the reviewer to assess whether the modified formalization is equivalent to the paper formulation, whether that modification is significant, and whether a lemma with an incomplete proof is obvious._
  * clearly mark and justify all unproved hypotheses in the proof, such as
    - known results that can be cited,
    - axioms that change the logic,
    - obvious lemmas that are tedious to prove (but keep in mind that “obvious” lemmas might not be obvious to reviewers, and that they might not be true);
  * examples:
    - ```coq
      (** We assume the four colour theorem
          [Chartrand & Lesniak 2005] *)
      Axiom four_colour_theorem : …
      ```
    - ```coq
      (** We extend the logic with functional extensionality,
          a well-known axiom that is proved to be consistent
          with the calculus of inductive constructions *)
      Require Import FunctionalExtensionality.
      ```
    - provide instructions on how to automatically determine what axioms and unproved hypotheses were used;
    - example: 
      _To verify that there are no added assumptions, run_<br>`grep postulate *.agda`


### Guidelines for Reviewers

- Follow the step-by-step instructions to build the proof.

- Verify that the code compiles.

- Check whether the code contains undocumented assumptions or unfinished proofs, such as the use of `Axiom` or `Admitted.` in Coq, `postulate` or `{!!}` in Agda.

- Familiarize yourself with the artifact documentation (proof structure, used frameworks and libraries, representations of data types, stated axioms and hypotheses, etc.) to the extent that is necessary to understand whether the formalizations in the proof and paper are equivalent, modulo the possible deviations that are documented in the paper or artifact.

- Compare the definitions and theorems in the paper to the ones in the proof. 

- Evaluate the artifact based on the following criteria:
  * *Readability*: Were you able to understand the mechanized definitions and theorem statements?
  * *Fidelity*: Do the mechanized definitions and theorems correspond precisely to those in the paper? If not, are the points of the departure justified?
  * *Completeness*:  Does the artifact formally verify everything that the paper claims it does?

### Additional Resources
- [How to review formalized mathematics](http://math.andrej.com/2013/08/19/how-to-review-formalized-mathematics/) by Andrej Bauer
- [Checking machine-checked proofs](https://project.inria.fr/coqexchange/checking-machine-checked-proofs/) by Assia Mahboubi
- [Guidelines for Packaging AEC Submissions](http://www.artifact-eval.org/guidelines.html)
- [About Artifact Evaluation](http://www.artifact-eval.org/about.html)

### Acknowledgments
I would like to thank Robert Rand, Derek Dreyer, Robby Findler, Benjamin Pierce, Stephanie Weirich, Daniel Selsam, Marco Vassena, Daniel Schoepe, Ilya Sergey, Abel Nieto, Ralf Jung, Hoang-Hai Dang, Maria Christakis and Philipp Haller for all their feedback which helped greatly improve these guidelines.
