# Huffman_Code

Implementing the huffman code compression algorithm in Python.

## Usage:

### compress.py
- python compress.py originalFile compressedFile

### decompress.py
- python decompress.py compressedFile decompressedFile

## Implementation:
- The compressed file includes all the information necessary for decompress.py to decompress the file.
- This includes a header portion which includes a representation of the Huffman tree.
- The tree is represented as follows: '0' for each internal node, '1' + ASCII representation for the symbol for each leaf node.
- The tree is traversed in preorder to form the header.
- The header is ended with the second occurrence of 16 '1's (the EOF symbol).
- The first occurrence of 16 '1's is the ASCII representation for the EOF symbol in a tree leaf node.
- The rest of the compressed file is the Huffman tree codes for each character in the original file text.
- The overhead for the header and the extra EOF symbol is listed in the following section.

## Overhead Included in the Algorithm:
- 1 extra bit for each internal node in the Huffman Tree to store in the compressed file.
- 9 extra bits (1 for node, 8 for ASCII representation) for each leaf node.
- 16 extra bits to signify end of the header portion of the compressed file.
- Extra bits for the EOF symbol (17 extra bits: 1 node, 16 ASCII rep. + the Huffman code to represent the symbol)

## Huffman Tree:
- The tree is constructed by counting up the frequencies of each character present in the file to be compressed.
- Then the tree is constructed from the leaves up.
- Each symbol is a leaf node so that no prefixes can exist. This prevents confusion based off parsing the code and being able to decode different symbols from it.
- With the use of a priority queue built upon a binary heap (code in the include directory) the 2 least-frequent nodes are combined to form a new internal node combining the frequencies of the 2 nodes taken to form it.
- This combined node is reinserted into the queue and the next 2 least-frequent nodes are taken and combined.
- This repeats until we have a single node, which will be the root of our tree.
- The code for each symbol (leaf node) is formed by traversing down the tree to each leaf. If a given node is a left-child it gets a '0' and a '1' for a right-child. So the length of the code for each symbol will depend on the level of the tree the symbol is present.
- More frequent symbols will be higher in the tree and therefore have a shorter code to represent it.
- The length of the code also depends on the number of unique symbols found in the text to be compressed.

## Results:

<table>

  <tr>

    <th>File</th><th>Original Size</th><th>Compressed Size</th><th>Ratio</th>

  </tr>

  <tr>

    <td>Test1</td><td>156 bytes</td><td>184 bytes</td><td>+17.95%</td>

  </tr>
  
  <tr>

    <td>Test2</td><td>2850 bytes</td><td>1383 bytes</td><td>-51.47%</td>

  </tr>

  <tr>

    <td>Test3</td><td>830 bytes</td><td>490 bytes</td><td>-40.96%</td>

  </tr>
  
  <tr>

    <td>Test4</td><td>95 bytes</td><td>203 bytes</td><td>+103.68%</td>

  </tr>
  
  <tr>

    <td>Test5</td><td>253309 bytes</td><td>134491 bytes</td><td>-46.95%</td>

  </tr>
  
  <tr>

    <td>Shakespeare</td><td>8250 bytes</td><td>4709 bytes</td><td>-42.92%</td>

  </tr>
  
</table>

### Explanation of Results:
- Test 1 has mostly unique characters with some repeats so the necessary overhead makes the compressed file larger.
- Test 4 has one of each printable ASCII character so there is little possibility for compression.
- Test 2 is a sentence repeated mulitple times allowing for compression.
- Test 3,5 and shakespeare include assorted characters resembling more natural written text.

## Possible Improvements:
- Use of multiple symbol patterns that occur frequently
- Use of different patterns for different portions of the file
- Error Correction
