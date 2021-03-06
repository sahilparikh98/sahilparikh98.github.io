---
layout: post
title: Python does it all in one line
subtitle: This is for you, Ankit
tags: [personal]
comments: true
---

Recently, a friend asked me for help on a Python program where everything needs to be done in one line. Sometimes, this really isn't possible. However, Python is massively powerful in one line, thanks to it basically being a form of pseudocode. Let's take a look at how this works.  

In this problem, we are given a dictionary object that contains strings mapped to another dictionary (nested dictionaries):  
```python
gradebooks = {'business analytics': {'Alice': 95, 'Troy': 92}, 
'Python Programming': {'James': 89, 'Charles': 100, 'Bryn': 69, 'Alice': 100}, 
'R programming': {'Troy': 93, 'James':100, 'Charles':88}}
```
The first issue we have is to give the max score across all the classes. We can use Python's `max()` function for this. However, we're probably going to have to do some fancier stuff on the inside, because the `max()` function needs to take in an iterable that can be compared. Your first idea may be to just do:
```python
max(gradebooks.values()) # this is wrong
```
However, `gradebooks.values()` returns the values, which in this case would be the dictionaries that represent each student and their score for the class. So `gradebooks.values()` would return an array of these dictionaries: 
```python
[{'Alice': 95, 'Troy': 92}, 
{'James': 89, 'Charles': 100, 'Bryn': 69, 'Alice': 100}, 
{'Troy': 93, 'James':100, 'Charles':88}]
```
This can't be compared in the `max()` function. What it needs is items that are comparable. How about numbers? Those seem pretty comparable to me. How do I turn all of that into a set of numbers? And in one line? Are you overwhelmed? It's not a huge issue, because Python has really powerful feature called **list comprehension**.  

We're going to use Python's list comprehension, which allows us to put values into a list based on some other iterable. The list building concept says that this:
```python
l = ['1', '2']
scores = []
for num in l:
  scores.append(l)
```
is equivalent to:
```python
l = ['1', '2']
scores = [num for num in l]
```
It makes sense if you read it backwards. Let's look at the for loop. This is going to go through every value in `l` and call it num. Then, num is appended. So basically, the statement is `[append num for num in list]`. Make sense? You take what you're appending and put it in the front, and then put the for loop afterwards. This is essentially how the list building syntax works. We'll see how similar it is for dicts in a bit.  

Now let's translate this for the first problem, which is getting the max of all the scores. What we want to do, is make a list of the max scores for each course, and then pass that into the `max()` function, and it'll get the max out of the list. How do I build that list? First, let's do it the way we know:
```python
scores = []
for course in gradebooks:
  values = gradebooks[course].values() # gets a list of all the scores for this course
  scores.append(max(values)) # appends the max from that list
```
Great! Now how do we do this in one line? Let's start by reducing the number of lines we already have by not storing the list of all the scores and just appending the value:
```python
scores = []
for course in gradebooks:
  scores.append(max(gradebooks[course].values())) # appends the max from that list
```
Now, this looks very similar to the simple version of this that we did! We want to take what we're appending and put it in the front, and then put the for loop afterwards. We're appending `max(gradebooks[course].values())`, and the for loop is `for course in gradebooks`. It's just that simple:
```python
scores = [max(gradebooks[course].values()) for course in gradebooks]
```
Now, we just pass it into the `max()` function.
```python
max_overall_score = max([max(gradebooks[course].values()) for course in gradebooks])
```
Now we move onto the more involved stuff, which is **dictionary comprehension**. We're going to use the same concept for list comprehension and use it to build a dictionary. It's not much rougher. The way we normally populate a dictionary from another dictionary is this:
```python
old_d = {'one': 1,'two': 2}
new_d = {}
for key in old_d:
  new_d[key] = old_d[key]
```
But it's equivalent to say:
```python
old_d = {'one': 1,'two': 2}
new_d = {key: old_d[key] for key in old_d}
```
See how similar this is to the list comprehension? Instead of taking what we're appending and putting it first, we supply the mapping first (`key: value`). So since we were putting `key` into `new_d` with the value from `old_d`, it shows that in this case, the format is `{key that goes in new dict: value from something for key in old dict}`. We'll use this in a very standard way for question 2.  

Question 2 says to build a dictionary in one line that contains the max scores for each class. In a way, we've already done a lot of the groundwork in Q1! In Q1, we build a list of all the max scores, and get the max out of that. So we know how to get the max score for a specific class, which is `max(gradebooks[course].values())` Now, instead of building a list, we'll build a dictionary, and map the course to that value. Let's do it the normal way first:
```python
max_score_dict = {}
for course in gradebooks:
  max_score_dict[course] = max(gradebooks[course].values())
```
Now, we can turn it into a one-liner! The mapping needs the key that goes into the new dictionary, which is `course` and the value that goes in, which is `max(gradebooks[course].values())`, so the mapping looks like `course: max(gradebooks[course].values())`, which is what we're putting into the dictionary. So the key-value pair goes first, and then the for loop:
```python
max_score_dict = {course: max(gradebooks[course].values()) for course in gradebooks}
```
We finished Q2! Now onto the hardest part, which combines all of these ideas and also adds a new twist.  
Q3 asks us to make a dictionary with the keys being the classes, and the values being a list of the students who got the max score. So given:
```python
gradebooks = {'business analytics': {'Alice': 95, 'Troy': 92}, 
'Python Programming': {'James': 89, 'Charles': 100, 'Bryn': 69, 'Alice': 100}, 
'R programming': {'Troy': 93, 'James':100, 'Charles':88}}
```
We want:
```python
{'business analytics': ['Alice'], 
'Python programming': ['Charles', 'Alice'], 
'R programming': ['James']}
```
We're gonna combine the list comprehension and dictionary comprehension into one long line here. So let's break down what we need to do. First, we need to get a list of the people who got the max score for a specific course. We know we can get the max score for a course with `max(gradebooks[course].values())`. We'll need to combine that with yet another dictionary comprehension. Let's write out the simple way to get a list of people who got the max score for a specific course:
```python
max_students = []
course = some course
for student in gradebooks[course]:
  student_score = gradebooks[course][student] # remember that gradebooks[course] returns a dictionary of students and their score, so gradebooks[course][student] returns the score for that student
  if student_score == max(gradebooks[course].values()):
    max_students.append(student)
```
Now, we can start collapsing. Python's list comprehension actually has a really interesting feature. It allows you to actually add things based on a condition. We put the condition after the for loop. So we can do this now:
```python
l = [1, 2, 3]
scores = []
for num in l:
  if num > 1:
    scores.append(l)
```
This can be written in one line like this:
```python
l = [1, 2, 3]
scores = [num for num in l if num > 1]
```
So now we know, that if we want to add something to a list based on a condition, we put what we're appending first, then the for loop, and then the condition. So in the above case, we're appending `num`, the for loop loops through `l`, and the condition is `if num > 2`. So it reads `[append num for num in l if condition is true]`. Let's apply this to our problem! Let's start by taking out the variable that stores the student score:
```python
max_students = []
course = some course
for student in gradebooks[course]:
  if gradebooks[course][student] == max(gradebooks[course].values()):
    max_students.append(student)
```
We're appending `student`, the for loop is `for student in gradebooks[course]`, and the condition is `if gradebooks[course][student] == max(gradebooks[course].values())`. So we can now build this all in one line:
```python
course = some course
max_students = [student for student in gradebooks[course] if gradebooks[course][student] == max(gradebooks[course].values())]
```
Great! This gives us a list of students who got the max score in a specific course. Now, we need to put that in a dictionary for a specific course. We can do this the simple way, and we'll use what we just did with finding the max students as well:
```python
max_score_students = {}
for course in gradebooks:
  max_students_course = [student for student in gradebooks[course] if gradebooks[course][student] == max(gradebooks[course].values())] # gets a list of students who got the max score
  max_score_students[course] = max_students_course
```
It should be pretty easy from here! First, let's take out the `max_students_course` variable by just setting the value rather than storing it:
```python
max_score_students = {}
for course in gradebooks:
  max_score_students[course] = [student for student in gradebooks[course] if gradebooks[course][student] == max(gradebooks[course].values())] 
```
This is a problem we've already solved! The `key: value` pair is `course: [student for student in gradebooks[course] if gradebooks[course][student] == max(gradebooks[course].values())]`, the for loop is `for course in gradebooks`. And now we can combine the `key: value` pair and the for loop:
```python
max_score_students = {course: [student for student in gradebooks[course] if gradebooks[course][student] == max(gradebooks[course].values())] for course in gradebooks}
```
I know this all doesn't fit in one line graphically, but it's technically one line. This really shows you the power of Python list comprehension! 
   
