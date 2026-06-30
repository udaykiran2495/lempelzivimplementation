# LZ78 Compression

A C++ implementation of the **LZ78 (Lempel-Ziv 1978)** lossless compression algorithm.

## How it works

LZ78 builds a phrase dictionary on the fly as it scans the input. Each new phrase is encoded as a `(prefix_index, symbol)` pair, where the prefix index references a previously seen phrase and the symbol is the next character. The resulting pairs are packed into a variable-width binary string.

- **Compression**: reads `input.txt`, writes a binary string to `compressed.txt`
- **Decompression**: reads `compressed.txt`, reconstructs the original text into `decompressed.txt`
- **Alphabet**: uppercase A–Z plus `. , ? : ;` and space (6-bit encoding, `MAX_BITS = 6`)

## Files

| File | Purpose |
|---|---|
| `main.cpp` | Compress / decompress (interactive menu) |
| `compare.cpp` | Verify round-trip: checks `input.txt` == `decompressed.txt` |
| `check.cpp` | Cross-check `finalString.txt` against `compressed.txt` |
| `input.txt` | Source text to compress |
| `compressed.txt` | Output of compression step |
| `decompressed.txt` | Output of decompression step |
| `alice29.txt`, `Shannon Text.txt`, `Einstein Text.txt`, `loremipsum.txt` | Sample texts for testing |

## Build & run

```bash
# Compile
g++ -o lz main.cpp
g++ -o compare compare.cpp

# Compress input.txt → compressed.txt
./lz
# Enter 1 at the prompt

# Decompress compressed.txt → decompressed.txt
./lz
# Enter 2 at the prompt

# Verify lossless round-trip
./compare
```

## Limitations

- The fixed alphabet (uppercase letters + a handful of punctuation) means lowercase text or special characters are not supported without modifying `createAlphabetMap()`.
- The phrase index bit-width grows with `log2(phrase_count)`, so very long inputs will use wider codes and compression ratio degrades.

## References

- [Lempel–Ziv–Welch — Wikipedia](http://en.wikipedia.org/wiki/Lempel%E2%80%93Ziv%E2%80%93Welch)
- [LZW compression technique — GeeksforGeeks](https://www.geeksforgeeks.org/lzw-lempel-ziv-welch-compression-technique/)
