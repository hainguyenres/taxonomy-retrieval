# taxonomy-retrieval
This tool will take in a list of genus and species in a text file and it will retrieve the full lineage (ranks: kingdom, phylum, class, order, family, genus, species) for each entry in the list.

The script does not rely on sending multiple queries to the NCBI database to retrieve taxonomy.  This is the ideal approach as you do not want to overload their servers with requests.

The script uses bash, python3, the ete3 library, and a downloaded copy of the NCBI taxonomy database.  It is best if you run it in Linux or Mac OS environment with access to a bash terminal.

---------------------------------------------

SETTING IT UP

Download and install miniconda3, a software package manager
https://docs.conda.io/en/latest/miniconda.html

Open a terminal and use conda to install ete3 in a new environment
conda create -n ete3
conda activate ete3
conda install -c etetoolkit ete3

---------------------------------------------

EXECUTE

Make sure you are inside the ete3 conda environment
conda activate ete3

1. go into the folder "1_convert_to_taxids"
cd ./1_convert_to_taxids

2. put your list of genus and species that you want to recover the full taxonomic lineage (using the NCBI taxonomy database) into the text file called "species.txt"

3. Execute these bash commands.  It converts your genus and species name into the NCBI Tax ID. It might be slow when you run it for the first time because it has to download the NCBI taxonomy database locally (taxdump.tar.gz).  If you give it an invalid genus and species name or one it doesn't recognize, it will print "WARN" in taxids_output.txt.  The second command with awk and sed only keeps the lines with the NCBI Taxonomy ID and replaces any line that says WARN with the number 1.

bash readline.sh &> taxids_output.txt

awk '!/NCBI/ && !/Parsing/ && !/Inserting taxids/'  taxids_output.txt | sed 's/.*WARN.*/1/' > taxids_output.fixed.txt

4. Copy or move the taxids_output.fixed.txt into the folder "2_recover_lineage"
cp taxids_output.fixed.txt ../2_recover_lineage

5. Go into the /2_recover_lineage/ folder and run the python script
cd ../2_recover_lineage/
python get_ncbi_taxonomy.py

6. open the tab delimited file produced called full_ranks.txt in MS Excel

NOTE: if you give it something that does not exist in NCBI taxonomy database, you won't get any lineage reported back.  It will show as <not present>.  The NCBI taxonomy database knows how to deal with synonyms.
