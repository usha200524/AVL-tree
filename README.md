class Main {
    // Node class for AVL tree
    static class Node {
        int data;
        Node left;
        Node right;
        int height;
        
        Node(int data) {
            this.data = data;
            this.left = null;
            this.right = null;
            this.height = 1; // New node is a leaf, so height is 1
        }
    }
    
    // Get height of a node
    private int getHeight(Node node) {
        if (node == null) {
            return 0;
        }
        return node.height;
    }
    
    // Get balance factor of a node
    private int getBalance(Node node) {
        if (node == null) {
            return 0;
        }
        return getHeight(node.left) - getHeight(node.right);
    }
    
    // Update height of a node
    private void updateHeight(Node node) {
        if (node != null) {
            node.height = Math.max(getHeight(node.left), getHeight(node.right)) + 1;
        }
    }
    
    // Right rotation
    private Node rightRotate(Node y) {
        Node x = y.left;
        Node T2 = x.right;
        
        x.right = y;
        y.left = T2;
        
        updateHeight(y);
        updateHeight(x);
        
        return x;
    }
    
    // Left rotation
    private Node leftRotate(Node x) {
        Node y = x.right;
        Node T2 = y.left;
        
        y.left = x;
        x.right = T2;
        
        updateHeight(x);
        updateHeight(y);
        
        return y;
    }
    
    // Insert method for AVL tree
    Node insert(Node root, Node n) {
        // Standard BST insert
        if (root == null) {
            return n;
        }
        
        if (n.data < root.data) {
            root.left = insert(root.left, n);
        } else if (n.data > root.data) {
            root.right = insert(root.right, n);
        } else {
            return root; // Duplicate values not allowed
        }
        
        // Update height of current node
        updateHeight(root);
        
        // Get balance factor
        int balance = getBalance(root);
        
        // Left Left Case
        if (balance > 1 && n.data < root.left.data) {
            return rightRotate(root);
        }
        
        // Right Right Case
        if (balance < -1 && n.data > root.right.data) {
            return leftRotate(root);
        }
        
        // Left Right Case
        if (balance > 1 && n.data > root.left.data) {
            root.left = leftRotate(root.left);
            return rightRotate(root);
        }
        
        // Right Left Case
        if (balance < -1 && n.data < root.right.data) {
            root.right = rightRotate(root.right);
            return leftRotate(root);
        }
        
        return root;
    }
    
    public static void main(String[] args) {
        Node n1 = new Node(10);
        System.out.println(n1.height);
        
        // Example usage of AVL tree
        Main avl = new Main();
        Node root = null;
        int[] values = {20, 30, 5, 15, 25};
        for (int value : values) {
            root = avl.insert(root, new Node(value));
        }
        System.out.println("Root node height: " + root.height);
    }
}
