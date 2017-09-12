---
layout: post
title: Image and Vedio Compression
date: 2017-09-12
tags: Huffman JPEG Image Vedio
categories: Image&Vedio_Processiong
---


### Lossless data compression

One way of compress data would be compress the way they are coded. By means of *Huffman coding*, one can recode the same data and achieve the **minimum redundancy**.

***Huffman coding*** code the data based on their frequency of appearance. It is reasonable to code the most frequently used data(letter, RGB...) using shortest code. 

```cpp
// Driver program to test above functions
int main()
{
    char arr[] = { 'a', 'b', 'c', 'd', 'e', 'f' };
    int freq[] = { 5, 9, 12, 13, 16, 45 };
    int size = sizeof(arr) / sizeof(arr[0]);
 
    HuffmanCodes(arr, freq, size);
 
    return 0;
}
```

The Algorithm for constructing the Huffman Codes, e.g. (implementing *HuffmanCodes(arr, freq, size)*) is 
    1   Create a leaf node for each unique character and build a min heap of all leaf nodes (Min Heap is used as a priority queue. The value of frequency field is used to compare two nodes in min heap. Initially, the least frequent character is at root)
    2   Extract two nodes with the minimum frequency from the min heap.
    3   Create a new internal node with frequency equal to the sum of the two nodes frequencies. Make the first extracted node as its left child and the other extracted node as its right child. Add this node to the min heap.
    4   Repeat **steps-2** and **3** until the heap contains only one node. The remaining node is the root node and the tree is complete.

| characters | frequency |
|------------|-----------|
| a          | 5         |
| b          | 9         |
| c          | 12        |
| d          | 13        |
| e          | 16        |
| f          | 45        |

In order to implement the leaf node, we have
```cpp
// A Huffman tree node
struct MinHeapNode
{
    char data;                // One of the input characters
    unsigned freq;             // Frequency of the character
    MinHeapNode *left, *right; // Left and right child
 
    MinHeapNode(char data, unsigned freq)
    {
        left = right = NULL;
        this->data = data;
        this->freq = freq;
    }
};
```

Now we will finish the Huffman coder step by step:
```cpp
void HuffmanCodes(char data[], int freq[], int size)
{
    //STEP 1： Create a min heap & inserts all characters of data[]
    priority_queue<MinHeapNode*, vector<MinHeapNode*>, compare> minHeap;
    for (int i = 0; i < size; ++i)
        minHeap.push(new MinHeapNode(data[i], freq[i]));
    // to be continued
}
```
We use *priority_queue* to implement the minHeap data structure, by default it guarantees that the top element is the largest element. In our case, we use *compare* to toggle
```cpp
// For comparison of two heap nodes (needed in min heap)
struct compare
{
    bool operator()(MinHeapNode* l, MinHeapNode* r)
    {
        return (l->freq > r->freq);
    }
};
```
By doing so, the smallest elements always at the top. Hence after the for loop, the table would be the same as we inserted. Then we will repeat **steps-2** and **3** until the heap contains only one node, hence, we can use a while loop here:
```cpp
    // Iterate while size of heap doesn't become 1
    while (minHeap.size() != 1)
    {
        // Extract the two minimum freq items from min heap
        left = minHeap.top();
        minHeap.pop();
 
        right = minHeap.top();
        minHeap.pop();
 
        // Create a new internal node with frequency equal to the
        // sum of the two nodes frequencies. Make the two extracted
        // node as left and right children of this new node. Add
        // this node to the min heap
        // '$' is a special value for internal nodes, not used
        top = new MinHeapNode('$', left->freq + right->freq);
        top->left = left;
        top->right = right;
        minHeap.push(top);
    }
```


After the while loop, we will have a heap like this
<img src="http://www.geeksforgeeks.org/wp-content/uploads/fig-5.jpg"></img>

Now we have the Huffman tree, how to code them?
We can **Traverse the tree formed starting from the root**. Maintain an auxiliary array. While moving to the left child, write 0 to the array. While moving to the right child, write 1 to the array. Print the array when a leaf node is encountered.

```cpp
// Prints huffman codes from the root of Huffman Tree.
void printCodes(struct MinHeapNode* root, string str)
{
    if (!root)
        return;
 
    if (root->data != '$')
        cout << root->data << ": " << str << "\n";
 
    printCodes(root->left, str + "0");
    printCodes(root->right, str + "1");
}
```
We can use the *printCodes()* function  by adding
```cpp
printCodes(minHeap.top(), "");
```
in *HuffmanCodes()*. We can further obtain the coding:
<img src="http://www.geeksforgeeks.org/wp-content/uploads/fig-6.jpg"></img>

The Complexity is about $O(n\log n)$.

### Lossy Compression

#### JPEG

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/JPEG_process.svg/598px-JPEG_process.svg.png"></img>

- Input Image: M $\times$ N matrix
- Construct n $\times$ n sub-images (usually 8$\times$ 8)
- Forward Transform
- Quantizer
- Symbol encoder
- Output Compressed Image

In each of the 8$\times$ 8 sub-matrix, we transform $[R,G,B]$ vector for each pixel into a $[Y, C_B, C_R]$ where Y represent luma, e.g. brightness and $C_B$ and $C_R$ representing color.

$$
\begin{align}
[Y, C_B, C_R]^T = A_{3\times 3}[R,G,B]^T
\end{align}
$$
Then each of the pixel with $[Y, C_B, C_R]$ undergoes the Discrete Cosine Transform (*DCT*). A DCT is similar to a Fourier transform in the sense that it produces a kind of spatial frequency spectrum.

<br>
The Quantizer part works as a low band pass filter to filter out the high frequency information.



