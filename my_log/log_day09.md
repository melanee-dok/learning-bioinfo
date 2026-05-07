  449  explorer.exe .
  450  cd learn/bioinformatic/week001
  451  ls -lh
  452  mv sample001.fastq.gz /week003
  453  cutadapt -a AGATCGGAAGAGCACACGTCTGAACTCCAGTCA          -q 20          -m 25          -o trimmed_sample001.fastq.gz          sample001.fastq.gz
  454  fastqc trimmed_sample001.fastq.gz -o ./
  455  explorer.exe .
  456  cd ~
  457  find -name "*.fasta"
  458  find -name "*.fastq"
  459  find -name '*log'
  460  find -name '.gz'
  461  mkdir -p ~/references/hg19
  462  cd ~/references/hg19
  463  # ดาวน์โหลดไฟล์แบบรวม (Whole Genome)
  464  wget http://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/hg19.fa.gz
  465  gunzip hg19.fa.gz
  466  ls -lh
  467  bwa index hg19.fa
  468  # ติดตั้ง bwa โดยใช้ conda
  469  conda install -c bioconda bwa
  470  bwa
  471  bwa index hg19.fa
  472  ls -lh
  473  cd ~
  474  cd learn/bioinformatic/
  475  ls -lh
  476  cd week003/
  477  ls -lh
  478  history | tail -n 30 > log_day09.md
