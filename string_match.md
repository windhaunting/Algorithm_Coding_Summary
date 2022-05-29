
# String Matching

String Matching is try to match a string patten with a another string text. It involves substring match and searching.


## Common string match algorithms

 Rabin–Karp and KMP algorithms

 Rabin–Karp is a rolling hash algorithm that matches the hash value of the pattern with the hash value of current substring of text,
  and if the hash values match then only it starts matching individual characters.' It is a hash function where the input is hashed in a window that moves through the input.

 KMP (Knuth–Morris–Pratt) string-searching algorithm is a strign match algorithm that employs the observation that when a mismatch occurs, the word itself embodies sufficient information to determine where the next match could begin, thus bypassing re-examination of previously matched characters. The main core of this algirthm is the array of Longest Proper Prefix which is Suffix (LPS), which is constructed for the pattern prior to the main phase of the algorithm


#### Implementation of Rabin–Karp

```
Following program is the python implementation of
# Rabin Karp Algorithm given in CLRS book
 
# d is the number of characters in the input alphabet
d = 256
 
# pat  -> pattern
# txt  -> text
# q    -> A prime number
 
def search(pat, txt, q):
    M = len(pat)
    N = len(txt)
    i = 0
    j = 0
    p = 0    # hash value for pattern
    t = 0    # hash value for txt
    h = 1
 
    # The value of h would be "pow(d, M-1)%q"
    for i in range(M-1):
        h = (h*d)%q
 
    # Calculate the hash value of pattern and first window
    # of text
    for i in range(M):
        p = (d*p + ord(pat[i]))%q
        t = (d*t + ord(txt[i]))%q
 
    # Slide the pattern over text one by one
    for i in range(N-M+1):
        # Check the hash values of current window of text and
        # pattern if the hash values match then only check
        # for characters one by one
        if p==t:
            # Check for characters one by one
            for j in range(M):
                if txt[i+j] != pat[j]:
                    break
                else: j+=1
 
            # if p == t and pat[0...M-1] = txt[i, i+1, ...i+M-1]
            if j==M:
                print ("Pattern found at index " + str(i))
 
        # Calculate hash value for next window of text: Remove
        # leading digit, add trailing digit
        if i < N-M:
            t = (d*(t-ord(txt[i])*h) + ord(txt[i+M]))%q
 
            # We might get negative values of t, converting it to
            # positive
            if t < 0:
                t = t+q
 
# Driver Code
txt = "GEEKS FOR GEEKS"
pat = "GEEK"
 
# A prime number
q = 101
 
# Function Call
search(pat,txt,q)

```

#### Implementation of KMP


```
# Python program for KMP Algorithm
def KMPSearch(pat, txt):
    M = len(pat)
    N = len(txt)
 
    # create lps[] that will hold the longest prefix suffix
    # values for pattern
    lps = [0]*M
    j = 0 # index for pat[]
 
    # Preprocess the pattern (calculate lps[] array)
    computeLPSArray(pat, M, lps)
 
    i = 0 # index for txt[]
    while i < N:
        if pat[j] == txt[i]:
            i += 1
            j += 1
 
        if j == M:
            print ("Found pattern at index", str(i-j))
            j = lps[j-1]
 
        # mismatch after j matches
        elif i < N and pat[j] != txt[i]:
            # Do not match lps[0..lps[j-1]] characters,
            # they will match anyway
            if j != 0:
                j = lps[j-1]
            else:
                i += 1
 
def computeLPSArray(pat, M, lps):
    len = 0 # length of the previous longest prefix suffix
 
    lps[0] # lps[0] is always 0
    i = 1
 
    # the loop calculates lps[i] for i = 1 to M-1
    while i < M:
        if pat[i]== pat[len]:
            len += 1
            lps[i] = len
            i += 1
        else:
            # This is tricky. Consider the example.
            # AAACAAAA and i = 7. The idea is similar
            # to search step.
            if len != 0:
                len = lps[len-1]
 
                # Also, note that we do not increment i here
            
            else:
                lps[i] = 0
                i += 1
 
txt = "ABABDABACDABABCABAB"
pat = "ABABCABAB"
KMPSearch(pat, txt)

```