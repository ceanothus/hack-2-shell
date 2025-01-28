Question 1. Get the data files

Download the following data files from the internet using the curl command: http://eaton-lab.org/pdsb/test.fastq.gz and http://eaton-lab.org/pdsb/iris-data-dirty.csv. 
Use the less or zless commands to look at each file. Describe what these commands do. Finally, use the head command to print the first 5 lines of the file iris-data-dirty.csv.

**My answer:** The command curl with option "-o" downloads the file from the server and sends it to a file. Zless first unzips a zipped file and then shows that file one page at a time.
Similarly less just shows a file one page at a time. Head with "-n" option outputs to stdout the contents of the first n lines of a file. 

```
curl -o test.fastq.gz http://eaton-lab.org/pdsb/test.fastq.gz 
curl -o iris-data-dirty.csv http://eaton-lab.org/pdsb/iris-data-dirty.csv
zless ./test.fastq.gz
less ./iris-data-dirty.csv
head -n5 iris-data-dirty.csv
```

Question 2. Clean the data

```
# list out the sorted species names, then check for errors. Specifically, use cut with the delimiter set to ",", take out the species column (column 5), then sort and output the unique results.
cut -d "," -f5 ./iris-data-dirty.csv | sort | uniq
# send the whole contents of the file to stdout with cat, then replace the misspelled species names with sed, then use grep -v (invert) to get all lines without "NA" and send to file. 
cat ./iris-data-dirty.csv | sed 's/setsa/setosa/g' | sed 's/versicolour/versicolor/g' | grep -v "NA" > iris-data-clean.csv
# similar as the first subtask, but this time uses the "-c" option for uniq to print out the number of occurrences of the results.
cut -d "," -f5 ./iris-data-clean.csv | sort | uniq -c 

```

Question 3. Find a sequence

Find how many lines in the data file test.fastq.gz start with "TGCAG" and end with "GAG". Describe your work.

**My answer:** I used zgrep to find the lines matching the description. zgrep unzips a file and finds a regular expression, with the "-c" option it outputs the number of results. 

```
zgrep -c "^TGCAG*GAG" ./test.fastq.gz
```

Question 4. Find a sequence chunk

Using grep and other tools if necessary find all lines that contain the sequence "AAAACCCC" and for each print that line, the line above it, and two lines below it (so that a 4-line chunk around each search hit is printed). Describe your work.

**My answer:** I used zgrep again, this time with the -A and -B options. This prints the number of lines After and Before the line containing the regular expression. 

```
zgrep -A2 -B1 "AAAACCCC" ./test.fastq.gz
```

