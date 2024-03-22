# BST-Operations-C


#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *left;
    struct Node *right;
};

struct Node *createNode(int data) {
    struct Node *newNode = (struct Node *)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->left = newNode->right = NULL;
    return newNode;
}

struct Node *insert(struct Node *root, int data) {
    if (root == NULL)
        return createNode(data);

    if (data < root->data)
        root->left = insert(root->left, data);
    else if (data > root->data)
        root->right = insert(root->right, data);

    return root;
}

void inorderTraversal(struct Node *root) {
    if (root != NULL) {
        inorderTraversal(root->left);
        printf("%d ", root->data);
        inorderTraversal(root->right);
    }
}

struct Node *deleteNode(struct Node *root, int key) {
    if (root == NULL)
        return root;

    if (key < root->data)
        root->left = deleteNode(root->left, key);
    else if (key > root->data)
        root->right = deleteNode(root->right, key);
    else {
        if (root->left == NULL) {
            struct Node *temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct Node *temp = root->left;
            free(root);
            return temp;
        }

        struct Node *temp = root->right;
        while (temp && temp->left != NULL)
            temp = temp->left;

        root->data = temp->data;

        root->right = deleteNode(root->right, temp->data);
    }
    return root;
}

int max(int a, int b) {
    return (a > b) ? a : b;
}

int height(struct Node *root) {
    if (root == NULL)
        return -1;

    int leftHeight = height(root->left);
    int rightHeight = height(root->right);

    return 1 + max(leftHeight, rightHeight);
}

void printLevelAndHeight(struct Node *root, int key, int level) {
    if (root == NULL) {
        printf("Node not found\n");
        return;
    }

    if (root->data == key) {
        printf("Level: %d, Height: %d\n", level, height(root));
        return;
    }

    if (key < root->data)
        printLevelAndHeight(root->left, key, level + 1);
    else
        printLevelAndHeight(root->right, key, level + 1);
}

int main() {
    int arr[] = {30, 20, 40, 10, 25, 35, 45, 5, 15};
    int n = sizeof(arr) / sizeof(arr[0]);

    struct Node *root = NULL;
    for (int i = 0; i < n; i++)
        root = insert(root, arr[i]);

    printf("Inorder traversal of the BST: ");
    inorderTraversal(root);
    printf("\n");

    // Deleting node 10
    root = deleteNode(root, 10);
    printf("Inorder traversal after deleting node 10: ");
    inorderTraversal(root);
    printf("\n");

    printf("Height of the BST: %d\n", height(root));

    int key = 20;
    printf("Printing level and height of node %d:\n", key);
    printLevelAndHeight(root, key, 0);

    return 0;
}
