#include <stdio.h>
#include <stdlib.h>

// Structure for a threaded binary tree node
typedef struct Node
{
    int data;
    struct Node *left, *right;
    int leftThread, rightThread;
} Node;

// Function to create a new node
Node *createNode(int data)
{
    Node *newNode = (Node *)malloc(sizeof(Node));
    newNode->data = data;
    newNode->left = newNode->right = NULL;
    newNode->leftThread = newNode->rightThread = 1;
    return newNode;
}

// Inorder traversal of a threaded binary tree
void inorderTraversal(Node *root)
{
    Node *current = root;
    while (current != NULL)
    {
        // Move to the leftmost node
        while (current->leftThread == 0)
            current = current->left;

        printf("%d ", current->data);

        // Using threads to move to the successor
        while (current->rightThread == 1)
        {
            current = current->right;
            if (current == NULL)
                return;
            printf("%d ", current->data);
        }
        current = current->right;
    }
}

// Insert a node into the threaded binary tree
Node *insert(Node *root, int data)
{
    Node *newNode = createNode(data);
    if (root == NULL)
        return newNode;

    Node *parent = NULL;
    Node *current = root;
    while (current != NULL)
    {
        parent = current;
        if (data < current->data)
        {
            if (current->leftThread == 0)
                current = current->left;
            else
                break;
        }
        else
        {
            if (current->rightThread == 0)
                current = current->right;
            else
                break;
        }
    }

    if (data < parent->data)
    {
        newNode->left = parent->left;
        newNode->right = parent;
        parent->leftThread = 0;
        parent->left = newNode;
    }
    else
    {
        newNode->right = parent->right;
        newNode->left = parent;
        parent->rightThread = 0;
        parent->right = newNode;
    }

    return root;
}

int main()
{
    Node *root = NULL;
    root = insert(root, 20);
    root = insert(root, 10);
    root = insert(root, 30);
    root = insert(root, 5);
    root = insert(root, 15);

    printf("Inorder Traversal of Threaded Binary Tree: \n");
    inorderTraversal(root);

    return 0;
}
