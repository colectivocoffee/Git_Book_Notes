# String

## String小技巧

在for loop的過程中，我們有可能會碰到需要換char的情況。然而，String is immutable，我們必須要重新組裝一個 `new_string`，來達到目的。

### 1. 在中間換char

`s[:i] + char + s[i+1:]`

### 2. 在最後換char

`s[:-1] = char`



