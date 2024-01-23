# Genepuller-Fungal-MLST
Blast wrapper script for pulling relevant genes from fungal (or indeed any) genomes for MLST

Fungal taxonomy based on multi-locus sequenced typing (MLST) is a tricky business. While there are more convenient and automated ways of doing whole-genome trees for fungi (for example, see https://github.com/stajichlab), most fungal genomes haven't been sequenced; in most cases, Sanger sequencing has been performed on specific marker genes (most often the ITS sequence, but also other marker genes such as calmodulin, translation elogation factor 1, etc.). Finding the best taxonomic placement of an unknown fungal WGS often relies on extracting the genus-appropriate marker genes from the genome and placing them in a tree with the same marker genes from related species or genera. 

However, because fungi are so tricky to properly ID, sequencing databases are rife with fungal misclassification. Choosing examples of the genes you're looking for depends on choosing appropriate examples of those marker genes in at least semi-closely-related species, and this requires you, the bioinformatian, to carefully assess whether a fungal gene is properly identified (for example, is the sequence from a culture collection - or from a lab that doesn't usually do fungal work?). There are two ways to do it: annotate all genes in the genome using something like Busco, and then find your gene of interest - or pull just the specific genes you want from the genome. This script implements the latter strategy.

What this script does: takes a folder full of fungal genomes and pulls the best match of each marker gene for which you have provided a blast database out. This information is displayed in a spreadsheet showing the best match for functional gene in each genome. Also provided is a separate spreadsheet for each marker gene showing more detailed results for each genome. The script also pulls out that best match area from your fungal genomes, so that you can check it against the NCBI, JGI, etc databases.

Using this script is designed to be an iterative process, where more and more sequences are progressively added to your database after the results of the previous run. 

How it works:
1. You provide the script with a folder containing a subfolder for each marker gene you're interested in, with the name of the gene in question. I've provided an example of a set of databases I was working with. You will want to make your own! However, keep the ITS1 database - that's a reformatted version of the UNITE database (https://unite.ut.ee/), and can help you establish initial taxonomy. You also provide the script with a folder of fungal genomes.
2. If you are adding new genes to a database or a new subfolder, choose the option to rebuild the databases and the script will do it for you
3. Run the script - the

# TBD
I am in the process of converting this script from something used in JPL Biotechnology and Planetary Protection R&D that no one else ever looks at, to something useful for anyone else with an MLST problem. TBD: 
1. remove steps having to do with pulling genomes and parsing their standardized ids from our Google Workspace account
2. Implement this as a standalone script with command line options
3. Include better initial set(s) of databases
4. Create more options for parsing the blast results, including reporting multiple good matches
5. Implement the blast jobs to run in parallel to make use of multi-threading


# Script inputs:
organism_type = fungus or bacteria

blast_type = "nucl" or "prot" (are you using the nucleotide or protein database)

update_blastdb = True or False <-- should the databases be remade (if you haven't added anything or otherwise touched them, this can be False)

input_jc_list = name of txt file containing list of jc numbers to be blasted <-- this option was specifically for our lab and will be removed

job_name = identifier which will be use to store results for the job (results will be in a directory under this name, so make it easy to identify, don't call it "results" or something)

filter_factor = what percent length of the reference sequence should the query alignment match? .9 = 90%, for example. Best to start out with this number as a low value (<70%) and see what matches you get, then blast those matches against the NCBI database to see what new sequences to add to the database.
