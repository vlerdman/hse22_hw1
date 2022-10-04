# hse22_hw1
Эрдман Владимир БПМИ206
## Комманды на сервере

#### 1. Создание ссылок в папке
```bash
 ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
```
#### 2. Выбор случайных чтений
```bash
seqtk sample -s2006 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s2006 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s2006 oilMP_S4_L001_R1_001.fastq 1500000 > matep1.fastq
seqtk sample -s2006 oilMP_S4_L001_R2_001.fastq 1500000 > matep2.fastq
```
#### 3. Оценка чтений используя FastQC
```bash
mkdir fastqc
ls sub* matep* | xargs -tI{} fastqc -o fastqc {}
```
#### 4. Создание отчета через MultiQC
```bash
mkdir multiqc
multiqc -o multiqc fastqc
```
#### 5. Обрезание чтений
```bash
platanus_trim sub*
platanus_internal_trim matep*
```
#### 6. Удаление лишних файлов
```bash
rm matep1.fastq matep2.fastq sub1.fastq sub2.fastq
```
#### 7. Оценка качества обрезанных чтений используя FastQC
```bash
mkdir fastqc_trimmed
ls sub* matep*| xargs -tI{} fastqc -o fastqc_trimmed {}
```
#### 8. Создание отчета для обрезанных чтений через MultiQC
```bash
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed
```
#### 9. Сбор контиг используя “platanus assemble”
```bash
time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
```
#### 10. Сбор скаффолдов
```bash
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> scaffold.log
```
#### 11. Уменьшение числа промежутков
```bash
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> gapclose.log
```

## Отчёты multiQC
#### Для исходных чтений
![](https://github.com/vlerdman/hse22_hw1/blob/main/img/general.png)
![](https://github.com/vlerdman/hse22_hw1/blob/main/img/scores.png)

#### Для подрезанных чтений
![](https://github.com/vlerdman/hse22_hw1/blob/main/img/general_trimmed.png)
![](https://github.com/vlerdman/hse22_hw1/blob/main/img/scores_trimmed.png)

## Ноутбук с кодом
![ссылка](https://github.com/vlerdman/hse22_hw1/blob/main/src/hse22_hw1.ipynb)
