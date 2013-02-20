# sed - stream editor

[sed tutorial](http://coolshell.cn/articles/9104.html)

[sed manual](http://www.gnu.org/software/sed/manual/sed.html)

## Replace with 's'

	# '/g': replace all matched strings in one line
	$ sed "s/from/to/g" filename

	# '-i': modify file directly
	$ sed -i "s/from/to/g" filename

	# Add '#' in the front of each line
	$ sed 's/^/#/g' filename

	# Add '--' to the end of each line
	$ sed 's/$/ -- /g' filename

### regular expression

	^:	line head
	$:	line end
	\<:	word head
	\>:	word end
	.:	single letter
	*:	numbers: 0 ~ n
	[]:	chars collection: [abc], [a-zA-Z], [^a] (not a)

### replace html tags

Example string:

	$ cat html.txt
	<b>This</b> is what <span style="text-decoration: underline;">I</span> meant. Understand?

This not work:

	$ sed 's/<.*>//g' html.txt
	 meant. Understand?

This work well:

	$ sed 's/<[^>]*>//g' html.txt
	This is what I meant. Understand?

### specify replace range

	# '3s': only do replace in third line
	$ sed "3s/from/to/g" filename

	# '3,6s': do replace between third to sixth lines
	$ sed "3,6s/from/to/g" filename

	# only replace first 'from' in each lines
	$ sed 's/from/to/1' filename

	# only replace second 'from' in each lines
	$ sed 's/from/to/2' filename

	# only replace 'from' from third and after
	$ sed 's/from/to/3g' filename

## Multi-match

	# 1 ~ 3 lines: my -> your, 3 ~ end: This -> That
	$ sed '1,3s/my/your/g; 3,$s/This/That/g' filename

Equals:

	$ sed -e '1,3s/my/your/g' -e '3,$s/This/That/g' filename

	# '&' is what matched, add [] to 'my'
	$ sed 's/my/[&]/g' filename

## curves match

	# quote matched vars with \1, \2
	$ sed 's/This is my \([^,]*\),.*is \(.*\)/\1:\2/g' filename

## sed commands

### N:	expand match range to current line and next line

	# do match in two lines each time
	$ sed 'N;s/my/your/' filename

	# Join two lines with ','
	$ sed 'N;s/\n/,/' pets.txt

### a:	append

### i:	insert

	# '1i': insert line before line 1
	$ sed "1 i insert this line before line 1" filename

	# '1a': append line after line 1
	$ sed "1 a append this line after line 1" filename

	# '/fish/a': append line if matched 'fish'
	$ sed "/fish/a append this line if matched fish" filename

### c:	replaced matched lines

	# '2c': replace line 2
	$ sed "2 c replace line 2 with this line" filename

	# '/fish/c': replace line witch matched 'fish'
	$ sed "/fish/c replace line witch matched fish" filename

### d:	delete matched lines

	$ sed '/fish/d' filename
	$ sed '2d' filename
	$ sed '2,$d' filename

### p:	print

	# print all lines and lines matched 'fish'
	$ sed '/fish/p' filename

	# '-n': don't print all lines, only print matched lines
	$ sed -n '/fish/p' filename

	# print lines from one pattern to another
	$ sed -n '/dog/,/fish/p' filename

	# print from line 1 to matched lines
	$ sed -n '1,/fish/p' filename

## Some hints

### Pattern Space

Something sed will run to, pseudo-code here:

```
foreach line in file {
    //放入把行Pattern_Space
    Pattern_Space <= line;
 
    // 对每个pattern space执行sed命令
    Pattern_Space <= EXEC(sed_cmd, Pattern_Space);
 
    // 如果没有指定 -n 则输出处理后的Pattern_Space
    if (sed option hasn't "-n")  {
       print Pattern_Space
    }
}
```

### Address

Specify range of sed run to, could be a number or pattern

	# comma separate address, ! - don't run cmd if matched
	[address[,address]][!]{cmd}

Address could be relative location:

	# '+3' means 3 lines after 'dog'
	$ sed '/dog/,+3s/^/# /g' filename

### Multi-commands

Could separate with semicolon ';', or quote with '{}' as nested cmd

	# run /This/d to 3 ~ 6 lines
	$ sed '3,6 {/This/d}' pets.txt

	# 3 ~ 6: match 'This' and then match 'fish', delete it if matched
	$ sed '3,6 {/This/{/fish/d}}' pets.txt
	
	# delete lines matched 'This' and remove ' '
	$ sed '1,${/This/d;s/^ *//g}' pets.txt

### Hold Space

Another space after pattern space, some cmds:

	g:	copy hold space to pattern space
	G:	append hold space to pattern space
	h:	copy pattern space to hold space
	H:	append pattern space to hold space
	x:	exchange hold space and pattern space

	+-------------+----------+
	|pattern space|hold space|
	+-------------+----------+

Examples:

	$ cat t.txt
	one
	two
	three

	$ sed 'H;g' t.txt
	one
	 
	one
	two
	 
	one
	two
	three

	# revert file
	# 1!G: don't run G to line 1, append hspace to pspace
	# h: copy pspace to hspace
	# $!d: don't delete last line, delete other lines
	$ sed '1!G;h;$!d' t.txt
	three
	two
	one
