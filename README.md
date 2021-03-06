# leetcode

## Facebook
### 408. Valid Word Abbreviation
Question
```
A string can be abbreviated by replacing any number of non-adjacent, non-empty substrings with their lengths. The lengths should not have leading zeros.

For example, a string such as "substitution" could be abbreviated as (but not limited to):

"s10n" ("s ubstitutio n")
"sub4u4" ("sub stit u tion")
"12" ("substitution")
"su3i1u2on" ("su bst i t u ti on")
"substitution" (no substrings replaced)
The following are not valid abbreviations:

"s55n" ("s ubsti tutio n", the replaced substrings are adjacent)
"s010n" (has leading zeros)
"s0ubstitution" (replaces an empty substring)
Given a string word and an abbreviation abbr, return whether the string matches the given abbreviation.

A substring is a contiguous non-empty sequence of characters within a string.

 

Example 1:

Input: word = "internationalization", abbr = "i12iz4n"
Output: true
Explanation: The word "internationalization" can be abbreviated as "i12iz4n" ("i nternational iz atio n").
Example 2:

Input: word = "apple", abbr = "a2e"
Output: false
Explanation: The word "apple" cannot be abbreviated as "a2e".
 

Constraints:

1 <= word.length <= 20
word consists of only lowercase English letters.
1 <= abbr.length <= 10
abbr consists of lowercase English letters and digits.
All the integers in abbr will fit in a 32-bit intege
```
Solution
```
class Solution:
    def validWordAbbreviation(self, word: str, abbr: str) -> bool:
        """
        abbr is a string, number is also string
        a->a
        a->"0" false
        abc->3
        a-z->26
        a->b false
        """
        m = len(word)
        n = len(abbr)
        
        i, j = 0, 0
        while i < m and j < n:
            if word[i] == abbr[j]:
                i += 1
                j += 1
            elif abbr[j] == "0":
                return False
            elif abbr[j].isdigit():
                # find all digits
                k = j
                while k < n and abbr[k].isdigit():
                    k += 1
                i += int(abbr[j:k])
                j = k
            else:
                return False
        
        
        return i == m and j == n
```



###  157. Read N Characters Given Read4
```
157. Read N Characters Given Read4
Easy

455

2722

Add to List

Share
Given a file and assume that you can only read the file using a given method read4, implement a method to read n characters.

Method read4:

The API read4 reads four consecutive characters from file, then writes those characters into the buffer array buf4.

The return value is the number of actual characters read.

Note that read4() has its own file pointer, much like FILE *fp in C.

Definition of read4:

    Parameter:  char[] buf4
    Returns:    int

buf4[] is a destination, not a source. The results from read4 will be copied to buf4[].
Below is a high-level example of how read4 works:


File file("abcde"); // File is "abcde", initially file pointer (fp) points to 'a'
char[] buf4 = new char[4]; // Create buffer with enough space to store characters
read4(buf4); // read4 returns 4. Now buf4 = "abcd", fp points to 'e'
read4(buf4); // read4 returns 1. Now buf4 = "e", fp points to end of file
read4(buf4); // read4 returns 0. Now buf4 = "", fp points to end of file
 

Method read:

By using the read4 method, implement the method read that reads n characters from file and store it in the buffer array buf. Consider that you cannot manipulate file directly.

The return value is the number of actual characters read.

Definition of read:

    Parameters:	char[] buf, int n
    Returns:	int

buf[] is a destination, not a source. You will need to write the results to buf[].
Note:

Consider that you cannot manipulate the file directly. The file is only accessible for read4 but not for read.
The read function will only be called once for each test case.
You may assume the destination buffer array, buf, is guaranteed to have enough space for storing n characters.
 

Example 1:

Input: file = "abc", n = 4
Output: 3
Explanation: After calling your read method, buf should contain "abc". We read a total of 3 characters from the file, so return 3.
Note that "abc" is the file's content, not buf. buf is the destination buffer that you will have to write the results to.
Example 2:

Input: file = "abcde", n = 5
Output: 5
Explanation: After calling your read method, buf should contain "abcde". We read a total of 5 characters from the file, so return 5.
Example 3:

Input: file = "abcdABCD1234", n = 12
Output: 12
Explanation: After calling your read method, buf should contain "abcdABCD1234". We read a total of 12 characters from the file, so return 12.
Example 4:

Input: file = "leetcode", n = 5
Output: 5
Explanation: After calling your read method, buf should contain "leetc". We read a total of 5 characters from the file, so return 5.
 

Constraints:

1 <= file.length <= 500
file consist of English letters and digits.
1 <= n <= 1000

```

```
"""
The read4 API is already defined for you.

    @param buf4, a list of characters
    @return an integer
    def read4(buf4):

# Below is an example of how the read4 API can be called.
file = File("abcdefghijk") # File is "abcdefghijk", initially file pointer (fp) points to 'a'
buf4 = [' '] * 4 # Create buffer with enough space to store characters
read4(buf4) # read4 returns 4. Now buf = ['a','b','c','d'], fp points to 'e'
read4(buf4) # read4 returns 4. Now buf = ['e','f','g','h'], fp points to 'i'
read4(buf4) # read4 returns 3. Now buf = ['i','j','k',...], fp points to end of file
"""

class Solution:
    def read(self, buf, n):
        """
        :type buf: Destination buffer (List[str])
        :type n: Number of characters to read (int)
        :rtype: The number of actual characters read (int)
        """
        readChars = 4
        buf4 = [''] * 4 
        buf_idx = 0
        
        while readChars > 0:                  # keep looping as long as there are characters in the buffer
            readChars = read4(buf4)       # read what's in buffer and save number of read chars from buffer
            for i in range(readChars):      # loop over number of chars which were read from buffer in current loop
                if buf_idx < n:                    # if we haven't exceeded number of chars which we have to read - save current char to local buf
                    buf[buf_idx] = buf4[i]   # save current char which was read from buf4 to buf
                    buf_idx += 1                # increment buf_idx which tracks cells in local buf
                
        return buf_idx                          # return last index where we stopped adding chars 
```
### 348. Design Tic-Tac-Toe
https://aaronice.gitbook.io/lintcode/data_structure/design-tic-tac-toe
```
Assume the following rules are for the tic-tac-toe game on an n x n board between two players:

A move is guaranteed to be valid and is placed on an empty block.
Once a winning condition is reached, no more moves are allowed.
A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.
Implement the TicTacToe class:

TicTacToe(int n) Initializes the object the size of the board n.
int move(int row, int col, int player) Indicates that the player with id player plays at the cell (row, col) of the board. The move is guaranteed to be a valid move.
 

Example 1:

Input
["TicTacToe", "move", "move", "move", "move", "move", "move", "move"]
[[3], [0, 0, 1], [0, 2, 2], [2, 2, 1], [1, 1, 2], [2, 0, 1], [1, 0, 2], [2, 1, 1]]
Output
[null, 0, 0, 0, 0, 0, 0, 1]

Explanation
TicTacToe ticTacToe = new TicTacToe(3);
Assume that player 1 is "X" and player 2 is "O" in the board.
ticTacToe.move(0, 0, 1); // return 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

ticTacToe.move(0, 2, 2); // return 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

ticTacToe.move(2, 2, 1); // return 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

ticTacToe.move(1, 1, 2); // return 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

ticTacToe.move(2, 0, 1); // return 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

ticTacToe.move(1, 0, 2); // return 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

ticTacToe.move(2, 1, 1); // return 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
 

Constraints:

2 <= n <= 100
player is 1 or 2.
0 <= row, col < n
(row, col) are unique for each different call to move.
At most n2 calls will be made to move.
```

```

class TicTacToe {
public:
    vector<vector<int>> board;
    int n;

    TicTacToe(int n) {
        board.assign(n, vector<int>(n, 0));
        this->n = n;
    }

    int move(int row, int col, int player) {
        board[row][col] = player;
        if (checkCol(col, player) ||
            checkRow(row, player) ||
            (row == col && checkDiagonal(player)) ||
            (row == n - col - 1 && checkAntiDiagonal(player))) {
            return player;
        }
        // No one wins
        return 0;
    }

    bool checkDiagonal(int player) {
        for (int row = 0; row < n; row++) {
            if (board[row][row] != player) return false;
        }
        return true;
    }

    bool checkAntiDiagonal(int player) {
        for (int row = 0; row < n; row++) {
            if (board[row][n - row - 1] != player) return false;
        }
        return true;
    }

    bool checkCol(int col, int player) {
        for (int row = 0; row < n; row++) {
            if (board[row][col] != player) return false;
        }
        return true;
    }

    bool checkRow(int row, int player) {
        for (int col = 0; col < n; col++) {
            if (board[row][col] != player) return false;
        }
        return true;
    }
};
```

```
class TicTacToe {
public:
    vector<int> rows;
    vector<int> cols;
    int diagonal;
    int antiDiagonal;

    TicTacToe(int n) {
        rows.assign(n, 0);
        cols.assign(n, 0);
        diagonal = 0;
        antiDiagonal = 0;
    }

    int move(int row, int col, int player) {
        int currentPlayer = (player == 1) ? 1 : -1;
        // update currentPlayer in rows and cols arrays
        rows[row] += currentPlayer;
        cols[col] += currentPlayer;
        // update diagonal
        if (row == col) {
            diagonal += currentPlayer;
        }
        // update anti diagonal
        if (col == (cols.size() - row - 1)) {
            antiDiagonal += currentPlayer;
        }
        int n = rows.size();
        // check if the current player wins
        if (abs(rows[row]) == n ||
            abs(cols[col]) == n ||
            abs(diagonal) == n ||
            abs(antiDiagonal) == n) {
            return player;
        }
        // No one wins
        return 0;
    }
};

```

Let, nn be the length of string ss.

Time Complexity: O(1) because for every move, we mark a particular row, column, diagonal, and anti-diagonal in constant time.

Space Complexity: O(n) because we use arrays rows and cols of size n. The variables diagonal and antiDiagonal use constant extra space.

### 1249. Minimum Remove to Make Valid Parentheses
```
Given a string s of '(' , ')' and lowercase English characters.

Your task is to remove the minimum number of parentheses ( '(' or ')', in any positions ) so that the resulting parentheses string is valid and return any valid string.

Formally, a parentheses string is valid if and only if:

It is the empty string, contains only lowercase characters, or
It can be written as AB (A concatenated with B), where A and B are valid strings, or
It can be written as (A), where A is a valid string.
 

Example 1:

Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
Example 2:

Input: s = "a)b(c)d"
Output: "ab(c)d"
Example 3:

Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.
Example 4:

Input: s = "(a(b(c)d)"
Output: "a(b(c)d)"
 

Constraints:

1 <= s.length <= 105
s[i] is either'(' , ')', or lowercase English letter.
```


```
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        # = remove extra "(", ")"
        # ")" is extra, if no "(" exsit
        # "(" is extra, if it reached the end
        # stack to trace index of "("
        
        target = list(s)
        stack = list()
        
        for idx, char in enumerate(target):
            if char == "(":
                stack.append(idx)
            elif char == ")":
                if len(stack):
                    stack.pop()
                else:
                    # ")" is extra, if no "(" exsit
                    target[idx] = ""
                    
        # "(" is extra, if it reached the end
        while stack:
            target[stack.pop()] = ""
            
        return "".join(target)
        
        
        
        
```
