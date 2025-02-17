#include <iostream>  // Include standard input/output library 

using namespace std;  // Use standard namespace for simplicity 

// Node structure for AVL Tree 
struct Node {   
    int key;              // Key value of the node 
    Node* left;           // Pointer to the left child 
    Node* right;          // Pointer to the right child 
    int height;           // Height of the node for balancing 

    // Constructor to initialize a new node 
    Node(int k) : key(k), left(nullptr), right(nullptr), height(1) {}   
}; 

// Function to get the height of a node 
int height(Node* node) {   
    return node ? node->height : 0;  // Return height or 0 if node is null 
} 

// Function to update the height of a node based on its children 
int updateHeight(Node* node) {   
    return max(height(node->left), height(node->right)) + 1;  // Max height of children + 1 
} 

// Function to calculate the balance factor of a node 
int getBalance(Node* node) {   
    return node ? height(node->left) - height(node->right) : 0;  // Difference between left and right heights 
} 

// Perform a right rotation on the subtree rooted at 'y' 
Node* rightRotate(Node* y) {   
    Node* x = y->left;         // 'x' becomes the new root 
    Node* T = x->right;        // Temporary node to hold subtree 

    x->right = y;              // Perform rotation 
    y->left = T; 

    y->height = updateHeight(y);  // Update heights 
    x->height = updateHeight(x); 

    return x;  // Return new root 
} 

// Perform a left rotation on the subtree rooted at 'x' 
Node* leftRotate(Node* x) {   
    Node* y = x->right;        // 'y' becomes the new root 
    Node* T = y->left;         // Temporary node to hold subtree 

    y->left = x;               // Perform rotation 
    x->right = T; 

    x->height = updateHeight(x);  // Update heights 
    y->height = updateHeight(y); 

    return y;  // Return new root 
} 

// Function to insert a key into the AVL tree 
Node* insert(Node* node, int key) {   
    if (!node) return new Node(key);  // If tree is empty, create a new node 

    // Recursively insert key into left or right subtree 
    if (key < node->key)   
        node->left = insert(node->left, key);  // Insert into left subtree 
    else if (key > node->key)   
        node->right = insert(node->right, key);  // Insert into right subtree 
    else   
        return node;  // Duplicate keys are not allowed 

    // Update the height of the current node 
    node->height = updateHeight(node); 

    // Get the balance factor to check for imbalance 
    int balance = getBalance(node); 

    // Perform rotations to balance the tree if needed 

    if (balance > 1 && key < node->left->key)  // Left Left Case 
        return rightRotate(node); 

    if (balance < -1 && key > node->right->key)  // Right Right Case 
        return leftRotate(node); 

    if (balance > 1 && key > node->left->key) {  // Left Right Case 
        node->left = leftRotate(node->left);   
        return rightRotate(node); 
    } 

    if (balance < -1 && key < node->right->key) {  // Right Left Case 
        node->right = rightRotate(node->right);   
        return leftRotate(node); 
    } 

    return node;  // Return the unchanged node 
} 

// Function to search for a key in the AVL tree 
bool search(Node* node, int key) {   
    if (!node) return false;         // If node is null, key not found 
    if (key == node->key) return true;  // If key matches, return true 
    if (key < node->key)               // If key is smaller, search in left subtree 
        return search(node->left, key); 
    return search(node->right, key);   // Otherwise, search in right subtree 
} 

// Function for inorder traversal of the AVL tree 
void inorder(Node* node) {   
    if (node) {   
        inorder(node->left);             // Traverse left subtree 
        cout << node->key << " ";        // Print current node's key 
        inorder(node->right);            // Traverse right subtree 
    }   
} 

// Main function to demonstrate the AVL Tree operations 
int main() {   
    Node* root = nullptr;  // Initialize an empty tree 
    // Insert elements into the AVL tree 

    root = insert(root, 10);   
    root = insert(root, 20);   
    root = insert(root, 5);   
    root = insert(root, 15);   
    root = insert(root, 30);   

    // Print the tree using inorder traversal 
    cout << "Inorder traversal: ";   
    inorder(root);   
    cout << endl; 

    // Search for specific keys in the tree 
    cout << "Is 15 in the tree? " << (search(root, 15) ? "Yes" : "No") << endl; 
    cout << "Is 25 in the tree? " << (search(root, 25) ? "Yes" : "No") << endl; 

    return 0;   
}
