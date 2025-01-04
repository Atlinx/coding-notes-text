# Trie
#data-structure 

A Trie is a tree of letters that lets you quickly find occurrences and prefixes of a list of words in a large string. This is also known as a Prefix Tree.

## Implementation
```python
class TrieNode:
	def __init__(self, letter = ''):
		self.letter = letter
		self.children = {}
		self.is_word = False


class Trie:
	def __init__(self):
		self.root = TrieNode()
	
	def insert_word(self, word: str):
		node = self.root
		for c in word:
			if c in node.children:
				node = node.children[c]
			else:
				node.children[c] = node = TrieNode(c)
		node.is_word = True

	def search(self, string: str) -> bool:
		node = self.root
		for c in string:
			if not c in node.children:
				return False
			node = node.children[c]
		return node.is_word

	def starts_with(self, string: str) -> bool:
		node = self.root
		for c in string:
			if not c in node.children:
				return False
			node = node.children[c]
		return True


trie = Trie()
trie.insert_word("hello")
trie.insert_word("help")
trie.insert_word("heat")
trie.insert_word("heaped")
trie.insert_word("appple")
trie.insert_word("apprentice")
trie.insert_word("dire")

def test(word):
	print(f"test: {f"'{word}',":>20}   match: {str(trie.search(word)):>5}   starts_with: {str(trie.starts_with(word)):>5}")

test("dire situation")
test("heat")
test("hello")
test("hell")
test("helloped")
```

## Flashcards
#flashcards/data-structures 

Trie
?
- Tree of letters that lets you quickly find occurrences and prefixes of a list of words in a large string
	- `TrieNode` contains
		- `children` $\to$ dictionary of children
		- `letter` $\to$ current letter
		- `is_word` $\to$ is the current node the ending letter of a word
- Big O
	- $w =$ length of string
	- Insert Word $\to O(w)$
		- Parse each character, and traversing one node each time
	- Search Word/Prefix $\to O(w)$
		- Parse each character, and traversing one node each time