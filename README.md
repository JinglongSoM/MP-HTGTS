MP-HTGTS data was analyzed following previously reported methods (Hu, 2016; Liang, 2021) but with newly written scripts that tailored for MP-HTGTS pipeline. 

1.	Sequencing reads from Illumina NovaSeq PE150 were de-multiplexed based on the out-layer barcode to generated sequencing files grouped by row. The individual library in each row was further demultiplexed using TranslocPreprocess.pl based on the inner barcode in P7I7 primer.  
2.	The number of reads of each library was normalized to an indicated value in batch using seqtk_batch_fq.py (seqtk need to be installed). 
3.	Assembly the information of all 96 libraries into one metadata and call TranslocWrapper.pl, which generated two types of key files, master files end with “.tlx” that containing the mapping information of all reads and result files end with “result.tlx” that containing only the reads with translocations. As part of the repair outcomes near the bait site (rejoin) were often missed out in the result files, script JoinT.R was used to generate files end with “JoinT.tlx” for K562 libraries.
4.	Filter out the reads in “.tlx”, “result.tlx” and “JoinT.tlx” files that were caused by PCR-mediated template switch based on the second inner barcode in P5I5 primer using filter_MID2.py. 
Note, it is possible to filtered out the switched reads in step 1 by modifying the TranslocoPreprocess.pl.
5.	The filtered “result.tlx” and “JoinT.tlx” files were converted to bedgraph files in batch manner by tlx2bed_batch.py that rely on base scripts tlx2bed.py, tlx2BED.pl and tlx2BED-MACS.pl (put them in same folder).
6.	The number of total reads in master “.tlx” was counted by tlx_unique_total_count.py;  the total junctions and junctions in specific chromosome regions in “result.tlx” and “JoinT.tlx” were counted by tlx_multiple_hotspots.py; the number of interchromsome translocations was counted by tlx_interchr_TL.py.
7.	The degree of DNA end resection and the outcome of repair pattern were analyzed by ResectionRSS.R and JctStructure.R, respectively. And if required, the genome-wide translocation profile was generated by circos plot toolkit (Liang, 2021). 

Ref:
Hu J, Meyers RM, Dong J, Panchakshari RA, Alt FW, Frock RL. Detecting DNA double-stranded breaks in mammalian genomes by linear amplification-mediated high-throughput genome-wide translocation sequencing. Nat Protoc. 2016 May; 11(5): 853-71.
Liang Z, Kumar V, Le Bouteiller M, et al. Ku70 suppresses alternative end joining in G1-arrested progenitor B cells. Proc Natl Acad Sci U S A. 2021 May; 118(21): e2103630118.