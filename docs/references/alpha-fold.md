---
id: alpha-fold
title: Alpha Fold
slug: /references/alpha-fold
---


# Alpha Fold 
by Google DeepMind

AlphaFold 2 (AF2) enabled computational predictions for hundreds of millions of proteins, far more than the ~150,000 experimentally solved structures that were previously known. 

A relatively small DeepMind team created an AI system that achieved near–experimental accuracy for many proteins and scaled predictions to the entire known protein universe.


Generated computationally in weeks, not a century.
Extremely valuable, but:
some are high confidence
many are partial confidence
some are low confidence
Predictions still need experimental validation.





FINAL?

In 2021, a team of 15 at Google DeepMind stood on 100 years of scientific work, and used AlphaFold 2 to expand humanity’s access to protein structure information by well over ~1000×, going from ~150,000 experimentally solved structures to ~200 million computational predictions.

Important caveat: it does not replace the structure validation that has occured during those past 100 years, but does leverage that century of data to scale structural knowledge in an unprecedented way. Protien structures and related interactions are now possible digitally, which enables a new path to identifying potential interactions without the previously laborious process.

Check out [Veritasium's incredible explanation for how AlphaFold works](https://www.youtube.com/watch?v=P_fHJIYENdI) to gain a better understanding. 

Incredibly, this work is available freely through the [AlphaFold Protien Structure DB](https://alphafold.ebi.ac.uk/) (see: [related paper](https://pubmed.ncbi.nlm.nih.gov/37933859/)) which allows for searching protien, gene, UniProt accession / organism / sequence. 



-------

Building on the earlier work of AF2, [AlphaFold 3](https://www.isomorphiclabs.com/articles/alphafold-3-predicts-the-structure-and-interactions-of-all-of-lifes-molecules?utm_source=chatgpt.com) improves accuracy and adds capabilities around predicting structures of molecular complexes: proteins interacting with other proteins, nucleic acids (DNA/RNA), small‐molecules/ligands, ions, and modified residues.

Modelling how a therapeutic antibody or how a small molecule drug might dock makes AF3 more useful for realistic biological/therapeutic contexts, ultimately expanding the “real-world utility” of the tool from predominantly structural biology into biomedical/drug discovery space. 

The [AlphaFold Server](https://alphafoldserver.com/welcome) was recently published which provides generation of highly accurate biomolecular structure predictions containing proteins, DNA, RNA, ligands, ions, and also model chemical modifications for proteins and nucleic acids

Although the scientific community has expressed concerns regarding access to the research, the [model/weights](https://github.com/google-deepmind/alphafold3) were open sourced in November 2024. 

