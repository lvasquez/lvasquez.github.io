---
layout: post
title: Find and copy files from csv file
comments: true
description: Python script that Find and Copy File reading a CSV File
published: true
categories:
- python
tags:
- csv
- python
- copy and paste
- read csv
- script
---

Long live the blog, this is a little script with python that basically read a .csv file, get a specific row with the name of the file that I needed to find and the copy the file and paste on a diferente folder. Since the origin path start with the first segments of the text row from the .csv file, I modify the script in order to build the path.

So this is the structure of my .CSV file, the row 7 is where the name of the file that I need to find

<div class="row previews" align="center">
		<img class="img-responsive" alt="DataTable to Pivot DataTable" src="/images/csv_file.png">
</div>

This is the path where I need to find the file

<div class="row previews" align="center">
		<img class="img-responsive" alt="DataTable to Pivot DataTable" src="/images/path_files.png">
</div>

source code:

{% highlight python %} 
import glob, os, csv, shutil

with open('file.csv', 'r') as csvFile:
    reader = csv.reader(csvFile)
    for row in reader:
        date_file = row[7].split("_", 2)
        new_date = date_file[0].split("-", 2)
        hour_array = date_file[1].split("-", 1)
        year = new_date[0]
        month = new_date[1]
        day = new_date[2]
        hour = hour_array[0]
        path_folder = '../GIR/' + year + '/' + month + '/' + day + '/' + hour
        for file in os.listdir(path_folder):
            if file.endswith(".mp3"):
                if file == row[7]:
                    shutil.copy(path_folder + '/' + file, 'new_path_folder/')
                    print("File Found", file)
                             
csvFile.close()
{% endhighlight %}

\o/

So far so good, right!