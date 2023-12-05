# Esenia_genomics


# Population Genomics steps 

## Prepare input files - Index the Reference Genome 
```
cd /nobackup/proj/ejpr/Kelp_Project/Scripts/Genome_FINAL_FINAL/Final_Genome

samtools faidx Eisenia_cockeri_nuclear.fasta

bwa index Eisenia_cockeri_nuclear.fasta
```
## Prepare input files - ONT individuals data for analysis 
```
tar -xvf X204SC22111908-Z01-F002_01.tar
tar -xvf X204SC22111908-Z01-F002_02.tar
tar -xvf X204SC22111908-Z01-F003_01.tar
tar -xvf X204SC22111908-Z01-F003_02.tar

## manually move all individual folders to analysis folder (/nobackup/proj/ejpr/Kelp_Project/Data/PopGen_data/All_raw_files/Individual_folders)
## move into directory 
cd /nobackup/proj/ejpr/Kelp_Project/Data/PopGen_data/All_raw_files/Individual_folders 
## run for loop to concatonate the fq.gz files for each individual and output them to seperate folder. Now ready for analysis. 
LIST=("G1"	"G10"	"G11"	"G12"	"G13"	"G14"	"G15"	"G16"	"G17"	"G2"	"G4"	"G5"	"G6"	"G7"	"G8"	"G9"	"H1_1"	"H1_10"	"H1_11"	"H1_12"	"H1_13"	"H1_14"	"H1_16"	"H1_2"	"H1_3"	"H1_4"	"H1_5"	"H1_6"	"H1_7"	"H1_8"	"H1_9"	"H2_17"	"H2_18"	"H2_19"	"H2_21"	"H2_22"	"H2_23"	"H2_24"	"H2_25"	"H2_26"	"H2_27"	"H2_28"	"H2_29"	"H2_30"	"H3_1"	"H3_10"	"H3_15"	"H3_16"	"H3_17"	"H3_18"	"H3_19"	"H3_2"	"H3_3"	"H3_4"	"H3_5"	"H3_6"	"H3_7"	"H3_9"	"H4_10"	"H4_11"	"H4_12"	"H4_13"	"H4_14"	"H4_15"	"H4_16"	"H4_17"	"H4_2"	"H4_3"	"H4_4"	"H4_5"	"H4_6"	"H4_7"	"H4_8"	"H4_9"	"M1"	"M10"	"M11"	"M12"	"M13"	"M14"	"M15"	"M16"	"M17"	"M18"	"M2"	"M20"	"M21"	"M3"	"M4"	"M5"	"M6"	"M7"	"M8"	"M9D"	"N1"	"N10"	"N11"	"N12"	"N13"	"N14"	"N15"	"N16"	"N17"	"N18"	"N19"	"N2"	"N3"	"N4"	"N5"	"N6"	"N7"	"N8"	"N9"	"P3_1"	"P3_10"	"P3_11"	"P3_12"	"P3_2"	"P3_3"	"P3_4"	"P3_5"	"P3_6"	"P3_7"	"P3_8"	"P3_9"	"PLC10"	"PLC11"	"PLC12"	"PLC14"	"PLC15"	"PLC16"	"PLC17"	"PLC18"	"PLC19"	"PLC7"	"PLC8"	"PLC9"	"PZ1"	"PZ10"	"PZ11"	"PZ12"	"PZ13"	"PZ14"	"PZ2"	"PZ3"	"PZ4"	"PZ7"	"PZ8")

echo  ${LIST[@]}

for d in ${LIST[@]}; do
    echo ${d}
    cd ${d}
    cat *1.fq.gz > ${d}_1.fq.gz
    cat *2.fq.gz > ${d}_2.fq.gz
    zcat ${d}_1.fq.gz | wc | awk '{print $1/4}'
    zcat ${d}_2.fq.gz | wc | awk '{print $1/4}'
    mv ${d}_1.fq.gz ../../Concatonated_ALL_indviduals/
    mv ${d}_2.fq.gz ../../Concatonated_ALL_indviduals/    
    cd ..
done

```
## Check read quality using FastQC

## Trimm Reads 

## Align Reads to reference genome 
```
for f1 in *_1.fq
do
        echo ${f1}
        #f2=${f1}"_2.fq"
        #bwa mem pb_genome_subset.fa $f1 $f2 > $f1.sam 
done

# convert sam to bam
# CHECK THIS LOOP WORKS  
for i in *.sam
do
samtools view -bh ${i}.sam > ${i}.bam
done

# sort the bam file and make an index 
for i in *.bam
do
samtools sort -o ${i}_sorted.bam ${i}.bam
samtools index ${i}_sorted.bam
done

```
## Filter the alignment 
## Variant Calling  
