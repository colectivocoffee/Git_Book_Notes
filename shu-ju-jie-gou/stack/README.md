# Stack

#### 何時要使用Stack?

在討論之前，我們可以先看Recursion這個方法。Recursion是自己call自己，來達到當我們不知道有多少層for loop時的一個手段。然而自己call自己，會造成Recursion遞歸深度過深的情況；因此，如果不能用自己call自己的辦法時，另外一種辦法就是用While loop + Stack來實現Recursion。  
  
由此可知，當我們要“記得”之前看過的elements，又不知道還剩下多少個時，就可以用while loop + stack來解決。  
  


