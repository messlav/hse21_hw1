## Домашнее задание №1

1. Линкуем данные в папку с дз
```bash
mkdir hw1
cd hw1
ls -1 /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
```
2. Выбираем случайные чтения
```bash
seqtk sample -s316 oil_R1.fastq 5000000 > pe_R1.fastq
seqtk sample -s316 oil_R2.fastq 5000000 > pe_R2.fastq
seqtk sample -s316 oilMP_S4_L001_R1_001.fastq 1500000 > mp_R1.fastq
seqtk sample -s316 oilMP_S4_L001_R2_001.fastq 1500000 > mp_R2.fastq
```
3. Удаляем ненужные нам уже файлы
```bash
rm oil_R1.fastq
rm oil_R2.fastq
rm oilMP_S4_L001_R1_001.fastq
rm oilMP_S4_L001_R2_001.fastq
```
4. С помощью программ fastQC и multiQC оцениваем качество чтений и получаем статистику
```bash
mkdir fastqc
ls *.fastq | xargs -P 4 -tI{} fastqc -o fastqc {}
mkdir multiqc
multiqc -o multiqc fastqc
```
5. Исходные картинки: TODO
6. С помощью программ platanus_trim и platanus_internal_trim подрезаем чтения по качеству и удаляем праймеры
```bash
platanus_trim pe_R1.fastq pe_R2.fastq 
platanus_internal_trim mp_R1.fastq mp_R2.fastq  
```
7. С помощью программ fastQC и multiQC оцениваем качество подрезанных чтений и получаем статистику
```bash
mkdir trimmed
mkdir trimmed_fastqc
ls trimmed/* | xargs -P 4 -tI{} fastqc -o trimmed_fastqc {}
mkdir trimmed_multiqc
multiqc -o trimmed_multiqc trimmed_fastqc 
```
8. Новые картинки: TODO
9. С помощью программы “platanus assemble” собираем контиги из подрезанных чтений
time platanus assemble -o Poil -f trimmed/pe_R1.fastq.trimmed trimmed/pe_R2.fastq.trimmed 2> assemble.log
10. config_analyze in notebook
11. С помощью программы “ platanus scaffold” собираем скаффолды
```bash
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 trimmed/pe_R1.fastq.trimmed  trimmed/pe_R2.fastq.trimmed -OP2 trimmed/mp_R1.fastq.int_trimmed trimmed/mp_R2.fastq.int_trimmed 2> scaffold.log
```
12. Анализ полученных скаффолдов и количество гепов для самого длинного в ноутбуке
13. С помощью программы “platanus gap_close” уменьшаем кол-во гепов
```bash
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 trimmed/pe_R1.fastq.trimmed  trimmed/pe_R2.fastq.trimmed -OP2 trimmed/mp_R1.fastq.int_trimmed trimmed/mp_R2.fastq.int_trimmed 2> gapclose.log
```


