# Set up Github
- สมัครบัญชี GitHub
- สร้างคลังเก็บงาน (Repository): ไปที่ GitHub แล้วสร้าง Repository ใหม่ ชื่อว่า Learning-Bioinfo
- เชื่อมต่อเครื่องเข้ากับ GitHub: เปิด WSL (Ubuntu) แล้วรันคำสั่ง:
  - `git config --global user.name "ชื่อของคุณ"`
  - `git config --global user.email "อีเมลของคุณ"`
- อัปโหลดงานครั้งแรก
  - cd ~/Learn/bioinfo  # ไปที่โฟลเดอร์งานของคุณ
  - git init            # เริ่มต้นระบบ Git ในโฟลเดอร์นี้
  - git add .           # เลือกไฟล์ทั้งหมด (ยกเว้นไฟล์ใหญ่ๆ)
  - git commit -m "First commit: Start learning PGT-A"
  - git remote add origin https://github.com/ชื่อของคุณ/Learning-Bioinfo.git
  - git push -u origin main
- เมื่อไปอยู่อีกเครื่องหนึ่ง (The Other PC)
  - git clone https://github.com/ชื่อของคุณ/Learning-Bioinfo.git
- **3. วงจรการเรียนให้ต่อเนื่อง (Daily Workflow)
นี่คือสิ่งที่คุณต้องทำเพื่อให้งานเชื่อมกันตลอดครับ:

ก่อนเลิกเรียน (เครื่อง A): 1. git add . (เตรียมไฟล์)
2. git commit -m "Update lesson: IGV and Samtools" (บันทึกว่าทำอะไรไป)
3. git push (ส่งงานขึ้น Cloud)

ก่อนเริ่มเรียน (เครื่อง B):

git pull (ดึงงานล่าสุดลงมา) ... แล้วเรียนต่อได้เลยครับ!

⚠️ ข้อควรระวังสำหรับ Bioinformatics
ไฟล์ข้อมูลจำพวก .fastq, .bam, .sam มักจะมีขนาดใหญ่มาก (หลักหลายร้อย MB หรือ GB) ซึ่ง GitHub ไม่ค่อยชอบไฟล์ใหญ่ครับ

วิธีแก้: ให้สร้างไฟล์ชื่อ .gitignore ไว้ในโฟลเดอร์งาน `nano .gitignore` แล้วพิมพ์ชื่อนามสกุลไฟล์ที่ใหญ่ๆ ลงไป เช่น *.bam หรือ *.fastq
สร้างไฟล์ชื่อ .gitignore ไว้ในโฟลเดอร์งานแล้วพิมพ์ข้อความนี้ลงไป
# Ignore large data files ไม่เก็บไฟล์ข้อมูลดิบและไฟล์ผลลัพธ์ขนาดใหญ่
*.bam
*.bai
*.sam
*.fastq
*.fq
*.gz

# Ignore temporary files
*.tmp
.DS_Store

# ไม่เก็บโฟลเดอร์ที่เป็นโปรแกรมหรือ Environment
miniconda3/
.conda/

ผลลัพธ์: Git จะเก็บเฉพาะ "โค้ด" และ "บันทึก (Logbook)" ของคุณ ส่วนไฟล์ BAM ขนาดใหญ่ แนะนำให้ใช้วิธี Copy ใส่ External SSD หรือดาวน์โหลดใหม่ในเครื่องที่สองแทน
  
  สร้างกุญแจดิจิทัล (SSH Key)
เพื่อให้คุณไม่ต้องพิมพ์รหัสผ่านทุกครั้งที่ push งาน ให้รันคำสั่งนี้ใน WSL (Ubuntu) บนเครื่อง MSI ครับ:
# สร้างกุญแจใหม่ (กด Enter ผ่านไปเรื่อย ๆ จนเสร็จครับ)
ssh-keygen -t ed25519 -C "your_email@example.com"

# แสดงรหัสกุญแจที่สร้างขึ้น
cat ~/.ssh/id_ed25519.pub
  ให้คุณก๊อปปี้ข้อความที่ยาว ๆ ที่ขึ้นต้นด้วย ssh-ed25519... ไปแปะใน GitHub เมนู Settings > SSH and GPG keys > New SSH key ครับ
  
 สร้างโปรเจกต์เรียนรู้ (First Repo)
ตอนนี้เราจะสร้างโฟลเดอร์ในเครื่องเพื่อเก็บโค้ดและ Logbook ทั้งหมดครับ:
mkdir -p ~/Learn/bioinfo_project
cd ~/Learn/bioinfo_project

ผูกพิกัด Git (The First Push)
รันคำสั่งเหล่านี้เพื่อส่งไฟล์ README.md ขึ้นไปบนบ้านหลังใหม่ของคุณครับ:
git init
git add README.md
git commit -m "Initialize my learning journey"
git branch -M main
# เปลี่ยน [username] เป็นชื่อที่คุณสมัคร GitHub ไว้ครับ
git remote add origin git@github.com:[username]/Learning-Bioinfo.git
git push -u origin main

ทดสอบการเชื่อมต่อ
เพื่อให้แน่ใจว่าทุกอย่างพร้อมรบ ลองพิมพ์คำสั่งนี้ใน Terminal ครับ:ssh -T git@github.com 
ถ้ามันขึ้นว่า Hi [YourUsername]! You've successfully authenticated... แสดงว่า คุณทำสำเร็จ 100% ครับ

ลองส่งไฟล์แรกขึ้นไปบน GitHub จริงๆ กันครับ รันคำสั่งนี้ต่อได้เลย:
# 1. เข้าไปที่โฟลเดอร์งาน (สมมติว่าชื่อ bioinfo_project)
cd ~/Learn/bioinfo_project

# 2. ตั้งค่าโปรเจกต์ (ทำเฉพาะครั้งแรก)
git init
git remote add origin git@github.com:melanee-dok/Learning-Bioinfo.git

# 3. เตรียมไฟล์และบันทึก
git add README.md
git commit -m "First push after successful SSH authentication"

# 4. ส่งงานขึ้น GitHub (วินาทีแห่งประวัติศาสตร์!)
git push -u origin main

# สร้างไฟล์บันทึกวันนี้
echo "# Bioinformatics Learning Log" > README.md
echo "Date: 13 April 2026 - Songkran Day" >> README.md



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
