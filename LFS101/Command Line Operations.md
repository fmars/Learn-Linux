## Section Basic operations
1. Login and SSH
2. `shutdown`
3. `which & whereis`
4. `cd -`
5. `tree -d -L`
6. difference between hard link and soft link (needs to understand inode before answering this question)

## Searching for a file
1. Standard file stream, 0: stdin, 1: stdout, 2: stderr
2. I/O redirect

   ```
   some_cmd 2>&1
   some_cmd > file 2>&1
   come_cmd >& file
   ```
   
3. Pipe. Difference between Pipe and I/O redirect. 
   1. Redirect passes output to a file/stream
   2. Pipe passes output to a program
   3. `p1 > tmp && p2 < tmp` is like `p1 | p2`. But pipe is more efficient. It doesn't need to create temporary file and can execute p2 without waiting p1 finished.

4. find 
   - find path [-type df] [-name] [-ctime [+|-]n[smhd]] [-size] [-exec ';'] [-ok ';']  
   - wildcard matching
   
5. locate and updatedb

