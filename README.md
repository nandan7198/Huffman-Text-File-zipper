<h1 align='center'> Huffman Text File Zipper</h1>
<img center='align' src='http://4.bp.blogspot.com/-dm7N1haRYeA/UzPmZ67ZyrI/AAAAAAAAAGo/pTBUre_72aY/s1600/Compress_PDF.png'/>
Text files can be compressed to make them smaller and faster to send, and unzipping files on devices has a low overhead. The process of encoding involves changing the representation of a file so that the (binary) compressed output takes less space to store and takes less time to transmit while retaining the ability to reconstruct the original file exactly from its compressed representation. Text files can be of various file types, such as HTML, JavaScript, CSS, .txt, and so on. Text compression is required because uncompressed data can take up a lot of space, which is inconvenient for device storage and file sharing.
This project is based on Huffman Coding, a lossless, bottom-up compression algorithm. It can compress and decompress any text files.
We use **Huffman's algorithm** to construct **a tree** that is used for data compression. 
We assume that each character has an associated weight equal to the number of times the character occurs in a file,
When compressing a file we'll need to calculate these weights.

## Huffman Coding Algorithm Description

Huffman's algorithm assumes that we're building a single tree from a group (or forest) of trees. 
Initially, all the trees have a single node with a character and the character's weight. 
Trees are combined by picking two trees, and making a new tree from the two trees. 
This decreases the number of trees by one at each step since two trees are combined into one tree.

## Implementation:

**Struct Node**: Represents a node of Huffman Tree which is generated during compression or decompression of files. It stores character data, its frequency, Huffman code, and two pointers that point towards the left or right node if they exist.

Huffman class has only two public functions
1) compress()
2) decompress()
And a constructor which accepts input file and output file. The object of this class can be initiated as follows: **huffman h(inputFileName, outputFileName)**;

### Compressing a Text File:
We use the Huffman Coding algorithm for this purpose which is a greedy algorithm that assigns variable length binary codes for each input character in the text file. The length of the binary code depends on the frequency of the character in the file. The algorithm suggests creating a binary tree where all the unique characters of a file are stored in the tree’s leaf nodes.

->The algorithm works by first determining all of the file’s unique characters and their frequencies.   
->The characters and frequencies are then added to a Min-heap.   
->It then extracts two minimum frequency characters and adds them as nodes to a dummy root.   
->The value of this dummy root is the combined frequency of its nodes and this root node is added back to the Min-heap.   
->The procedure is then repeated until there is only one element left in the Min-heap.  
This way, a Huffman tree for a particular text file can be created.
### Compressing file compress():
Following are the steps followed to compress the input file:

1)**createMinHeap()**: This function reads the input file and stores the frequency of all the characters appearing in the file. It further creates a Min Heap structure based on the frequency of each character using priority queue as a data structure that stores Nodes and uses its frequency as a comparing parameter.

2)**createTree()**: This function generates the Huffman tree by duplicating the Min Heap created earlier keeping the original Min Heap. It pops the two Nodes with the lowest frequency. Further, it assigns these two as left and right nodes to a new Node with a frequency which is the sum of the two popped nodes and pushes this Node back to the Min Heap. This process is continued until Min Heap has a size equal to 1.

3)**createCodes()**: This function traverses the entire Huffman tree and assigns codes in binary format to every Node.

4)**saveEncodedFile()**: This function saves the Huffman encoded input file to the output file.  
![image](https://user-images.githubusercontent.com/129162262/231460244-5405cbf0-14f3-4e79-9ef0-74ff965a7222.png)


minHeap = ({character data} {huffman code for that character}) * minheapsize

{huffman code for that character} = 128 bits divided into 16 decimal numbers. Every number represents 8 bit binary number.
eg: {127 - code.length()} * '0' + '1' (representing start bit) + code = 128 bits
It is converted to 16 * 8-bit decimal numbers = 128 bits

{Encoded input File characters} {count0} = Entire file is converted into its huffman encoded form which is a binary code. This binary string is divided in 8-bit decimal numbers. If the final remaining bits are less than 8 bits, (8 - remainingBits) number of '0's are appended at the end. count0 is the number of '0's appended at the end.

The output file should be (.huf) file which represents it is a Huffman encoded file.
### Decompressing the Compressed File:
->The compressed file is opened, and the number of unique characters and the total number of characters in the file are retrieved.  
->The characters and their binary codes are then read from the file. We can recreate the Huffman tree using this.  
->For each binary code: 
A left edge is created for 0, and a right edge is created for 1. 
Finally, a leaf node is formed and the character is stored within it.
 This is repeated for all characters and binary codes. The Huffman tree is thus recreated in this manner.  
->The remaining file is now read bit by bit, and the corresponding 0/1 bit in the tree is traversed. The corresponding character is written into the decompressed file as soon as a leaf node is encountered in the tree.  
->Step 4 is repeated until the compressed file has been read completely. 
In this manner, we recover all of the characters from our input file into a newly decompressed file with no data or quality loss.


### Decompressing file decompress(): 
Following are the steps followed to decompress the Huffman encoded file.

1)**getTree()**: This function reads the Min Heap stored at the beginning of the file and reconstructs the Huffman tree by appending one Node at a time. MinHeapSize is the first value, next {MinHeapSize * (1+16)} contains character data and 16 decimal values representing 128 bits of binary Huffman code. Ignore the initial (127 - code.length()) of '0's and starting '1' bit and append the Node.

2)**saveDecodedFile()**: This function reads the entire {Encoded input File charachters} and {count0} by ignoring the first {MinHeapSize * (1 + 16)} of the file. The decimal values are converted to their binary equivalent(huffman codes) and the resulting character is appended to the output file by traversing the reconstructed huffman tree. The final count0 number of '0's are ignored since they were extra bits added while saving the encoded file.

## References

*1. [Huffman coding, from WikiWorld](https://www.wikiwand.com/en/Huffman_coding)*

*2. Thomas H. Cormen, Charles E. Leiserson, Ronald L.Rivest, Clifford Stein “Introduction to Algorithms 3rd Edition - Thomas H. Cormen, Charles E. Leiserson, R”*

*3. Weiss, Mark Allen. “Data structures and algorithm analysis in Java." Addison-Wesley Long-man Publishing Co., Inc., 1998.*