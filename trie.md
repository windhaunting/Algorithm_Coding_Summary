
# Trie

Trie is a digital tree or prefix tree, is a type of k-ary search tree, a tree data structure used for locating specific keys from within a set. It is an efficient information retrieval data structure, and is fast for lookup of words in a dictionary;  
Compared to the normal hashmap,  its average time o(l) words length, and space is smaller than regular hashmap.



## Implementation of Trie: 

```

# How to store the children node;  use list of list; or dictionary of dictionaries to represent trie
# or use a dictionary to present trie children (edge connection)

from collections import defaultdict

class TrieNode(object):
    def __init__ (self, endOfWord = False):
        #self.char = char               # store the charcter when endofWord is
        self.endofWord = endOfWord
        self.children = defaultdict(TrieNode)           #key is character, value is another TrieNode it points to
        self.prefixCount = 0          #how many prefix in the trie words


class Trie():
    def __init__(self):
        self.root = TrieNode()
        
    
    def insert(self, word):
        '''
        insert word into trie
        '''
        current = self.root
        
        for c in word:         # check every character
            if c not in current.children:
                current.children[c] = TrieNode(False)      #add 
                
            current = current.children[c]
            current.prefixCount += 1
        current.endofWord = True
        

    def search(self, word):                # search whole word, not prefix
        current  = self.root
        for c in word:
            if c not in current.children:
                return False
            current = current.children[c]
        return current.endofWord
    
    def remove(self, word):
        '''
        not real delete, make endofword flag as false
        '''
        current = self.root
        for c in word:
            if c not in current.children:
                return False
            #if current.endofWord:
            #    current.endofWord = False
            current = current.children[c]
        current.endofWord = False
        return True
     
    def countPrefix(self, word):
        #how many words have this input words as prefix
        current = self.root
        for c in word:
            if c not in current.children:
                return 0
            current = current.children[c]
        return current.prefixCount
    
    def searchPrefixWord(self, root, prefix ):
        '''
        auto fill the rest of words
        '''
        out = []
        if root.endofWord:
            out.append( prefix )
		
        for ch in root.children:
            if isinstance( prefix, tuple ):
                self.searchPrefixWord( root.children[ch], prefix + (ch,) )
            elif isinstance( prefix, list ):
                tmp = self.searchPrefixWord( root.children[ch], prefix + [ch] )
            else:
                tmp = self.searchPrefixWord( root.children[ch], prefix + ch )
            out.extend(tmp)
        return out
    
    def enumerateAllWordWithPrefix(self, prefixs):
        #search all words with common prefix
        current = self.root
        for c in prefixs:
            if c not in current.children:
                return []
            current = current.children[c]
        return self.searchPrefixWord(current, prefixs)

TrieObj = Trie()

TrieObj.insert('abcd')
TrieObj.insert('abcef')
TrieObj.insert('adef')

print (TrieObj.search('abcd'))
print (TrieObj.search('abc'))
print (TrieObj.search('abcdef'))
print (TrieObj.search('abcef'))
print (TrieObj.search('adef'))

TrieObj.remove('abcd')
print (TrieObj.search('abcd'))

TrieObj.insert('abcd')
print (TrieObj.search('abcd'))

TrieObj.insert('abgef')
print (TrieObj.enumerateAllWordWithPrefix("ab"))

```

## Another implementation: use array to store

```
class TreeNode:
    def __init__(self):
        self.children = [None]*26       # 26 letters here
        
        self.endFlag  = False        # end of world flag
        
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.nd = TreeNode()
    
        
    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        #word = lower(word)    # default lowercase
        p = self.nd
        for i, w in enumerate(word):
            #print ("ord(w)-97: ", ord(w)-97)
            if p.children[ord(w)-ord('a')] is None:
                p.children[ord(w)-ord('a')] = TreeNode()
            
            p = p.children[ord(w)-ord('a')]    
        
        p.endFlag = True

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        p = self.nd
        for i, w in enumerate(word):
            if p.children[ord(w)-ord('a')] is None:
                return False
            
            p = p.children[ord(w)-ord('a')]     
    
        if p is not None and p.endFlag == True:
            return True
        return False

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        p = self.nd
        for i, w in enumerate(prefix):
            if p.children[ord(w)-ord('a')] is None:
                return False     
            p = p.children[ord(w)-ord('a')]     

        if p is not None:
            return True
        return False


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)

```

### Example Trie: 1803. Count Pairs With XOR in a Range
Given a (0-indexed) integer array nums and two integers low and high, return the number of nice pairs. A nice pair is a pair (i, j) where 0 <= i < j < nums.length and low <= (nums[i] XOR nums[j]) <= high. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/count-pairs-with-xor-in-a-range/) 


```

from collections import defaultdict

class TrieNode:
     def __init__(self):
        self.children = defaultdict(TrieNode)
        self.cnt = 0
        
class Trie:
    def __init__(self):
        self.root = TrieNode()  # defaultdict(Trie)   # {}
    def insert(self, val):
        # insert into trie
        node = self.root
        HIGHEST = 15
        for i in range(HIGHEST-1, -1, -1):  # find out all the bit position; e.g 6=> 110  
            bit = (val >> i) & 1
            node.children[bit].cnt += 1         # bit is 1; cnt +=1;  
            node = node.children[bit]
        print("node" , self.root)
        
    def count(self, val, limit):
        # the number of element xor val < limit
        
        HIGHEST = 15
        ans = 0
        node = self.root
        for i in range(HIGHEST-1, -1, -1):
            if not node:
                break
            bit = (val >> i) & 1
            lmt_bit = (limit >> i) & 1
            if lmt_bit:         # 1    ;  1 ^ 1 => 0
                #if bit in node.children[bit]
                ans += node.children[bit].cnt
                node = node.children[1^bit] # node.get(1^bit, {}) #  1 ^ 1 => 0
            else:      # 0 ;   0; not need to be accumulated to ans
                node = node.children[0^bit]  # node.get(0^bit, {})  
                
        return ans
        
class Solution:
    def countPairs(self, nums: List[int], low: int, high: int) -> int:
        
        """
        # 1st naive idea
        n = len(nums)
        ans = 0
        for i in range(n):
            for j in range(i+1, n):
                if low <= nums[i]^nums[j] <= high:
                    ans += 1
        return ans
        """   
        
        # 2nd optimize use trie, XOR problem, count;
        # find out how many number of values XOR num that is samller than limit, that define a function get_counts(num, limit)
        # then we have for each val in nums array,  get_counts(num, high+1) - get_counts(num, low)
        
        root = Trie()
        ans = 0
        for val in nums:
            ans += root.count(val, high + 1) - root.count(val, low)
            root.insert(val)
            
        return ans

```