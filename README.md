#####Introduction#####

#AGTAR is a program that uses adapted genetic algorithm to assemble and quantify transcripts based on RNA-seq data.

#####Requirements#####

#AGTAR is suitable for Linux systems, and requires python2.7(we tested AGTAR based on the Redhat6.8 system environment, python2.7.13).If the result file mapping reads to genome is in "bam" format, you need to install SAMtools (http://samtools.sourceforge.net/#samtools) to convert "bam" format to "sam" format.In addition, the pysam module is required. We have provided a pysam installation package that can be used for local installation, or it can be installed by executing the following command:

pip install pysam

#####Installation#####

#Unzip the downloaded installation package

unzip AGTAR-main.zip

#Add the location of bin directory to the system path.If the AGTAR folder is located at /opt/biosoft/AGTAR-main, then execute the following command:

echo 'PATH=$PATH:/opt/biosoft/AGTAR-main/bin/' >> ~/.bashrc

source ~/.bashrc

#Executing the software with the parameter -h can list all the options:

AGTAR -h

#####Example#####

#Switch to the sample data folder and unzip:

cd /opt/biosoft/AGTAR-main/test_example

unzip tophat.zip 

unzip AGTAR.zip 

unzip accepted_hits.zip

unzip accepted_hits.sam.zip

unzip accepted_hits.sam.junc.zip

#Convert bam file to sam file:

samtools view -h -o accepted_hits.sam  ./tophat/accepted_hits.bam

#Use processesam module to generate .junc.bed file and .instance file:

processsam  -a accepted_hits.sam

#Assembling and quantifying transcripts with AGTAR:

AGTAR  -o AGTAR -p 8  -c accepted_hits.sam.junc.bed -t accepted_hits.sam.instance ./tophat/accepted_hits.bam

#####Arguments#####

#processsam argument:

-a/--annotation    

Output annoation files,including read coverage (.real.wig), read coverage considering junctions and paired-end read spans (.wig),instance range and boundary(.bound.bed),junctions(.bed) and junction summary (.junction.bed).

#Please refer to http://alumni.cs.ucr.edu/~liw/cem.html for more details on the use of processesam.

 

#AGTAR arguments:

Usage: AGTAR [options] <input.bam/.sam>

Version: AGTAR V1.0

Options:

--version					

show program's version number and exit

-h, --help				

show this help message and exit

-o OUTPUT_DIR, --output_dir=OUTPUT_DIR    

Set the storage path of the output file. (default:./)

-G GTF, --GTF=GTF                        

Use the reference transcript annotation file to guide transcript assembly(GTF format).

-p NUM_THREADS, --num-threads=NUM_THREADS

Specify the number of processes to use for transcript assembly. (default:1)

-c JUNCTION, --junction=JUNCTION

The junction file output by the processsam module(must be given).

-t INSTANCE, --instance=INSTANCE
    
The instance file output by the processsam module(must be given).

-n POPULATION_CAPACITY,--population-capacity= POPULATION_CAPACITY 
    
Set population capacity of genetic algorithm.(default:100)

-m MAXIMUM_EVOLUTIONARY_GENERATION,--maximum-evolutionary-generation=MAXIMUM_EVOLUTIONARY_GENERATION

Set the maximum evolutionary generation number of genetic algorithm.(default:100)

-C CROSSOVER-PROBABILITY, --Crossover-probability=CROSSOVER-PROBABILITY
   
Set the initial crossover probability.(default:0.7)

-M MUTATION-PROBABILITY, --Mutation-probability=MUTATION-PROBABILITY
   
Set the initial mutation probability.(default:0.05)

-j JUNC, --junc=JUNC                   
    
Set the minimum junction coverage. (default:1)

-a ANCHOR, --min-anchor=ANCHOR
   
Set the minimum anchor length on each side of the junction. (default:8)

-I MAX_INTRON_LENGTH, --max-intron-length=MAX_INTRON_LENGTH
   
Set the maximum length of the unannotated introns.(default:200000)

-i MIN_INTRON_LENGTH, --min-intron-length=MIN_INTRON_LENGTH
   
Set the minimum length of the unannotated introns.(default:5)





