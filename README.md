# Broad_eukaryote_viruses_reference
Here I'll document how I build my  fasta reference of eukaryote viruses for viral reads detection using RNAseq.  My reference is not as broad as the entire "nt_viruses" but also not restrictive as the refseq viral genomes or specific databases such as "ZOVER".  Here I filter my own database  from "nt_viruses" and I'll remove redundacy with mmseq.

I can't find a public nucleotide fasta viral database that exactly attend my needs. I want a compreheensive database of eukaryotic viral sequences to generate indexes for RNAseq mappings and viral reads detection.
In the "NCBI FTP" (https://ftp.ncbi.nlm.nih.gov/) we have the broad nt_viruses (https://ftp.ncbi.nlm.nih.gov/blast/db/nt_viruses*). This is a vast redundant database that also include phages (probably most of the sequences there are from phages).
One can also find the viral databases avaliable in the refseq folder "https://ftp.ncbi.nlm.nih.gov/refseq/release/viral/". These ones could be a bit restrictive, not including metagenomic assembled viral sequences (I don't wanna miss these ones!).
If you work with specific organisms, semi-curated databases such as "ZOVER" (https://doi.org/10.1093/nar/gkab862) could be enough, but I don't want a database from selected hosts for my "Viral sensor" for RNAseq libraries. 
Here I 'll filter my eukaryote viruses sequences from "nt_viruses" and , for a matter os aligment speed and computational resources use, I will remove reduncies with MMseqs2 (https://github.com/soedinglab/MMseqs2).


1- Download and decompress indexed "nt_viruses"

    #Download
    rsync -avP rsync://ftp.ncbi.nlm.nih.gov/blast/db/nt_viruses*

    #Check the md5 sum files before continuing (example here: https://github.com/nylander/Check_MD5SUMS)
    #Decompress it using your preferred program
    for i in *.tar.gz; do pigz -dc -p 70 ${i} | tar -xvf - && rm -f ${i}; echo "${i} is done!"; done 

2- For our phage filter lets extract  accession + taxid  with blastdbcmd (you can get it here: https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/

    blastdbcmd -db nt_viruses -entry all -outfmt "%a\t%T" > nt_viruses.acc_taxid.tsv

    #fixing the file in case it came separated by the string "\t" and not real TAB
    awk '{gsub(/\\t/, "\t"); print}' nt_viruses.acc_taxid.tsv > nt_viruses.acc_taxid.real.tsv

    #Download the taxonkit from here: https://github.com/shenwei356/taxonkit/releases/latest/download/taxonkit_linux_amd64.tar.gz

    #BUild a list of phages IDs
    taxonkit list --data-dir taxdump --ids 10744,2842243,10860,2840022,2731619,2842242,12333 > phage_taxids.txt

    # Build the list of accessions to KEEP (non-phage)
    awk '
    FNR==NR {bad[$1]=1; next}
    !bad[$2] {print $1}
    ' phage_taxids.txt nt_viruses.acc_taxid.tsv > nonphage.acc.txt

    # Extract the filtered FASTA directly from the BLAST DB
    blastdbcmd -db nt_viruses \
    -entry_batch nonphage.acc.txt \
    -out nt_viruses_no_phage.fna

The output of this filter was:

    Initial: 14,481,233
    Removed: 123,568 (~0.85%)
    Kept: 14,357,665
Well, I was expecting a more effective removal, lets proceed.

Do obvious “phage” strings still appear in headers?



     

    






