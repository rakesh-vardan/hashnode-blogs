---
title: "Reverse words in a sentence"
datePublished: Thu Mar 28 2024 18:48:20 GMT+0000 (Coordinated Universal Time)
cuid: clubl88ct000b08js8h363wof
slug: reverse-words-in-a-sentence

---

### Solution-1

```java
public class Solution1 {
    public static void main(String[] args) {
        String sentence = "Hello World!";
        System.out.println(reverseWords(sentence));
    }

    public static String reverseWords(String sentence) {
        String[] words = sentence.split(" ");
        StringBuilder reversedString = new StringBuilder();

        for (int i = words.length - 1; i >= 0; i--) {
            reversedString.append(words[i]).append(' ');
        }

        return reversedString.toString().trim();
    }
}
```

In this code, the `reverseWords` function takes a sentence as input, splits the sentence into an array of words, and then appends the words in reverse order to a `StringBuilder` object. Finally, it returns the reversed string.

If you run this code with "Hello World!" as input, it will print: "World! Hello".

### Solution-2:

Another approach is to use a stack. Stack follows a last-in, first-out (LIFO) principle which can be used to reverse the words in a sentence.

```java
import java.util.Stack;

public class Solution2 {
  public static void main(String[] args) {
      String sentence = "Hello World!";
      System.out.println(reverseWords(sentence));
  }

  public static String reverseWords(String sentence) {
      String[] words = sentence.split(" ");
      Stack<String> stack = new Stack<String>();

      for (String word: words) {
          stack.push(word);
      }

      StringBuilder reversedString = new StringBuilder();

      while (!stack.isEmpty()) {
          reversedString.append(stack.pop()).append(' ');
      }
      return reversedString.toString().trim();
  }
}
```

In this version of the code, it splits the sentence into words and then pushes each word onto a stack. It then pops each word off the stack, which results in the words being in reverse order, and appends them to a `StringBuilder` object. Finally, it returns the reversed string.

### Solution-3:

The third approach will not use `split` or `Stack`, but will reverse the words in-place through a series of character swaps.

```java
public class Solution3 {
  
    public static void main(String[] args) {
          String sentence = "Hello World!";
          System.out.println(reverseWordsInPlace(sentence.toCharArray()));
      }

      public static String reverseWordsInPlace(char[] s) {
          int i = 0, j = 0, l = s.length; 
          while (i < l) { 
              while (i < j || i < l && s[i] == ' ') {
                  i++; 
              }
              while (j < i || j < l && s[j] != ' ') { 
                  j++; 
              }
              reverse(s, i, j - 1); 
          }   
          return new String(s);
      }

      public static void reverse(char[] s, int i, int j) {
          while (i < j) {
              char temp = s[i];
              s[i++] = s[j];
              s[j--] = temp;
          }
      }
  }
```

In this version of the code, the `reverseWordsInPlace` function initially separates the string into individual words and then calls the `reverse` function on each word. `reverse` function performs pairwise swaps starting from the beginning and end of each word and moving toward the middle. The result is each word being mirrored around its center. It is also more efficient as it processes the string in place without creating any extra data structure.

### Solution-4:

We can also use Java 8's Stream API to reverse the words in a sentence.

```java
import java.util.Arrays;
import java.util.stream.Collectors;

public class Solution4 {
    public static void main(String[] args) {
        String sentence = "Hello World!";
        System.out.println(reverseWords(sentence));
    }

    public static String reverseWords(String sentence) {
        String reversed = Arrays.stream(sentence.split(" "))
                .reduce((firstWord, secondWord) -> secondWord + " " + firstWord)
                .orElse(sentence);

        return reversed;
    }
}
```

In this version of the code, it uses the `split` method to divide the sentence into an array of words. This [`Arrays.stream`](http://Arrays.stream) creates a Stream of this array. The `reduce` operation then combines these words in reverse order. The `orElse` method returns the original sentence if it consists of only one word without space. Finally, the reversed sentence is returned.

### Solution-5:

Another approach using the Deque interface in Java. A Deque (short for Double Ended Queue) is a data structure that allows you to insert and remove elements from both ends. This makes it very suitable for problems involving reversal or rotation.

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution5 {
  
   public static void main(String[] args) {
     String sentence = "Hello World!";
     System.out.println(reverseWords(sentence));
   }

   public static String reverseWords(String sentence) {
     String[] words = sentence.split(" ");
     Deque<String> stack = new ArrayDeque<>();

     for (String word : words) {
         stack.push(word);
     }

         StringBuilder reversedSentence = new StringBuilder();

         while (!stack.isEmpty()) {
             reversedSentence.append(stack.pop());
             if (!stack.isEmpty()) {
                 reversedSentence.append(" ");
             }
         }

         return reversedSentence.toString();
     }
}
```

In this code, the `reverseWords` function splits the sentence into an array of words and pushes each word onto a stack (implemented as an `ArrayDeque`). Then it pops each word off the stack (which results in the words being in reverse order) and appends them to a `StringBuilder`. If the stack is not empty after popping a word, it adds a space to the `StringBuilder`. Finally, the function returns the reversed sentence.

### Comparative table for those five solutions:

| Approach | Advantages | Disadvantages | Time Complexity | Space Complexity |
| --- | --- | --- | --- | --- |
| 1\. Split & Loop | \- Easy to understand. - Simple implementation. | \- Uses extra space for creating the array and StringBuilder. | O(n) | O(n) |
| 2\. Using Stack | \- Consistent with principles of data structure. | \- Uses extra space for creating the stack and StringBuilder. | O(n) | O(n) |
| 3\. In-place reversal | \- No use of extra data structures. - Efficient in memory utilization. | \- Logic is slightly more complex. - In-place mutation can be risky if the data needs to be reused. | O(n) | O(1) |
| 4\. Using Stream API | \- Clean and concise syntax. - The functional style can improve readability. | \- Can be slower due to overhead of Streams. - May be hard to understand for those not familiar with functional programming. | O(n) | O(n) |
| 5\. Using Deque | \- Well covers concept of data structures. | \- Uses extra space for the Deque and StringBuilder. | O(n) | O(n) |

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1711651606884/6b39b2e8-917d-4cb1-afe4-707fb1d427d1.png align="center")

### Notes:

* In the time complexity, 'n' is the length of the input string.
    
* All of the mentioned approaches have the same time complexity **(O(n))** because they all traverse the string once.
    
* However, they differ in space complexity. The in-place reversal approach has a constant space complexity **(O(1))** as it does not use any additional data structure to store the words or characters. The other approaches use additional data structures and thus, have a linear space complexity (O(n)).
    
* Also, space complexity includes both fixed and variable space. Even the in-place reversal approach has to take in account of storing the input, hence actual memory usage is higher.
    
* Different approaches may be more suitable depending on the specific constraints and requirements of your problem (e.g., if speed is more important than memory usage, or if the input size is very large, etc.).