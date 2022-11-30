
## Patterns
### Object initialization
Pattern for object initialization in C++
```
call sub_?????
...
call object
```
Pattern for method call : 
```
mov eax, [esi+0] #Use a pointer to our structure
...
mov ecx, esi
call [eax+4] #Based on the created structures, rename appropriately 
```
Pattern for encoding strings
```
#boucle
xor ...
```
## Functions
### new
### malloc
### memset
### memcpy

