# 面試準備技巧

Tips:

1. Learn patterns - top k means at least consider a heap. "Find the order of these relationships" - consider topological sort.
2. Solve problems on paper. Leetcode is useful but if you can solve on paper you'll be fine in coderpad
3. Talk out loud a lot. Nobody loses points for thinking.
4. As soon as you understand the question, propose new inputs. for example if your question is [remove invalid parens](https://leetcode.com/problems/remove-invalid-parentheses/), suggest one or two inputs and example outputs. This gives you test cases AND shows you're thinking
5. Immediately propose naive solution - eg if you have Range Sum immediately just note "the obvious solution is n^2 runtime without using space, but I think we can do better." Show your work, as professors like to say.
6. Pseudocode a solution ASAP to get interviewer buy in.
7. Once you get to a working solution, go back to your test cases and manually walk through the code. Open a block comment and write line by line the data transformations.
8. The product design interview is HARD. Do a ton of these on paper for anything you can imagine. Online ordering at Walmart, pretend the deli counter at your grocery store needs to scale to your entire state, whatever you see. Just practice.
9. Come up with a script for design interviews. I use the acronym README - requirements, estimations, API, data model, main system, extended system \(extended means load balancing, caching, etc\).
10. Write a spreadsheet for your behavioral interview. You can make one story about three different questions. For example, "I was the lead of this feature" can be about technical challenges, how did you give feedback, how do you prioritize. Just find 4-5 anecdotes that can scale to various possible questions.
11. Buy a whiteboard. Google drawing and bluejeans whiteboard aren't as good.
12. Trello board. Use spaced repetition. I'll add a pic of mine below

Resources:

1. Leetcode premium. Just do it and do a ton of questions and the mock interview tab. Repeating questions is way better than solving 1000 questions and not understanding the patterns. You should be able to see certain words and immediately reach for data structures and algorithms based on the pattern. Just like a musician - skill means focused repetition.
2. P-R-A-M-P: do lots of mock interviews. I did about 10 before my onsite. 7 coding, 3 design.
3. g-r-o-k-k-i-n-g the SD: pay for it, go over it, practice.

## 面試準備

* Do not write your ideas on paper, write on IDE. 
* Do not start coding immediately! Explain your ideas first.  Leetcode Easy 1-4 min / Medium 1-8 min / Hard 1-12
* "Eyeballing" first to check Syntax Error 

### 1. 當面試沒Clue的時候該怎麼辦？

**Ans: Ask Clarifying Questions. Repeat back questions in my own words.  
  
Example of Clarifying Questions**   
\(1\) What are the input constraints?  
\(2\) What are the Data types?  
\(3\) Scale of Input?   
\(4\) Contain duplicates?  
\(5\) Do we allow permutation/combinations? \(ex: \[-3,3,0\], \[-3,0,3\]\)  
\(6\) The sum will be overflow?

### 2. 面試開始時可以問面試官什麼樣的問題？

Ans: Create a Cheat Sheet about Inputs.  

Example of **`String`** Related Questions Cheat Sheet  
\(1\) Does input contain **None/Null**?  
\(2\) Is input **sorted**?   
\(3\) Does input contain **duplicates**? 

### 3. 要如何ask for a hint？

\(X\) Don't Do: please give me a hint \(X\)  
\(O\) Do:  1. my assumptions are X and Y \(O\)  
               2. I'm thinking of doing Z  \(O\)

### 4. Common Permission Questions to Ask

* May I have a few mins to think about the question?
* May I run it on interpreter? 

### 5. Positive Signal？

* Talk about runtime complexity
* Hashmap lookup is not always O\(1\), could be O\(n\)
* String immutability: Python - immutable / Java - Concat vs StringBuilder 

### 6. 如何解題？ 

Find keywords from the question itself. So then we can classify questions. Find more from [Here](https://app.gitbook.com/@iscolectivo/s/algonote/~/drafts/-MDbZHkqxUtUZKZ5OhH1/basics-1/suan-fa-dao-du)

### 7. What to do when getting stuck？

* Always start with Brute Force Then improve with different tools \(ex: Hashtable/Set O\(n\) \)
* Identify bottleneck
* Check time complexity

```text
Exponential  > Polynomial(cubic/quadratic) > linearithmic >  Linear  >  Log  > Constant 
2^n/Recursion               n^3 -> n^2         nlogn           n       logn     1
```



## 準備 System Design

在面試的時候，很有可能會同一個題目往不同的面向發展。

