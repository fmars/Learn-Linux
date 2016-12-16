# ps
- ps gathers information from /proc
- `ps aux` Columns
  - VSZ: virtual memory size is all the memory a process uses
  - RSS: resident set size is non-swapped memory size
  - [command] kernel process
  - -f how connects by ancestory
  
# pstree
# top
- VIRT: all the memory used
- RES: non-swapped memory
- SHR: shared libary which is not neccessary in RES
- `e`: human readable size
- `L': find a process
- `k`: send signal
- `r`: renice
- Display/Order by columns:
  - `f` to show fileds management
  - `d` toggle display
  - `s` sets sort
- `R` sort by ascending/decending order
