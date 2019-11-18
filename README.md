## Simple but useful tool for subsetting & rearranging columns by name ##
> Shane Neph

This is often nicer than <code>cut -f1,12,23,78 < input.txt | awk '{ print $2, $1, $3, $4 }'</code> types of approaches, where columns may change positions over time.
It can rearrange any number of columns in a table separated by tabs, and will repeat columns if desired.
Column names are case-sensitive (exact matches to names in this first release).  Selected column names may be separated by commas or spaces.

Simple examples
================
<ul>
<li><code>cutnm Fred,Scooby,Snack < input.tsv</code></li>
<li><code>cutnm Fred,Snack,Scooby,Fred < input.tsv</code></li>
<li><code>cutnm Velma Daphne Snack < input.tsv</code></li>
<li><code>cutnm Mom Dad Sister Aunt myfile.txt</code></li>
</ul>

Notice that you can pass a file name as the last argument or pass things in through stdin.

Suppose you have a file with names such as Indiv-1,...,Indiv-100 but you need them in some other order that matches a different table.
Further, suppose second-table.txt has other columns that are not of interest, such as Color, RobotID, and Age.<p><br /></p>
<pre>
<code>
indivs=$(awk 'NR == 1' second-table.txt | tr '\t' '\n' | awk '$1 ~ /^Indiv-/')
cutnm $indivs first-table.txt > result.txt
</code>
</pre>

result.txt will contain the columns in first-table.txt in the same order they appear in second-table.txt
Some of the extra bits of awk and tr are just to give a little flavor to common problems.
In order, awk grabs the first line of the file (column names), tr changes tabs to newlines, and awk grabs the subset that start
with "Indiv-", which is useful since cutnm currently only works with fixed string exact matches.

In the event that you pass in a non-existent column name, say XYZ, the output will have XYZ_NOTFOUND, followed by NOTFOUND for
all remaining rows.

cutnm is just a bash script wrapping an awk statement.  It might become more sophisticated in time.  I use it when dealing with
large matrices, because I get tired of counting out to column 73.
