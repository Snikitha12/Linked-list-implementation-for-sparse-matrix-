# Linked-list-implementation-for-sparse-matrix-
#include <iostream>

class Node {
public:
    int data;
    int row;
    int col;
    Node* next;

    Node(int val, int r, int c) {
        data = val;
        row = r;
        col = c;
        next = nullptr;
    }
};

class SparseMatrix {
public:
    int numRows;
    int numCols;
    Node** rows;

    SparseMatrix(int m, int n) {
        numRows = m;
        numCols = n;
        rows = new Node*[m];
        for (int i = 0; i < m; i++) {
            rows[i] = nullptr;
        }
    }

    void insert(int val, int row, int col) {
        if (row < 0 || row >= numRows || col < 0 || col >= numCols) {
            std::cout << "Invalid row or column index." << std::endl;
            return;
        }

        Node* newNode = new Node(val, row, col);
        if (rows[row] == nullptr || rows[row]->col > col) {
            newNode->next = rows[row];
            rows[row] = newNode;
        } else {
            Node* current = rows[row];
            while (current->next != nullptr && current->next->col < col) {
                current = current->next;
            }
            newNode->next = current->next;
            current->next = newNode;
        }
    }

    void display() {
        for (int i = 0; i < numRows; i++) {
            Node* current = rows[i];
            for (int j = 0; j < numCols; j++) {
                if (current != nullptr && current->col == j) {
                    std::cout << current->data << " ";
                    current = current->next;
                } else {
                    std::cout << "0 ";
                }
            }
            std::cout << std::endl;
        }
    }
};

int main() {
    SparseMatrix matrix(3, 3);

    matrix.insert(1, 0, 0);
    matrix.insert(2, 0, 1);
    matrix.insert(3, 1, 1);
    matrix.insert(4, 2, 2);

    matrix.display();

    return 0;
}
