# Apache Spark
เป็นเครื่องมือหนึ่งที่ถูกดีไซน์มาให้เราใช้งานแบบทำงานกลุ่มได้ โดยที่เชื่อมต่อระบบการทำงานของคอมพิวเตอร์เข้าด้วยกัน หรือเรียกว่า Cluster computing platform ซึ่งสามารถกระจายงานที่ต้องทำไปยังเครื่องอื่นๆภายในระบบ ทำให้เราสามารถประมวลผลข้อมูลขนาดใหญ่แบบเต็มประสิทธิภาพ หรือแบบ real-time ไปพร้อม ๆ กันได้ ตัวชูโรงของ Spark คือการใช้คอนเซ็ปต์ RDD

![500](../../_assets/data_science/distributed_systems/apache_spark/apache_spark_logo.png)

## Resilient Distributed Datasets (RDD)
คือโครงสร้างข้อมูลพื้นฐานของ Spark เป็นชุดข้อมูลไม่สามารถเขียนทับได้ (immutable) ทนต่อการล่มได้เพราะจะเก็บประวัติการเปลี่ยนแปลงของข้อมูลไว้ในหลายโหนด หากมีโหนดไหนเสียไปก็สามารถเรียกจากอีกโหนดได้

แต่ละ Dataset ใน Spark จะถูกแบ่งเป็น partition ทั่วทั้ง cluster และยังสามารถทำงานแบบขนานกันได้ (parallel) ระหว่าง node ใน cluster โดยข้อมูลเหล่านี้ถูกสร้างขึ้นโดยการกำหนดการทำงานว่า
1. ทำบนข้อมูลบน stable storage
2. ทำด้วย mutable และ immutable collection ที่มีอยู่
3. ทำด้วยไฟล์ภายนอกใน Hadoop HDFS หรือเป็นระบบที่รองรับ
ข้อมูลเหล่านี้จะถูกเก็บบนหน่วยความจำ (Memory) ดังนั้นจึงสามารถเรียกใช้ซ้ำได้อย่างรวดเร็ว และมีประสิทธิภาพ

มี Key feature หลัก ๆ ดังนี้
### Lazy Evaluation
คำสั่งแบบ Transformation ทั้งหมดจะไม่ทำงานเลย (Lazy) แต่จะเก็บเป็นประวัติไว้ก่อน Spark จะประมวลผลเมื่อมีคำสั่งแบบ Action มาสั่งให้ทำ

### In-Memory Computation
ข้อมูลถูกเก็บไว้ใน RAM (random access memory) แทนที่จะเก็บไว้ใน disk ส่งผลให้ใช้พื้นที่จัดเก็บข้อมูลน้อยลง เมื่อทำซ้ำ ๆ จะสามารถจำ pattern ทำให้วิเคราะห์ข้อมูลขนาดใหญ่ได้มีประสิทธิภาพมากขึ้น

### Fault Tolerance
ข้อมูลดังจะถูกเก็บไว้ท่ามกลาง Spark executor ใน worker node ที่อยู่ใน cluster และสามารถติดตามข้อมูลที่เสียหายถูกเก็บในที่อื่น ๆ ได้จึงทำให้สามารถสร้างใหม่ได้ทันทีเมื่อข้อมูลเสียหาย

### Immutability
สามารถป้องกันปัญหาที่จะเกิดขึ้นเมื่ออัปเดตข้อมูลจากการทำงานหลาย thread พร้อมกัน การมีข้อมูลที่เขียนทับไม่ได้ปลอดภัยเมื่อเราทำงานพร้อมกัน

### Partitioning
มีข้อมูลมากมายที่มีขนาดใหญ่มากจนไม่สามารถเก็บไว้ใน node เดียว และจำเป็นต้องแบ่งออกเพื่อเก็บไว้ใน node ได้ Spark จะทำการแบ่งข้อมูล และจัดเก็บข้อมูลที่ถูกแบ่งออกอัตโนมัติ มีจุดสำคัญที่เกี่ยวข้องกับการแบ่งข้อมูลดังที่แสดงด้านล่าง
- แต่ละ node ใน Spark cluster มี 1 หรือมากกว่า 1 partition
- partition ด้านในไม่ได้ถูกเก็บไว้หลายเครื่อง
- จำนวนของ barrier ใน Spark สามารถปรับแต่งได้ และควรปรับให้เหมาะสม
- สามารถเพิ่มจำนวน executor บน cluster ได้เพื่อจะทำให้มีการทำงานแบบขนานมากขึ้น

### Location Setup Capability
สามารถลบ placement preference เพื่อประมวลผลข้อมูล จึงทำให้สามารถประมวลผลข้อมูลได้เร็วขึ้น placement preference คือข้อมูลเกี่ยวกับสถานที่เก็บข้อมูล 

## Unified Stack of Apache Spark
โครงสร้าง Spark ประกอบไปด้วยหลายส่วนด้วยกัน คล้ายๆกับการใช้ libraries ในซอร์ฟแวร์อื่นๆ เพื่อรองรับกับงานที่หลากหลายได้ กรอบไปด้วย

![](../../_assets/data_science/distributed_systems/apache_spark/unified_stack_of_apache_spark.png)

### Spark Core
มีฟังชั่นเบื่องต้นสำหรับกำหนดงาน จัดการหน่วยความจำ fault recovery การจัดการ storage system และอื่น ๆ อีก เช่น การเรียก API เฉพาะตัวที่เรียกว่า RDDs ซึ่งเป็นสิ่งที่ช่วยในการรันโปรแกรมในหลายๆเครื่อง พร้อมๆกันแบบคู่ขนาน

### Spark SQL
เป็นส่วนที่สามารถใช้ SQL, HQL จัดการกับข้อมูลที่มีโครงสร้างในรูปแบบต่าง ๆ เช่น Hive table, Parquet และ JSON ได้ นอกจากนี้ยังมีความพิเศษตรงที่เราสามารถใช้ SQL queries กับภาษาอื่น ๆ พร้อมกันได้ เช่น Python Java Scala ผ่าน RDDs ในที่เดียว

### Spark Streaming
เป็นส่วนที่ทำให้เราทำงานกับข้อมูลแบบ live streams ได้ ซึ่งมีการใช้ API ในการจัดการที่คล้ายกับ RDDs ทำให้มันง่ายกับโปรแกรมเมอร์ในการสลับไปมาระหว่างข้อมูลในหน่วยความจำ ดิสก์ หรือข้อมูลที่มาแบบ real time

### MLlib
มีฟังชั่นในการทำ Machine Learning ตั้งแต่เบื่องต้นจนถึงแอดวานซ์ ตั้งแต่ Machine Learning algorithms, collaborative filltering รวมถึงการประเมิณผลโมเดล และการนำข้อมูลไปวิเคราะห์

### GraphX
เป็นส่วนที่ใช้จัดการกราฟ เช่น กราฟที่เชื่อมความสัมพันธ์ระหว่างเรากับเพื่อนใน social media สามารถใช้ RDDs ในการควบคุมได้ อีกทั้งยังมี operation และ libraries ต่าง ๆ เข้ามาเสริมด้วย

## ข้อดีของ Apache Spark
- เก็บข้อมูลบนหน่วยความจำ (Memory) แทนการเขียนไฟล์เก็บข้อมูลทุกขั้นตอน
- เก็บการเปลี่ยนแปลงข้อมูล แทนการเก็บข้อมูลที่เปลี่ยนแล้ว
- ทนต่อการล่ม (Fault-tolerant) ด้วย RDD (Resilient Distributed Dataset)
- รองรับหลายภาษา เช่น Python, Java, Scala, R
- มี Component ให้ใช้งานเยอะ เช่น Spark SQl, MLlib, GraphX

# ข้อมูลเพิ่มเติม
- [A Complete Guide to RDD in Apache Spark](https://www.xenonstack.com/blog/rdd-in-spark/)
- [Apache Spark คืออะไร เครื่องมือ Big Data ที่ไม่รู้จักไม่ได้](https://blog.datath.com/apache-spark-big-data/)
- [Introduction to Apache Spark: A Unified Analytics Engine](https://www.oreilly.com/library/view/learning-spark-2nd/9781492050032/ch01.html)
- [PySpark Tutorial](https://www.youtube.com/watch?v=_C8kWso4ne4)






