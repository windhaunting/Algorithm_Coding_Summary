
# Trie

Trie is a digital tree or prefix tree, is a type of k-ary search tree, a tree data structure used for locating specific keys from within a set. It is an efficient information retrieval data structure, and is fast for lookup of words in a dictionary;  
Compared to the normal hashmap,  its average time o(l) words length, and space is smaller than regular hashmap.


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