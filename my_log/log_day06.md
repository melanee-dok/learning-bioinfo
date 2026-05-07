# Duplicate hunting pipeline And IGV & Samtools View
- `conda activate bioinfo` เปิดเข้าห้อง bioinfo ที่สร้างไว้ใน conda
- `samtools sort -n -o namesorted.bam sample002.bam` จัดเรียงข้อมูลในไฟล์ sample002.bam ให้ Read ที่เหมือนกันมาอยู่ใกล้กัน แล้วบันทึกไว้เป็นไฟล์ชื่อ namesort.bam
- `samtools fixmate -m namesorted.bam fixmated.bam` เตรียมข้อมูลตำแหน่งให้พร้อมสำหรับการเช็ค Duplicate
- `samtools sort -o positionsorted.bam fixmated.bam` จัดเรียงตามตำแหน่งโครโมโซม 
- `samtools markdup positionsorted.bam mark_duplicates.bam` ติดป้าย duplicate
- `samtools flagstat mark_duplicates.bam` ได้ผลลัพธ์คือ
  - 1366496 + 0 in total (QC-passed reads + QC-failed reads)
  - 1364632 + 0 primary
  - 1864 + 0 secondary
  - 0 + 0 supplementary
  - 428410 + 0 duplicates
  - 428410 + 0 primary duplicates
  - 1218803 + 0 mapped (89.19% : N/A)
  - 1216939 + 0 primary mapped (89.18% : N/A)
  - 0 + 0 paired in sequencing
  - 0 + 0 read1
  - 0 + 0 read2
  - 0 + 0 properly paired (N/A : N/A)
  - 0 + 0 with itself and mate mapped
  - 0 + 0 singletons (N/A : N/A)
  - 0 + 0 with mate mapped to a different chr
  - 0 + 0 with mate mapped to a different chr (mapQ>=5)
  **Duplicate โดยทั่วไป 1-5% ถือว่าปกติ หากมากกว่า 20% ต้องวิเคราะห์หาสาเหตุ**
  - *Duplicate ที่พบคิดเป็น 31% ของข้อมูลทั้งหมด มีสาเหตุมาจากงาน PGT-A มี DNA ตั้งต้นน้อย จึงต้องมีการ amplification มาก โอกาสที่จะพบ duplicate จึงสูงขึ้นด้วย*
- `samtools view mark_duplicates.bam | head -n 5` ดูเนื้อหาในไฟล์ mark_duplicates.bam
- `samtools index mark_duplicates.bam` สร้างดัชนี(สารบัญ) จะได้ไฟล์ชื่อ mark_duplicates.bam.bai
- `samtools view mark_duplicates.bam | head -n 5` ผลลัพธ์

  - MN01160:162:000HC37GF:1:21102:5025:12125        1040    chrM    1       4       18S58M  *       0       0       AAATAAGACATCACGATGGATCACAGGTCTATCACCCTATTAACCACTCACGGGAGCTCTCCATGCATTTGGTATT    FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFAFFAFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF     NM:i:0  MD:Z:58 AS:i:58 XS:i:56 RG:Z:250927049_EVA_PARKS_1      XA:Z:chr17,-22020709,76M,4;
  - MN01160:162:000HC37GF:1:21108:14319:10802       1040    chrM    1       4       18S58M  *       0       0       AAATAAGACATCACGATGGATCACAGGTCTATCACCCTATTAACCACTCACGGGAGCTCTCCATGCATTTGGTATT    FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFA     NM:i:0  MD:Z:58 AS:i:58 XS:i:56 RG:Z:250927049_EVA_PARKS_1      XA:Z:chr17,-22020709,76M,4;
  - MN01160:162:000HC37GF:1:22108:20960:19129       16      chrM    1       4       18S58M  *       0       0       AAATAAGACATCACGATGGATCACAGGTCTATCACCCTATTAACCACTCACGGGAGCTCTCCATGCATTTGGTATT    FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF     NM:i:0  MD:Z:58 AS:i:58 XS:i:56 RG:Z:250927049_EVA_PARKS_1      XA:Z:chr17,-22020709,76M,4;
  - MN01160:162:000HC37GF:1:23104:19376:16787       16      chrM    1       0       20S56M  *       0       0       TTAAATAAGACATCACGATGGATCACAGGTCTATCACCCTATTAACCACTCACGGGAGCTCTCCATGCATTTGGCA    FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFAFFFFFF     NM:i:1  MD:Z:54T1       AS:i:54 XS:i:54 RG:Z:250927049_EVA_PARKS_1      XA:Z:chr17,-22020707,76M,5;
  -MN01160:162:000HC37GF:1:11110:26485:17711       16      chrM    26      60      76M     *       0       0       CCACTCACGGGAGCTCTCCATGCATTTGGTATTTTCGTCTGGGGGGTATGCACGCGATAGCATTGCGAGACGCTGG    FFFFFFAFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFAFFFFF/FFFFFFFFFFFFFFFFFFFFFFFFFFF     NM:i:1  MD:Z:47G28      AS:i:71 XS:i:47 RG:Z:250927049_EVA_PARKS_1
  - **คอลัมน์,ชื่อเรียก,ความหมาย (แบบเข้าใจง่าย)
    - 1,QNAME,ชื่อ ID ของ Read นั้นๆ (เหมือนเลขที่นั่งบนเครื่องบิน)
    - 2,FLAG,รหัสลับบอกสถานะ (เช่น เป็น Duplicate ไหม? แปะกลับด้านหรือเปล่า?)
    - 3,RNAME,"ชื่อโครโมโซมที่มันไปแปะ (เช่น chr1, chrX)"
    - 4,POS,"ตำแหน่ง ""เบสตัวแรก"" ที่มันเริ่มวางแปะบนโครโมโซม"
    - 5,MAPQ,คะแนนความมั่นใจ (0-60) ยิ่งเยอะยิ่งชัวร์ว่าแปะไม่ผิดที่
    - 6,CIGAR,"บอกว่า Read นี้ ""แหว่ง"" หรือ ""เกิน"" ตรงไหนบ้าง (เช่น 100M คือตรงเป๊ะ 100 เบส)"
    - 7,SEQ,รหัสพันธุกรรมจริง (ATGC...) ที่เราตามหา!
- `samtools idxstats mark_duplicates.bam` ใช้บอกข้อมูลว่ามีโครโมโซมอะไรบ้าง มีกี่ Read โดยอ่านจากไฟล์ .bai
  - chrM    16571    16854   0
  - chr1    249250621       94575   0
  - chr2    243199373       103766  0
  - chr3    198022430       78687   0
  - chr4    191154276       82972   0
  - chr5    180915260       73774   0
  - chr6    171115067       69957   0
  - chr7    159138663       63435   0
  - chr8    146364022       60955   0
  - chr9    141213431       49752   0
  - chr10   135534747       60109   0
  - chr11   135006516       54639   0
  - chr12   133851895       52913   0
  - chr13   115169878       36883   0
  - chr14   107349540       36010   0
  - chr15   102531392       31365   0
  - chr16   90354753        33892   0
  - chr17   81195210        26106   0
  - chr18   78077248        32188   0
  - chr19   59128983        20208   0
  - chr20   63025520        32988   0
  - chr21   48129895        16253   0
  - chr22   51304566        11278   0
  - chrX    155270560       71908   0
  - chrY    59373566        7336    0
  - 0       0       147693 
    - *คอลัมน์ 1 (ชื่อ): ชื่อโครโมโซม (เช่น chr1, chr2, ..., chrX, chrY และ chrM)*
    - *คอลัมน์ 2 (ความยาว): ความยาวทั้งหมดของโครโมโซมนั้น (หน่วยเป็น bp)*
    - *คอลัมน์ 3 (Mapped Reads): คือจำนวนชิ้นส่วน DNA ที่แปะลงบนโครโมโซมนั้นได้สำเร็จ*
    - *คอลัมน์ 4 (Unmapped): จำนวน Read ที่รู้ว่าควรอยู่แถวนี้แต่แปะไม่ลง (ปกติจะเป็น 0)*
  - ลองเทียบ chr1 (94,575) กับ chrY (7,336)
    - สัดส่วน Y ต่อ 1 คือประมาณ 0.07 (หรือ 7%)
    - ในเคสที่เป็น ผู้ชายจริง (XY) สัดส่วน Y ต่อ 1 มักจะอยู่ที่ประมาณ 0.3 - 0.5 (ขึ้นอยู่กับความยาวโครโมโซม)

- ขั้นตอนการใช้งาน IGV 
  1. ติดตั้ง IGV บน Windows
    - ไปที่เว็บไซต์ igv.org/doc/desktop/#Download
    - *เลือกโหลดเวอร์ชัน Windows (แนะนำตัวที่มี Java รวมมาด้วยเพื่อความง่าย)*

  2. เปิดไฟล์จาก Ubuntu เข้าสู่ IGV
    - เมื่อเปิดโปรแกรม IGV ขึ้นมาแล้ว: 
      - เลือก Genome: ที่มุมซ้ายบน ให้เลือกเป็น hg38 (หรือ hg19 ขึ้นอยู่กับว่าตอนทำ Alignment ใช้ตัวไหน แต่ส่วนใหญ่ PGT-A ยุคใหม่จะใช้ hg38)
      - โหลดไฟล์: ไปที่เมนู File > Load from File...
      - หาที่อยู่ไฟล์: เข้าไปที่ Path ของ WSL เช่น:\\wsl.localhost\Ubuntu\home\melaneed\learn\bioinformatic\week002\
      - เลือกไฟล์: เลือกไฟล์ mark_duplicates.bam (โปรแกรมจะเรียกหาไฟล์ .bai โดยอัตโนมัติ)

- 🧬 สิ่งที่จะเห็นใน IGV 
  - เมื่อโหลดเสร็จ ให้ Zoom In เข้าไปในโครโมโซมสักแท่ง:

    - Coverage Track: แถบกราฟด้านบน จะบอกว่าตำแหน่งไหนมี DNA แปะอยู่หนาแน่นแค่ไหน (ถ้าแหว่งไปเลยในงาน PGT-A อาจแปลว่าโครโมโซมขาด/Monosomy)

    - Alignment Track: แถบด้านล่างจะโชว์ Read เป็นเส้นๆ มันจะมาเรียงกันให้เห็นเลยว่าแปะตรงไหนบ้าง

    - Color Code: * ถ้าเห็น แถบสีแดง/น้ำเงิน แทรกอยู่ใน Read แสดงว่าตรงนั้นมี Mutation หรือเบสไม่ตรงกับต้นฉบับ
