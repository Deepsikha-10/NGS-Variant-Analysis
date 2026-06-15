# installation
1.	install sra-toolkit 
2.	fastq-dump --split-files --gzip DRR013000

# quality control

3.	wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.12.1.zip

4.	/home/rgitbt/Desktop/SDA1/tools/FastQC/fastqc
           /home/rgitbt/Desktop/SDA1/raw_reads/DRR013001_1.fastq.gz
           -o /home/rgitbt/Desktop/SDA1/fastqc/before

# trimming using Trimmomatic

1.	wget https://github.com/usadellab/Trimmomatic/releases/download/v0.39/Trimmomatic-0.39.zip

2.	java -jar /home/rgitbt/Desktop/SDA1/tools/tools1/Trimmomatic-0.39/trimmomatic-0.39.jar PE /home/rgitbt/Desktop/SDA1/reads/DRR013000_1.fastq.gz /home/rgitbt/Desktop/SDA1/reads/DRR013000_2.fastq.gz DRR013000_1_paired.fastq.gz DRR013000_1_unpaired.fastq.gz DRR013000_2_paired.fastq.gz DRR013000_2_unpaired.fastq.gz ILLUMINACLIP: /home/rgitbt/Desktop/SDA1/tools/tools1/Trimmomatic-0.39/adapters/TruSeq3-PE.fa:2:3:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50

3.	/home/rgitbt/Desktop/SDA1/tools/FastQC/fastqc 
/home/rgitbt/Desktop/ SDA1/trim/DRR013000_1_paired.fastq.gz 
-o /home/rgitbt/Desktop/ SDA1/fastqc/after/

# alignment (indexing)

1.	git clone https://github.com/lh3/bwa.git -> cd bwa -> make

2.	/home/rgitbt/Desktop/SDA1/tools/tools1/bwa/bwa index /home/rgitbt/Desktop/SDA1/whole_genome1/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz

# alignment (mapping)

 1.	/home/rgitbt/Desktop/SDA1/tools/tools1/bwa/bwa mem -t 6 -R'@RG\tID:00\tSM:00' /home/rgitbt/Desktop/SDA1/whole_genome1/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz /home/rgitbt/Desktop/SDA1/trim1/DRR013000_1_paired.fastq.gz /home/rgitbt/Desktop/SDA1/trim1/DRR013000_2_paired.fastq.gz > /home/rgitbt/Desktop/SDA1/bwa1/DRR013000.sam

# post alignment 

 4.	samtools sort -o DRR013000.sorted.bam DRR013000.sam

 5.	java -jar /home/rgitbt/Desktop/SDA1/tools/tools1/bwa/picard/build/libs/picard.jar MarkDuplicates -I /home/rgitbt/Desktop/SDA1/bwa1/DRR013000.sorted.bam -O DRR013000_RM.bam -M marked_dup_metrics.txt --REMOVE_DUPLICATES

# variant calling

 •	freebayes -f /Users/deeps/Desktop/SDA/whole_genome/Homo_sapiens.GRCh38.dna.primary_assembly.fa -r 8 /Users/deeps/Desktop/SDA/remove_duplicates/*.bam> variants_8.vcf

# filtering 
 
•	awk '{if (($7 == "1/1" && $9 == "1/1" && $11 == "1/1") &&($6 == "0/0" && $8 == "0/0" && $10 == "0/0") && ($13 >= 10 && $14 >= 10 && $15 >= 10 && $16 >= 10 && $17 >= 10 && $18 >= 10)) print $0}' variants_8_modified.vcf > mutations.vcf

•	bcftools view -i 'MIN(FMT/DP)>5' variants_8.vcf > variants_8_DP.vcf

•	bcftools view -g ^het variants_8_DP.vcf > variants_8_DP_Hom.vcf

•	bcftools query -f '%CHROM\t%POS\t%REF\t%ALT\tGT:[\t%GT]\tDP:[\t%DP]\n' variants_8_Hom.vcf > variants_8_modified.vcf

