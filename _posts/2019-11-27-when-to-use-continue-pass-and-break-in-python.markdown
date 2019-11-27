---
title: When to use continue, pass and break in python
date: 2019-11-27 23:16:00 +05:30
categories:
- blog
tags:
- python
- beginners
- productivity
- codequality
category: blog
---

### **continue**
continue forces the loop to start at the next iteration. In short, continue will jump back to the top of the loop
```
a = [0, 1, 2]
for element in a:
    if not element:
        continue
    print(element)
```
output-
```
1
2
```
**UseCase**: The continue statement can be used to skip the current iteration only. Loop does not terminate but continues on with the next iteration.
### **pass**
while pass means there is no code to execute here, it passes execution to the next statement. So pass will continue processing.
```
a = [0, 1, 2]
for element in a:
     if not element:
         pass
     print(element)
```
output-
```
0
1
2
```
**UseCase**: You can use pass statement when you have created a method but have not implemented, yet.
### **break**
The break statement provides you with the opportunity to exit out of a loop when a given condition is triggered. 
````
a = [0, 1, 2, 3, 4, 5]
for element in a:
    if element == 3:
        break
    print(element)
```
output-
```
0
1
2
```
**UseCase**: The break statement can use in a program to break out of a loop for the matching condition.
