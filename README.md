# MerkleTree

## Introduction to Merkle tree :

The Merkle tree is an essential component of blockchain technology. It is one of the tree data structures which contains a tree full of hashes and the various blocks of data that provides an overview of all the transactions in a session. It creates Hash Key for each and every transaction, such that it enables efficient and secure content authentication in large amounts of data. Merkle trees are used by both Bitcoin and Cryptocurrency. It also aids in the verification of the data's accuracy and quality. Since there is unique hash key involved for each and every transaction, Merkle Tree is sometimes referred as a Hash Tree.

## Architecture of Merkle tree:

In a blockchain, there will be different blocks. All these blocks are connected with the hash keys. And again each block contains different transactions. For each transaction, it generates different hash keys.
 For example, consider that there is a single block containing 4 transactions. Each transaction has its unique hash key. So from all these, we need to find a single hash key of that block. Here initially calculate the Hash of each transaction i.e, Hash1, HAsh2, Hash3, Hash4 for transaction1, 2, 3, 4 respectively. Each of them is hashed again as a pair of two hashes. It means Hashing of Hash1 and Hash2, resulting in Hash12, then hashing of Hash3 and Hash4, resulting in Hash34. The two hashes (Hash12 and Hash34) are then hashed once again to generate the Root Hash or Merkle Root. That is the hash of the Merkle root.
Since it is a binary tree it should contain 2 leaf nodes for every individual parent node. If there is only one leaf node then repeat the same hash key and make it 2 leaf nodes.


![image info](https://miro.medium.com/max/894/1*1e-wyMbvf8-u7Le1LUxTBA.png)

The Merkle Root is kept in the block header. The block header is the section of the bitcoin block that is hashed throughout the mining process. In a Merkle Tree, it holds the hash of the previous block, a Nonce, and the Root Hash of all the transactions in the current block. As a result, including the Merkle root in the block header makes the transaction encrypt. Because this Root Hash contains the hashes of all transactions within the block.

![image info](https://static.javatpoint.com/tutorial/blockchain/images/blockchain-merkle-tree2.png)

## Algorithm

A Merkle tree is constructed by recursively hashing pairs of nodes until there is only one hash.
a, b, c, and d are some data elements and H is a hash function.
a hash function acts as a “digital fingerprint” of some piece of data by mapping it to a simple string with a low probability that any other piece of data will map to the same string.
Each node is created by hashing the concatenation of its “parents” in the tree.

## Illustration of an Algorithm:

![image info](https://miro.medium.com/max/1400/1*prtcx2rVQZmX9oZcyrC_gQ.png)

## Implementation using java:

**MerkTrees.java(LOGIC)**

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;

import javafx.scene.Node;

public class MerkleTree {

    public static Node generateTree(ArrayList<String> dataBlocks) {
        ArrayList<Node> childNodes = new ArrayList<>();

        for (String msg : dataBlocks) {
            childNodes.add(new Node(null, null, HashAlgorithm.generateHash(msg)));
        }

        return buildTree(childNodes);
    }

    private static Node buildTree(ArrayList<Node> children) {
        ArrayList<Node> parents = new ArrayList<>();

        while (children.size() != 1) {
            int index = 0, length = children.size();
            while (index < length) {
                Node leftChild = children.get(index);
                Node rightChild = null;

                if ((index + 1) < length) {
                    rightChild = children.get(index + 1);
                } else {
                    rightChild = new Node(null, null, leftChild.getHash());
                }

                String hashParent = HashAlgorithm.generateHash(leftChild.getHash() + rightChild.getHash());
                parents.add(new Node(leftChild, rightChild, hashParent));
                index += 2;
            }
            children = parents;
            parents = new ArrayList<>();
        }
        return children.get(0);
    }

    private static void printLevelOrderTraversal(Node root) {
        if (root == null) {
            return;
        }

        if ((root.getLeft() == null && root.getRight() == null)) {
            System.out.println(root.getHash());
        }
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        queue.add(null);

        while (!queue.isEmpty()) {
            Node node = queue.poll();
            if (node != null) {
                System.out.println(node.getHash());
            } else {
                System.out.println();
                if (!queue.isEmpty()) {
                    queue.add(null);
                }
            }

            if (node != null && node.getLeft() != null) {
                queue.add(node.getLeft());
            }

            if (node != null && node.getRight() != null) {
                queue.add(node.getRight());
            }

        }

    }

    public static void main(String[] args) {
        ArrayList<String> dataBlocks = new ArrayList<>();
        dataBlocks.add("Java");
        dataBlocks.add("DBMS");
        dataBlocks.add("C++");
        dataBlocks.add("Java script");
        Node root = generateTree(dataBlocks);
        printLevelOrderTraversal(root);
    }
}
```

## WORKING OF ALOGRITHM (OUTPUT)

Actual Output
![image info](https://1.bp.blogspot.com/-qel_meQ_-WU/YKk7LOWIabI/AAAAAAAAt-k/0AnWT3LpgM8xD-kbEznTlhLizUEZczF3QCLcBGAsYHQ/s16000/Screenshot%2B2021-05-22%2Bat%2B10.40.38%2BPM.png)

Now, let's change the s in Java script to capital letter i.e, Java Script
We will get complete different output for that data block. So, it means, its parent hash changes and all its ancestors hash value changes in the tree like below.
![image info](https://1.bp.blogspot.com/-cqwOaUDK_h0/YKk8PcgoFzI/AAAAAAAAt-0/4froByM9UeofivSzHck5wvYxhwv7wpIKQCLcBGAsYHQ/s16000/Screenshot%2B2021-05-22%2Bat%2B10.45.07%2BPM.png)

## References:

1. [Introduction to Merkle Tree](https://www.investopedia.com/terms/m/merkle-tree.asp)
2. [Architecture of Merkle Tree](https://www.javatpoint.com/blockchain-merkle-tree)
2. [Merkel Tree Algorithm](https://www.linkedin.com/pulse/merkle-tree-its-implementation-java-nikhil-goyal/)
3. [Java Code Implementation](https://www.pranaybathini.com/2021/05/merkle-tree.html)
###Deva Bashith
###19BBS0162
