# hse22_hw1

## Обязательная часть
### Подготовим файлы

- Создадим ярлыки для доступа к данным.

```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
```

- Установим seed (в моем случае это 722) и выбирем случайно 5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs.

```
seqtk sample -s125 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s125 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s125 oilMP_S4_L001_R1_001.fastq 1500000 > matepairs.fastq
seqtk sample -s125 oilMP_S4_L001_R2_001.fastq 1500000 > matepairs2.fastq
```

### Определяем качество 

- С помощью fastQC и multiQC оцениваем качество исходных чтений и выводим по ним общую статистику.
```
mkdir fastqc
ls sub* matepairs* | xargs -tI{} fastqc -o fastqc {}
mkdir multiqc
multiqc -o multiqc fastqc
```


- С помощью platanus_trim и platanus_internal_trim подрезаем чтение по качеству и удаляем адаптеры.
```
platanus_trim sub*
platanus_internal_trim matepair*
```
- Удаляем исходные файлы.
```
rm sub1.fastq
rm sub2.fastq
rm matepairs.fastq 
rm matepairs2.fastq
```

- С помощью  fastQC и multiQC оцениваем качество подрезанных чтений и получаем по ним общую статистику.
```
mkdir fastqc_trim
ls sub* matepairs*| xargs -tI{} fastqc -o fastqc_trim {}
mkdir multqctrim
multiqc -o multqctrim fastqc_trim
```


### Анализ контигов и скаффолдов
-С помощью “platanus assemble” собираем контиги из подрезанных чтений.
```
time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
```
Ссылка на Google Colab: 

Или блокнот вы можете найти здесь: 

- С помощью “platanus scaffold” собираем скаффолды из контигов, а также из подрезанных чтений.
```
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matepairs.fastq.int_trimmed matepairs2.fastq.int_trimmed 2> scaffold.log
```
- Уменьшаем промежутки.
```
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matepairs.fastq.int_trimmed  matepairs2.fastq.int_trimmed 2> gapclose.log
```
- Удаляем подрезанные чтения.
```
rm matepairs.fastq.int_trimmed
rm matepairs2.fastq.int_trimmed
rm sub1.fastq.trimmed
rm sub2.fastq.trimmed
```
- Переносим все вы одну папку

```
mkdir Main
mv Poil* Main/
mv oil* Main/
mv *.log Main/
mv fast* Main/
mv mult* Main/
```


## Дополнительная часть
### Дополнительная сборка 1. Чтений в 2 раза меньше.



- Ссылка на Google Colab та же.

 - Статистики:

### Дополнительная сборка 2. Чтений в 4 раза меньше.

- 

- Ссылка на Google Colab та же.

 - Статистики:
    - До 
    - После 
    
    - До 
  
    - После 
    
    
    
    
 ### Выводы
