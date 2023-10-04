#include <iostream>
#include <unordered_map>
#include <vector>

using namespace std;

class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    bool isEndOfWord;
    
    TrieNode() : isEndOfWord(false) {}
};

class Trie {
public:
    TrieNode* root;
    
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* node = root;
        for(char ch : word) {
            if(node->children.find(ch) == node->children.end()) {
                node->children[ch] = new TrieNode();
            }
            node = node->children[ch];
        }
        node->isEndOfWord = true;
    }
    
    vector<string> findWordsWithPrefix(string prefix) {
        TrieNode* node = root;
        vector<string> result;
        
        for(char ch : prefix) {
            if(node->children.find(ch) == node->children.end()) {
                return result;
            }
            node = node->children[ch];
        }
        
        findAllWords(node, prefix, result);
        return result;
    }
    
    void findAllWords(TrieNode* node, string currentWord, vector<string>& result) {
        if(node->isEndOfWord) {
            result.push_back(currentWord);
        }
        
        for(auto& pair : node->children) {
            findAllWords(pair.second, currentWord + pair.first, result);
        }
    }
};

int main() {
    Trie trie;
    string arr[] = {"apple", "app", "bat", "ball", "cat", "dog"};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    // Insert words into the trie
    for(int i = 0; i < size; ++i) {
        trie.insert(arr[i]);
    }
    
    string prefix;
    cout << "Enter prefix: ";
    cin >> prefix;
    
    // Find words with the given prefix
    vector<string> wordsWithPrefix = trie.findWordsWithPrefix(prefix);
    
    // Output the words with the given prefix
    cout << "Words with prefix '" << prefix << "': ";
    for(const string& word : wordsWithPrefix) {
        cout << word << " ";
    }
    cout << endl;
    
    return 0;
}
//# WordSearch
