# Estructura-de-datos
import java.util.*;

class BST {
    class Node {
        int key;
        Node left, right;
        Node(int k) { key = k; }
    }

    Node root;

    void insert(int k) {
        root = insertRec(root, k);
    }

    Node insertRec(Node r, int k) {
        if (r == null) return new Node(k);
        if (k < r.key) r.left = insertRec(r.left, k);
        else if (k > r.key) r.right = insertRec(r.right, k);
        return r;
    }

    boolean search(int k, List<Integer> path) {
        return searchRec(root, k, path);
    }

    boolean searchRec(Node r, int k, List<Integer> p) {
        if (r == null) return false;
        p.add(r.key);
        if (k == r.key) return true;
        if (k < r.key) return searchRec(r.left, k, p);
        return searchRec(r.right, k, p);
    }

    void delete(int k) {
        root = deleteRec(root, k);
    }

    Node deleteRec(Node r, int k) {
        if (r == null) return null;
        if (k < r.key) r.left = deleteRec(r.left, k);
        else if (k > r.key) r.right = deleteRec(r.right, k);
        else {
            if (r.left == null) return r.right;
            if (r.right == null) return r.left;
            r.key = minValue(r.right);
            r.right = deleteRec(r.right, r.key);
        }
        return r;
    }

    int minValue(Node r) {
        while (r.left != null) r = r.left;
        return r.key;
    }

    void print() {
        printRec(root, 0);
    }

    void printRec(Node r, int space) {
        if (r == null) return;
        space += 5;
        printRec(r.right, space);
        System.out.println(" ".repeat(space) + r.key);
        printRec(r.left, space);
    }

    void inorder(List<Integer> list) {
        inorderRec(root, list);
    }

    void inorderRec(Node r, List<Integer> list) {
        if (r == null) return;
        inorderRec(r.left, list);
        list.add(r.key);
        inorderRec(r.right, list);
    }
}

public class Main {

    static void buscar(BST arbol, int clave) {
        List<Integer> p = new ArrayList<>();
        boolean ok = arbol.search(clave, p);
        System.out.println("Buscar " + clave + ": " + (ok ? "ENCONTRADO" : "NO está"));
        System.out.println("Recorrido: " + p + "\n");
    }

    public static void main(String[] args) {

        BST A = new BST();
        int[] aVals = {10, 43, 24, -10, 54, 0, 23, 82, 43};
        for (int x : aVals) A.insert(x);

        BST B = new BST();
        int[] bVals = {32, 67, 43, 25, 52, 56, 78, 64, 23, 67};
        for (int x : bVals) B.insert(x);

        BST C = new BST();
        Map<Character,Integer> map = new HashMap<>();
        int base = 1;
        for (char c : "ABCDEFGHIJKLMNOPQRSTUVWXYZ".toCharArray())
            map.put(c, base++);

        char[] cVals = {'A','Y','E','F','G','X','W','U','Z','R','B'};
        for (char c : cVals) C.insert(map.get(c));

        System.out.println("Árbol A:");
        A.print();
        System.out.println("\nÁrbol B:");
        B.print();
        System.out.println("\nÁrbol C:");
        C.print();

        System.out.println("\n--- BÚSQUEDAS A ---");
        buscar(A, 22);
        buscar(A, 0);
        buscar(A, 24);
        buscar(A, 23);

        System.out.println("\n--- BÚSQUEDAS B ---");
        buscar(B, 23);
        buscar(B, 24);
        buscar(B, 25);

        System.out.println("\n--- BÚSQUEDAS C ---");
        buscar(C, map.get('U'));
        buscar(C, map.get('V'));
        buscar(C, map.get('W'));

        int[] addA = {-5,-3,22,44};
        for (int x : addA) A.insert(x);

        int[] addB = {24,26,27};
        for (int x : addB) B.insert(x);

        char[] addC = {'L','M','N','O'};
        for (char c : addC) C.insert(map.get(c));

        A.delete(10);
        A.delete(54);
        A.delete(82);

        List<Integer> finA = new ArrayList<>();
        List<Integer> finB = new ArrayList<>();
        List<Integer> finC = new ArrayList<>();

        A.inorder(finA);
        B.inorder(finB);
        C.inorder(finC);

        System.out.println("\nÁrbol A final (in-order): " + finA);
        System.out.println("Árbol B final (in-order): " + finB);
        System.out.println("Árbol C final (in-order, valores numéricos): " + finC);

        System.out.println("\nÁrbol C final (convertido a letras): ");
        for (int v : finC)
            System.out.print(getLetter(v) + " ");
    }

    static char getLetter(int n) {
        return (char)('A' + (n - 1));
    }
}
