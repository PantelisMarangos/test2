
# Lab 2 Findings

To find the total number of files:

>for file in \`find . -name '*.java'`; do echo "$file";  done | wc -l

![Imgur](https://i.imgur.com/vTabY3J.png)

Total files = 3756.

### Directories structure
![Imgur](https://i.imgur.com/tDLOumU.png)

Total directories = 30

- We know that the main job of 'FastJSON' is to convert Java Objects into JSON representation and vice versa. 
- In order to do this, we'd have to look for the files / directories that do the serialization and deserialization:

### Serializer files
> for file in \`find ./fastjson/src/main/ -name '*.java'`;  do echo "$file"; done | grep -i serializer

![Imgur](https://i.imgur.com/vEkxtJJ.png)

To see how many files there are:
> for file in \`find ./fastjson/src/main/ -name '*.java'`;  do echo "$file"; done | grep - i serializer | wc -l

![Imgur](https://i.imgur.com/BUh6TAZ.png)

Total .java files with the word serializer in them = 96

However, these 96 files include deserializer files so we don't know the correct number of files used for serialization.  The next section takes a look at only deserialization files.

### Deserializer files

> for file in \`find ./fastjson/src/main/ -name '*.java'`;  do echo "$file"; done | grep -i deserializer
> 
![Imgur](https://i.imgur.com/rKDtOnp.png)


To see how many files there are:
> for file in \`find ./fastjson/src/main/ -name '*.java'`;  do echo "$file"; done | grep - i deserializer | wc -l

![Imgur](https://i.imgur.com/Sz5k1fr.png)

Total .java files with the word deserializer in them = 28
>96 - 28 =  68

From this we can deduce that 68 of the .java files are used for serialization.



### Potentially important files
In order to choose which files we should analyse further, we can view how many Lines of Code (LOC) each of the .java files have. To do this we can create a csv file that outputs the file name and how many LOC each of them has.

>for file in \`find ./fastjson -name '*.java'`; do total=$(wc -l < \$file); echo "\$file,$total"; done > /tmp/exceptionalEntities.csv; head -50 /tmp/exceptionalEntities.csv

![Imgur](https://i.imgur.com/FRAMM33.png)

To make it easier to look at the data stored in the csv file, we can open it as an Excel file.

>"C:\Program Files\Microsoft Office\root\Office16\EXCEL.EXE" "/tmp/exceptionalEntities.csv"

- Use the path where Excel is stored followed by the path used to create the csv file.

![Imgur](https://i.imgur.com/hu2sJMy.png)

This opens the csv file using Excel.  After sorting the data so that LOC is in ascending order,  out of the 3756 total files, these are the ones with the most LOC. 
- The number on the left is the file number -1 because of the title row
- The number on the right is the LOC

![Imgur](https://i.imgur.com/7hrC6Hf.png)

A plot chart which shows the names and LOC for the most important files was created.

![Imgur](https://i.imgur.com/St64tVu.png)

### Looking at the code
Now that we know which files have the most LOC we can take a look at the code to help us further understand how the software is structured.

>head -7000 ./fastjson/src/test/java/com/alibaba/json/bvtVO/BigClass.java

![Imgur](https://i.imgur.com/kdDTALb.png)
![Imgur](https://i.imgur.com/qEruINM.png)

Unfortunately the file with the most LOC is just public String declarations which doesn't help clarify anything about the software.

Moving on to the file with the second most LOC.

>head -50 ./fastjson/src/main/java/com/alibaba/fastjson/parser/JSONLexerBase.java

![Imgur](https://i.imgur.com/9vczh4X.png)

This file is an abstract class which focuses on parsing strings.

---
To look at the files with the most LOC in the main section of the software we need to slightly change the command above.

>for file in \`find ./fastjson/src/main -name '*.java'`; do total=$(wc -l < \$file); echo "\$file,$total"; done > /tmp/exceptionalEntities.csv; head -50 /tmp/exceptionalEntities.csv

![Imgur](https://i.imgur.com/WAOHS0z.png)

Again the csv file is opened using Excel.  After sorting the data so that LOC is in ascending order,  out of the 191 total files, these are the ones with the most LOC.

![Imgur](https://i.imgur.com/W1m4bBW.png)

A plot chart was created showing the file names and LOC. As is to be expected this was very clustered but the files we care about, (the ones with the most LOC are readable).

![Imgur](https://i.imgur.com/UTGX0He.png)

Looking at the code for the files with the most LOC:

>head -100 ./fastjson/src/main/java/com/alibaba/fastjson/JSONPath.java

![Imgur](https://i.imgur.com/9YEGzyp.png)
(could need further explanation of code)

---

By altering the initial bash command, we can view which of the files that have "serializer" in their names have the most LOC.

>for file in \`find ./fastjson/src/main -name '*.java'`; do total=$(wc -l < \$file); echo "\$file,$total"; done | grep -i serializer > /tmp/exceptionalEntities.csv; head -50 /tmp/exceptionalEntities.csv
>
![Imgur](https://i.imgur.com/rHkeWjy.png)

Again the csv file is opened using Excel.  After sorting the data so that LOC is in ascending order,  out of the 96 total files, these are the ones with the most LOC.

![Imgur](https://i.imgur.com/Z0vzcKk.png)

This is the plot graph showing the files with serializer in their name with the most LOC

![Imgur](https://i.imgur.com/kcp0l59.png)

Looking at the code for the files with the most LOC:

>head -100 ./fastjson/src/main/java/com/alibaba/fastjson/serializer/SerializeWriter.java

![Imgur](https://i.imgur.com/YCXKUSt.png)

Skipping the license agreement.

![Imgur](https://i.imgur.com/NOKHgp8.png)
(could need further explanation of code)

Looking at the code for the file with the second most LOC.

>head -100 ./fastjson/src/main/java/com/alibaba/fastjson/serializer/ASMSerializerFactory.java

![Imgur](https://i.imgur.com/o07ukXV.png)
(could need further explanation of code)
