# Broad_eukaryote_viruses_reference
Here I'll document how I build my  fasta reference of eukaryote viruses for viral reads detection using RNAseq.  My reference is not as broad as the entire "nt_viruses" but also not restrictive as the refseq viral genomes or specific databases such as "ZOVER".  Here I filter my own database  from "nt_viruses" and I'll remove redundacy with mmseq.

I can't find a public nucleotide fasta viral database that exactly attend my needs. I want a compreheensive database of eukaryotic viral sequences to generate indexes for RNAseq mappings and viral reads detection.
In the "NCBI FTP" (https://ftp.ncbi.nlm.nih.gov/) we have the broad nt_viruses (https://ftp.ncbi.nlm.nih.gov/blast/db/nt_viruses*). This is a vast redundant database that also include phages (probably most of the sequences there are from phages).
One can also find the viral databases avaliable in the refseq folder "https://ftp.ncbi.nlm.nih.gov/refseq/release/viral/". These ones could be a bit restrictive, not including metagenomic assembled viral sequences (I don't wanna miss these ones!).
If you work with specific organisms, semi-curated databases such as "ZOVER" (https://doi.org/10.1093/nar/gkab862) could be enough, but I don't want a database from selected hosts for my "Viral sensor" for RNAseq libraries. 
Here I 'll filter my eukaryote viruses sequences from "nt_viruses" and , for a matter os aligment speed and computational resources use, I will remove reduncies with MMseqs2 (https://github.com/soedinglab/MMseqs2).


