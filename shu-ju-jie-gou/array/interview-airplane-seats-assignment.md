# \[Interview\] Airplane Seats Assignment

Assuming there's an airplane seat assignment \(with N represented as number of rows\) that need to be filled up. You are assigning all 3 ppl groups to the seat assignments. These 3 ppl groups would like to sit together no matter what. The airplane seats are designed as follows:

```python
    A B C  D E F G  H J K
 1  # # #  # # # #  # # #
 2  # # #  # # # #  # # #
 3  # # #  # # # #  # # #
 4  # # #  # # # #  # # #
```

However, some of the seats are taken by other passengers like the following. Which will represent as a piece of String S  \(`S = '1C 2E 2F 3J 4G'`\) Now here comes the question, how many 3 ppl groups can be seated given N and S?  

```python
    A B C  D E F G  H J K
 1  # # O  # # # #  # # #
 2  # # #  # O O #  # # #
 3  # # #  # # # #  # O #
 4  # # #  # # # O  # # #
```

#### Example:

```python
 Given: N = 2, S = '1C 2B 2E 2F'
 
    A B C  D E F G  H J K
 1  # # O  # - 1 -  - 2 -
 2  # O #  # O O #  - 3 -

 Result: 3 groups
```

## Code

```python
class AirplaneSeats:
	'''
	N: Number of rows that are being placed on the plane.
	S: A string which represents a series of taken seats, separated by space
	(i.e. '1A 2C 1F')
	'''
	def assignment(self, N, S):

		col_id = ['A','B','C','D','E','F','G','H','J','K']
		col_dict = dict()

		for i in col_id:
			col_dict[i] = col_id.index(i)

		cols = len(col_id) + 2
		rows = N
		occupied_seats = S.split(' ')
		# print(occupied_seats)
		occupied = [[False]*cols for _ in range(rows)]

		# aisle are occupied
		for i in range(rows):
			for j in range(cols):
				if j == 3 or j == 8:
					# print(j)
					# print(occupied[i][j])
					occupied[i][j] = True
		
		# mark taken seats
		if len(S) != 0:	
			for seat in occupied_seats:
			  #      1A    ->   1              A      
				occupied[int(seat[0])-1][col_dict[seat[1]]] = True
			print(occupied)

		count = 0
		# go thru every row
		for row in range(rows):
			for col in range(cols-2):
				if occupied[row][col] == False and occupied[row][col+1] == False and occupied[row][col+2] == False:
					occupied[row][col] = True
					occupied[row][col+1] = True
					occupied[row][col+2] = True
					count += 1
		# print(occupied)
		return count


# Driver Code 
if __name__ == "__main__":
	print(AirplaneSeats().assignment(2, "1A 2B"))
	print(AirplaneSeats().assignment(2, "1A 2F 1C"))
	print(AirplaneSeats().assignment(1, ""))
```

