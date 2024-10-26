// New Design Questions

1: The 3-byte sequence 11100000 10000000 10100001 will encode the code point U+00 in UTF-0021. Which is corresponding to the character of exclamation mark !
This is because: Starting byte is 11100000 which is a 3 byte character. Next two bytes are starting with 10 which means they are continous bytes. from the first byte we take 0000 and from the second two we get 000000 and 100001. This becomes 0000000000100001. This is 21 in hex or ! mark.

2: Since U+0021 is an ASCII character (less than U+007F), here are all possible encodings.
The standard 1-byte encoding (correct way):
00100001 (0x21)

Overlong 2-byte encoding:
11000000 10100001

Overlong 3-byte encoding (what was given in the question):
11100000 10000000 10100001


3:A character with exactly three encodings would be any character in the range U+0080 to U+07FF.Let's take 'ñ' (U+00F1) as anexampl.

2-byte (standard): 11000011 10110001
3-byte (overlong): 11100000 10000011 10110001
4-byte (overlong): 11110000 10000000 10000011 10110001

4: The main issues with overlong encodings are security vulnerbilities,Inconsistent string comparisons, Wasted space, UTF-8 validation complexity. This means that for security vulnerbility an attacker can bypass security checks by using non standard encodings. For inconsistant string comparisons means that the same character might have different byte representatoins which makes equality checks unreliable and it will waste space because using more bytes than necessary increases storage and bandwidth usage and finaly for UTF-8 validation complexity means that programs need more complicated code to check for and manage overlong encodings.


//Previous Design Questions
1:  UTF-32 uses a fixed 4 bytes per code point, making it simple to index and manipulate - just multiply the index by 4. However, this simplicity comes at a storage cost. For English text (ASCII), UTF-32 uses 4x more space than UTF-8. UTF-8 is more space-efficient for ASCII-heavy text, using just 1 byte for ASCII characters, but requires more complex processing to find the nth character or to parse. For example, to find the 5th character in UTF-8, you need to parse the string from the beginning, while in UTF-32 you can jump directly to byte 20 (5 × 4).

2:  The leading '10' in continuation bytes serves as a synchronization mechanism. If a program starts reading in the middle of a UTF-8 stream (like when seeking in a file), it can easily find the start of the next valid code point by skipping bytes beginning with '10' until it finds a start byte. Without this, a program might misinterpret continuation bytes as start bytes, leading to data corruption or display errors.
