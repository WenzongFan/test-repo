# AWK

[Reference link](http://coolshell.cn/articles/9070.html)

## Print columns

	$ awk '{print $1, $4}' netstat.txt

## Format output

	$ awk '{printf "%-8s %-8s %-8s %-18s %-22s %-15s\n",$1,$2,$3,$4,$5,$6}' netstat.txt

## Filter records
	
	# operators: ==, !=, >, <, >=, <=
	$ awk '$3==0 && $6=="LISTEN" ' netstat.txt
	$ awk ' $3>0 {print $0}' netstat.txt

	# builtin var NR: add table title
	$ awk '$3==0 && $6=="LISTEN" || NR==1 ' netstat.txt
	$ awk '$3==0 && $6=="LISTEN" || NR==1 {printf "%-20s %-20s %s\n",$4,$5,$6}' netstat.txt

## Built in vars

	$0	current record (whole line)
	$1 ~ $n columns, separated by FS
	FS	column separator, tab or blank space by default
	NF	column numbers
	NR	line numbers, accumulate for all files
	FNR	line numbers, only for current file
	RS	record separator, line break by default
	OFS	column sepatator, blank space by default
	ORS	record separator, line break by default
	FILENAME	input filename

Examples：

	# output line numbers
	$ awk '$3==0 && $6=="ESTABLISHED" || NR==1 {printf "%02s %s %-20s %-20s %s\n",NR, FNR, $4,$5,$6}' netstat.txt

	# specify separator
	$ awk  'BEGIN{FS=":"} {print $1,$3,$6}' /etc/passwd
	
	or
	$ awk  -F: '{print $1,$3,$6}' /etc/passwd

	# specify multiple separators
	$ awk -F '[;:]'

	# specify output separator '\t'
	$ awk  -F: '{print $1,$3,$6}' OFS="\t" /etc/passwd		
	
## String match

	$ awk '$6 ~ /FIN/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" netstat.txt
	
	$ awk '/LISTEN/' netstat.txt

	$ awk '$6 ~ /FIN|TIME/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" netstat.txt

	$ awk '$6 !~ /WAIT/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" netstat.txt

	or
	$ awk '!/WAIT/' netstat.txt
	
## Split file

	# split file as column 6 and ignore the first line
	$ awk 'NR!=1{print > $6}' netstat.txt

	# print specify columns to file
	$ awk 'NR!=1{print $4,$5 > $6}' netstat.txt

	# if-else-if
	$ awk 'NR!=1{if($6 ~ /TIME|ESTABLISHED/) print > "1.txt";
	else if($6 ~ /LISTEN/) print > "2.txt";
	else print > "3.txt" }' netstat.txt

## Stats

	# stats size of all .c/cpp/h files
	$ ls -l  *.cpp *.c *.h | awk '{sum+=$5} END {print sum}'

	# stats connection numbers with array
	$ awk 'NR!=1{a[$6]++;} END {for (i in a) print i ", " a[i];}' netstat.txt

	# stats memery size used by each user (RSS)
	$ ps aux | awk 'NR!=1{a[$1]+=$6;} END { for(i in a) print i ", " a[i]"KB";}'

## Statements

	BEGIN{statements before awk running}

	END{statements after awk running}

	{statements for processing each line}

Example

```

	# awk script, process score.txt
	$ cat cal.awk

#!/bin/awk -f
#运行前
BEGIN {
    math = 0
    english = 0
    computer = 0
 
    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"
}
#运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
#运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}

	# run it

	$ awk -f cal.awk xxx.txt or ./cal.awk xxx.txt

```

## ENV Vars

```

# with '-v' and ENVIRON(need to be exported first)
$ x=5
 
$ y=10
$ export y
 
$ echo $x $y
5 10
 
$ awk -v val=$x '{print $1, $2, $3, $4+val, $5+ENVIRON["y"]}' OFS="\t" score.txt

```

## Simples

```

#从file文件中找出长度大于80的行
awk 'length>80' file
 
#按连接数查看客户端IP
netstat -ntu | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr
 
#打印99乘法表
seq 9 | sed 'H;g' | awk -v RS='' '{for(i=1;i<=NF;i++)printf("%dx%d=%d%s", i, NR, i*NR, i==NR?"\n":"\t")}'

```

## References

[builtin vars](http://www.gnu.org/software/gawk/manual/gawk.html#Built_002din-Variables)

[process control](http://www.gnu.org/software/gawk/manual/gawk.html#Statements)

[builtin funs](http://www.gnu.org/software/gawk/manual/gawk.html#Built_002din)

[regular expression](http://www.gnu.org/software/gawk/manual/gawk.html#Regexp)
