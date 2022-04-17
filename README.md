# printing_tables

Printing tables is a curiosity in Python. You see it pretty regularly when you do ```conda update anaconda```, but not much outside of that. 
Here is how you do it Python 3+! The big idea is to use an f-string. 

I will assume we have a dictionary to print. For simplicity, I will know the structure of the dictionary, but if you didn't, write a function 
to loop over the elements or other variables you have and get the lengths of each element for printing. 

```
# Set up a dictionary to pull data from when we print.
d = {0:['This','example'],
     1:['shows','how'],
     2:['to','print'],
     3:['a','table']}
```

Notice that each key contains a list with two elements. The maximum word length in each position of the lists across all of the lists is [5,7]. The
length of each key value is 1.

The second thing you need to determine is how much space you want between each column and if you want a vertical separator in each column. I 
personally like 5 spaces between each column no vertical separator. Finally, you need to determine the actual column headers so you can set up the 
full set of length separators. For simplicity, I will make the column headers 'Key', 'Column 1', and 'Column 2'. The lengths of these headers are 
3, 8, and 8, respectively. Knowing these things, we can think about the final structure of the table.  

At a high level, the basic idea is to print a header, print a horizontal dividing line, and then print each row in succesion by setting up the 
column features. To do this accurately, we need to think of the table in terms of characters in each line of the print. Consider the layout below that
documents the length of each row. The + signs document blank space in the final table. 

```
Key+++++Column 1+++++Column 2
-----------------------------
0+++++++This+++++++++example+
1+++++++shows++++++++how+++++
2+++++++to+++++++++++print+++
3+++++++a++++++++++++table+++
```

In my experience, it is easiest to think about the header row in chunks; the number of chunks is equal to the number of columns, and length of each
chunk is the length of the header plus the space between columns. So in our example, you get three chunks with lengths as shown:

```
Key+++++
len(Key+++++) = 8

Column 1+++++
len(Column 1+++++) = 13

Column 2
len(Column 2) = 8
```

You use this information to format the f-string for the header. When you format the f-string, you will see the three structures that look like 
```{str_var:<#}```. This structure tells the f-string to left justify (```<```) a block of characters of length ```#``` to display ```str_var```, 
which could be a variable or an actual string; in this situation, if ```len(str_var) < #```, then blank space equal to ```# - len(str_var)``` will 
be inserted to give a total length of ```#```. The line of dashes below the header should be the total length of all columns.

```
print(f'{"Key":<8}{"Column 1":<13}{"Column 2":<8}')
print('-' * (8 + 13 + 8))
```

Finally, the trick is to loop over all entries in the dictionary and build rows that look like the header row. The full code is below.

```
d = {0:['This','example'],
     1:['shows','how'],
     2:['to','print'],
     3:['a','table']}

print(f'{"Key":8}{"Column 1":13}{"Column 2":8}')
print('-' * (8 + 13 + 8))
for k, v in d.items():
    print(f'{k:<8}{v[0]:<13}{v[1]:<8}')
```
