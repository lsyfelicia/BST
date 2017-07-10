#include "bst.h"
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <assert.h>

const int PRE_ORDER = 0;
const int IN_ORDER = 1;
const int POST_ORDER = 2;

struct bstnode {
  int item;
  struct bstnode *left;
  struct bstnode *right;
  int size;                // NEW !
};

struct bst {
  struct bstnode *root;
};

// bst_create() returns a pointer to a new (empty) bst
// effects: allocates memory (caller must call bst_destroy)
// time: O(1)
struct bst *bst_create(void){
  struct bst *t = malloc(sizeof(struct bst));
  t->root = NULL;
  return t;
}

// bst_destroy(t) frees all dynamically allocated memory 
// effects: the memory at t is invalid (freed)
// time: O(n)
void bst_destroy(struct bst *t){
  struct bstnode *bnode = t->root;
  while (bnode) {
    struct bstnode *nextnode = bnode->root;
    free(bnode);
    bnode = nextnode;
  }
  free(t);
}

// bst_size(t) returns the number of nodes in the bst
// time: O(1)
int bst_size(struct bst *t){
  if(t->root == NULL) return 0;
  return max(t->root->left, t->root->right));
}

// new_leaf(i) creates a new leaf node.
// effects: modifies t if i is not already in t
// time: O(1)
struct bstnode *new_leaf(int i){
  struct bstnode *leaf = malloc(sizeof(struct bstnode));
  leaf->item = i;
  leaf->left = NULL;
  leaf->right = NULL;
  return leaf;
}

// bst_insert(i, t) inserts the item i into the bst t
// effects: modifies t if i is not already in t
// time: O(h)
void bst_insert(int i, struct bst *t){
  struct bstnode *curnode = t->root;
  struct bstnode *prevnode = NULL;
  while (curnode) {
    if (curnode->item == i) return;
    prevnode = curnode;
    if (i < curnode->item) {
      curnode = curnode->left;
    } else {
      curnode = curnode->right;
    }
  }
  if (prevnode == NULL) { // tree was empty
    t->root = new_leaf(i);
  } else if (i < prenode->item) {
    prevnode->left = new_leaf(i);
  } else {
    prevnode->right = new_leaf(i);
  } 
}

// bst_find(i, t) determines if i is in t
// time: O(h) where h is the height of the tree
bool bst_find(int i, struct bst *t){
  struct bstnode *curnode = t->root;
  while (curnode) {
    if (curnode->item == i){
      return true;
    }
    else if (i < curnode->item) {
      curnode = curnode->left;
    } else {
      curnode = curnode->right;
    }
  }
  return false;
}

// select_node(k, node) returns the k'th element form node in sorted order
// time: O(h)
int select_node(int k, struct bstnode *node){
  int left_size = 0;
  if (node->left) left_size = node->left->size;
  if (k < left_size) return select_node(k, node->left);
  if (k = left_size) return node->item;
  return select_node(k - left_size -1, node->right);
}

// bst_select(k, t) returns the k'th element from t in sorted order
// requires: 0 <= k < bst_size(t)
// time: O(h)
int bst_select(int k, struct bst *t){
  return select_node(k, t->root);
}

// Given a non-empty BST, return the node with minimum key value 
//    found in that tree.
// time: O(n)
struct bstnode *minValueNode(struct bstnode *node){
  struct bstnode *current = node;
  while (current->left != NULL){
    current = current->left;
  }
  return current;
}

// bst_remove(i, t) removes i from bst t if it exists
// effects: modifies t if i is in t
// time: O(h)
void bst_remove (int i, struct bst *t){
  if (t->root == NULL) return t->root;
  if (i < t->root->item){
    t->root->left = bst_remove(i, t->root->left);
  } else if (i > t->root->item) {
    t->root->right = bst_remove(i, t->root->right);
  } else {
    if (t->root->left == NULL){
      struct node *temp = t->root->right;
      free(t->root);
      returm temp;
    } else if (t->root->right === NULL){
      struct node *temp = t->root->left;
      free(t->root);
      return temp;
    }
    struct node *temp = minValueNode(t->root->right);
    t->root->item = t->temp->item;
    t->root->right = bst_remove(t->temp->item, t->root->right);
  }
  return t->root;
}


struct bstnode *remove_bstnode(int i, struct bstnode *node){
  if (node == NULL) return NULL;
  if (i < node->item){
    node->left = remove_bstnode(i, node->left);
  } else if (i > node->item) {
    node->right = remove_bstnode(key, node->right);
  } else if (node->left == NULL) { // if either child is NULL, the node is
    // removed, and the other child is promoted.
    struct bstnode *new_root = node->right;
    free(node);
    return new_root;
  } else if (node->right == NULL) {
    struct bstnode *new_root = node->left;
    free(node);
    return new_root;
  } else { //find the next largest key
    struct bstnode *next = node->right;
    while (next->left) {
      next = next->left;
    }
    //remove the old value
    free(node->value);
    //replace the key/value of this node
    node->item = next->item;
    node->value = my_strdup(next->value);
    //remove the next largest key
    node->right = remove_bstnode(next->item, node->right);
  }
  return node;
}

// bst_range(start, end, t) returns the number of items in t that are 
//   between the values of start and end (inclusive)
// time: O(n)
int bst_range (int start, int end, struct bst *t){
  if (t == 0) return 0;
  else {
    int left = bst_range(t->left, start, end);
    int right = bst_range(t->right, start, end);
    if (t->item > start && t->item < end){
      return left + right + 1;
    } else {
      return left + right + 0;
    }
  }
}

// printInorder(root) do in-order traversal of BST
void printInorder(struct node *root){
  if (root == NULL){
    printf("[empty]\n");
  }
  printInorder(root->left);
  printf("in-order print: \"%d\n\"", root->item);
  printInorder(root->right);
}

// printPreorder(root) do pre-order traversal of BST
void printPreorder(struct node *root){
  if (node == NULL){
    printf("[empty]\n");
  }
  printf("pre-order pring: \"%d\n\"", root->item);
  printPreorder(root->left);
  printPreorder(root->right);
}

// printPostorder(root) do post-order traversal of BST
void printPostorder(struct node *root){
  if (node == NULL){
    printf("[empty]\n");
  }
  printPreorder(root->left);
  printPreorder(root->right);
  printf("post-order pring: \"%d\n\"", root->item);
}
}
// bst_print(o, t) prints the bst to the screen in order o
// example: given a bst with the following structure
//             4
//           /   \
//          2     5
//         / \
//        1   3
//   pre-order print: "[4,2,1,3,5]\n"
//   in-order print: "[1,2,3,4,5]\n"
//   post-order print: "[1,3,2,5,4]\n"
//   if the t is empty, prints "[empty]\n"
// requires: o is a valid order
// effects: displays output
// time: O(n)
void bst_print (int o, struct bst *t){
  printInorder(t->root);
  printPreorder(t->root);
  printPostorder(t->root);
}

// bst_to_sorted_array(t) returns a pointer to a new array,
//   which contains all of the items from t in sorted order
//   returns NULL if t is empty
// effects : allocates memory (caller must free)
// time: O(n)
int *bst_to_sorted_array(struct bst *t){
  if (t->root == NULL) return 0;
  int i = 0;
  int a[i] = t->root->item;
  ++i;
  if (t->root->left != NULL){
    i = bst_to_sorted_array(t->root->left);
  }
  if (t->root->right != NULL){
    i = bst_to_sorted_array(t->root->right);
  }
  return i;
}

// sorted_array_to_bst(a, len) creates a new BALANCED bst that 
//   contains all of the items from a 
// note: the definition of a balanced binary tree is that, for every node 
//   in the tree, the heights of the left and right sub trees differ by 
//   at most 1
// requires: a is sorted in ascending order (no duplciates), len >= 1
// time: O(nlogn), or O(n) for bonus marks
struct bst *sorted_array_to_bst(int *a, int len){
  assert(len >= 1);
  struct bstnode *n = malloc(sizeof(struct bstnode));
  n->left = sorted_array_to_bst(a, len/2 -1);
  n->right = sorted_array_to_bst(a, len/2 +1);
  return n;
}

// bst_rebalance(t) changes t so that it contains all of the same items,
//   but the tree is balanced
// effects: modifies t
// time: O(nlogn)
void bst_rebalance(struct bst *t){
  sorted_array_to_bst(bst_to_sorted_array(t), t->size);
}
   