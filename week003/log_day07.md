  360  git remote add origin git@github.com:melanee-dok/Learning-Bioinfo.git
  361  git add README.md
  362  echo "# Bioinformatics Learning Journey" > README.md
  363  ls -lh
  364  git add README.md
  365  git commit -m "First commit: Initializing my lab notes"
  366  git config --global user.email "melanee.dok68@gmail.com"
  367  git config --global user.name "melanee-dok"
  368  git commit -m "First commit: Initializing my lab notes"
  369  git push -u origin main
  370  git branch -M main
  371  git push -u origin main
  372  git branch -M master
  373  git push -u origin master
  374  git remote remove origin
  375  git remote add origin https://github.com/melanee-dok/learning-bioinfo.git
  376  git push -u origin main
  377  echo "# learning-bioinfo" >> README.md
  378  git init
  379  git add README.md
  380  git commit -m "first commit"
  381  git branch -M main
  382  git remote add origin https://github.com/melanee-dok/learning-bioinfo.git
  383  git push -u origin main
  384  git remote add origin https://github.com/melanee-dok/learning-bioinfo.git
  385  git push -u origin main
  386  git remote set-url origin git@github.com:melanee-dok/learning-bioinfo.git
  387  git push -u origin main
  388  conda activate bioinfo
  389  cd learn/bioinformatic/
  390  nano .gitignore
  391  git add .gitignore
  392  ls -a
  393  git commit -m "Add gitignore"
  394  git push
  395  pwd
  396  ls -lh
  397  mv /week003
  398  mkdir week003
  399  ls -lh
  400  cd week001
  401  ls -lh
  402  zcat sample001.fastq.gz | head -n 4
  403  zcat sample001.fastq.gz | grep -c "@"
  404  echo $(zcat sample001.fastq.gz | wc -l) / 4 |bc
  405  cd ..
  406  cd .
  407  ls -lh
  408  cd week003/
  409  history | tail -n 50 > log_day07.md
