# assignment-1
## I. Get the data files 
**Answer:**  
$ curl http://eaton-lab.org/pdsb/test.fastq.gz -o test.fastq.gz    
$ head -5 test.fastq.gz 
```<!doctype html>
<!--[if lt IE 7]><html class="no-js lt-ie9 lt-ie8 lt-ie7" lang="en"> <![endif]-->
<!--[if (IE 7)&!(IEMobile)]><html class="no-js lt-ie9 lt-ie8" lang="en"><![endif]-->
<!--[if (IE 8)&!(IEMobile)]><html class="no-js lt-ie9" lang="en"><![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js" lang="en"><!--<![endif]-->
```  
$ curl http://eaton-lab.org/pdsb/iris-data-dirty.csv -o iris-data-dirty.csv    
$ head -5 iris-data-dirty.csv  
```5.1,3.5,1.4,0.2,Iris-setosa
4.9,3.0,1.4,0.2,Iris-setosa
4.7,3.2,1.3,0.2,Iris-setosa
4.6,3.1,1.5,0.2,Iris-setosa
5.0,3.6,1.4,0.2,Iris-setosa
```
Sources-  
[The head command](http://www.linfo.org/head.html)

## II. Clean the data 
**Answer:**   
$ curl http://eaton-lab.org/pdsb/iris-data-dirty.csv > iris.txt  
**Lines with NA** 
```$ grep -n NA iris.txt
73:6.3,NA,4.9,1.5,Iris-versicolor
89:5.6,NA,4.1,1.3,Iris-versicolor
```  
**What are the misspelled words**  
$ Cut -s -d ',' -f 5 iris-data-dirty.csv | sort | uniq -c  
     49 Iris-setosa  
      **1 Iris-setsa**   
     49 Iris-versicolor    
     **1 Iris-versicolour**  
     50 Iris-virginica    

**Lines with misspelled words** 
```$ grep -n setsa iris.txt
12:4.8,3.4,1.6,0.2,Iris-setsa
```
```$ grep -n versicolour iris.txt
51:7.0,3.2,4.7,1.4,Iris-versicolour
```
**Creating new file**  
$ curl http://eaton-lab.org/pdsb/iris-data-dirty.csv > iris-data-clean.csv   
**Changing misspelled words**   
$ perl  -i -pe 's/setsa/setosa/g' iris-data-clean.csv     
$ perl -i -pe 's/colour/color/g' iris-data-clean.csv  
**Removing lines with NA**   
$ sed -i '/NA/d' iris-data-clean.csv   
**Number of data values**   
* Iris-setosa-  
$ grep -w "Iris-setosa" -c iris-data-clean.csv  
50  
* Iris-versicolor  
$ grep -w "Iris-versicolor" -c iris-data-clean.csv  
48
* Iris-virginica  
$ grep -w "Iris-virginica" -c iris-data-clean.csv  
50

Sources-  
[Removing lines 1](https://stackoverflow.com/questions/5410757/delete-lines-in-a-text-file-that-contain-a-specific-string)  
[Removing lines 2](https://stackoverflow.com/questions/11641780/bash-delete-a-line-from-a-file-permanently)  
[Editing with perl command](https://askubuntu.com/questions/434051/how-to-replace-a-string-on-the-5th-line-of-multiple-text-files)  
[Counting occurances with grep](https://stackoverflow.com/questions/3137094/how-to-count-lines-in-a-document)  

## III. Summarize sequence data file  
**Answer**  
$ zcat test.fastq.gz | grep -c '^TGCAG.*GAG$'  
44  

Source:   
Gitter chatroom-Vjay Ramesh 

## IV. Summarize sequence data file  
**Answer**  
$ mkdir sorted_reads/    
$ zcat test.fastq.gz | grep '^@[0-9]' | cut -d '.' -f 1 | sort | uniq | cut -d '_' -f 2 | sort | uniq    
cyathophylla  
cyathophylloides  
przewalskii  
rex  
superba  
thamno 

**Variable**  
$ name=$(zcat test.fastq.gz | grep '^@[0-9]'| cut -d '.' -f 1 | sort -nr | uniq -c)   
**For-loop**   
$ for name in $name ; do  zcat test.fastq.gz | grep @$name -A 4 > sorted_reads/$name.fastq; done

Sources-  
Gitter chat   
Class 









