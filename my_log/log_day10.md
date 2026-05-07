  503  conda install -c conda-forge ncurses=6.4
  504  conda install -c bioconda --force-reinstall samtools
  505  cd references/hg19/
  506  samtools view -bS output.sam > output.bam
  507  samtools --version
  508  conda activate bioinfo
  509  conda install -c conda-forge ncurses=6.4
  510  conda install -c bioconda --force-reinstall samtools
  511  ls -lh
  512  cd references/
  513  ls -lh
  514  cd ~
  515  find -name 'output*'
  516  cd references/hg19/
  517  ls -lh
  518  rm output.bam 
  519  ls -lh
  520  samtools view -bS output.sam > output.bam
  521  cd ~
  522  conda create -n bioinfo_fix -c bioconda -c conda-forge samtools ncurses=6.4
  523  conda activate bioinfo_fix
  524  cd references/hg19/
  525  samtools view -bS output.sam > output.bam
  526  ls -lh
  527  samtools sort output.bam -o output.sorted.bam
  528  samtools index output.sorted.bam
  529  ls -lh
  530  cd ~
  531  conda install -c bioconda bcftools
  532  cd references/hg19/
  533  # 1. ทำ pileup และ call variants พร้อมกันในครั้งเดียว
  534  bcftools mpileup -f hg19.fa output.sorted.bam | bcftools call -mv -Ob -o output.raw.bcf
  535  # 2. แปลงไฟล์ BCF เป็น VCF (อ่านด้วยตาเปล่าได้)
  536  bcftools view output.raw.bcf > output.vcf
  537  ls -lh
  538  grep -v "^#" output.vcf | head -n 10
  539  grep -v '^#' output.vcf | head -n 10
  540  wc -l output.vcf 
  541  less output.vcf 
  542  cd ~
  543  cd learn/bioinformatic/week003
  544  ls -lh
  545  cd ..
  546  mkdir /week004
  547  mkdir week004
  548  ls -lh
  549  cd week004
  550  history | tail -n 30 > log_day10.md
  551  code log_day10.md 
  552  history | tail -n 50 > log_day10.md
