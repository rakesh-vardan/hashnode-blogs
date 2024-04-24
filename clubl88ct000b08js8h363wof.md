---
title: "[Java] Reverse words in a String"
datePublished: Thu Mar 28 2024 18:48:20 GMT+0000 (Coordinated Universal Time)
cuid: clubl88ct000b08js8h363wof
slug: reverse-words-in-a-sentence
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713935275613/a949a1db-5be9-436d-b7d6-59a86d35f498.png
tags: java, coding, string

---

### Problem Statement:

[https://leetcode.com/problems/reverse-words-in-a-string/](https://leetcode.com/problems/reverse-words-in-a-string/)

```java
String input = "Hello World!";
String output = "World! Hello"
```

### Solution-1

```java
import org.testng.annotations.Test;

import java.util.Arrays;

import static org.assertj.core.api.Assertions.assertThat;

public class Solution1 {

    String input1 = "Hello World!";             // "World! Hello"
    String input2 = " Hello World! ";           // "World! Hello"
    String input3 = "Hello  World its  me ";    // "me its World Hello"

    public String reverseWordsS1(String sentence) {
        String[] words = sentence.split(" ");
        StringBuilder reversedString = new StringBuilder();

        // to remove the empty strings in the array - corner case
        String[] modifiedWordsArray = Arrays.stream(words)
                                            .filter(s -> !s.isEmpty())
                                            .toArray(String[]::new);
        
        for (int i = modifiedWordsArray.length - 1; i >= 0; i--) {
            reversedString.append(modifiedWordsArray[i]).append(' ');
        }
        return reversedString.toString().trim();
    }

    @Test
    public void testLogic() {
        assertThat(this.reverseWordsS1(input1)).isEqualTo("World! Hello");
        assertThat(this.reverseWordsS1(input2)).isEqualTo("World! Hello");
        assertThat(this.reverseWordsS1(input3)).isEqualTo("me its World Hello");
        assertThat(this.reverseWordsS1(input4)).isEqualTo("Hello");
    }
}
```

In this code, the `reverseWords` function takes a sentence as input, splits the sentence into an array of words, and then appends the words in reverse order to a `StringBuilder` object. Finally, it returns the reversed string. If any any case, there are empty Strings in the given array, we need to filter them as well.

If you run this code with "Hello World!" as input, it will print: "World! Hello".

### Solution-2:

Another approach is to use a stack. Stack follows a last-in, first-out (LIFO) principle which can be used to reverse the words in a sentence.

```java
import org.testng.annotations.Test;

import java.util.Arrays;
import java.util.Stack;

import static org.assertj.core.api.Assertions.assertThat;

public class Solution2 {

    public String reverseWordsS2(String sentence) {
        String[] words = sentence.split(" ");
        Stack<String> stack = new Stack<>();

        String[] modifiedWordsArray = Arrays.stream(words)
                                        .filter(s -> !s.isEmpty())
                                        .toArray(String[]::new);

        for (String word: modifiedWordsArray) {
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

We can also use Java 8's Stream API to reverse the words in a sentence.

```java
import java.util.Arrays;
import java.util.stream.Collectors;

public class Solution3 {
    public String reverseWordsS3(String sentence) {
        String[] words = sentence.split(" ");
        String[] modifiedWordsArray = Arrays.stream(words)
                                        .filter(s -> !s.isEmpty())
                                        .toArray(String[]::new);

        return Arrays.stream(modifiedWordsArray)
                                .reduce((firstWord, secondWord) -> secondWord + " " + firstWord)
                                .orElse(sentence);
    }
}
```

In this version of the code, it uses the `split` method to divide the sentence into an array of words. This [`Arrays.stream`](http://Arrays.stream) creates a Stream of this array. The `reduce` operation then combines these words in reverse order. The `orElse` method returns the original sentence if it consists of only one word without space. Finally, the reversed sentence is returned.

### Solution-4:

Another approach using the Deque interface in Java. A Deque (short for Double Ended Queue) is a data structure that allows you to insert and remove elements from both ends. This makes it very suitable for problems involving reversal or rotation.

```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.List;
import java.util.ArrayList;

public class Solution4 {
   public String reverseStringS4(String sentence) {
        String[] words = sentence.split(" ");

        // to remove the empty strings in the array - another approach
        List<String> list = new ArrayList<>(Arrays.asList(words));
        list.removeIf(String::isEmpty);

        String[] modifiedArray = list.toArray(new String[0]);
        Deque<String> stack = new ArrayDeque<>();

        for (String word : modifiedArray) {
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

### Comparative table:

| Approach | Advantages | Disadvantages | Time Complexity | Space Complexity |
| --- | --- | --- | --- | --- |
| 1\. Split & Loop | \- Easy to understand.  - Simple implementation. | \- Uses extra space for creating the array and StringBuilder. | O(n) | O(n) |
| 2\. Using Stack | \- Consistent with principles of data structure. | \- Uses extra space for creating the stack and StringBuilder. | O(n) | O(n) |
| 3\. Using Stream API | \- Clean and concise syntax.  - The functional style can improve readability. | \- Can be slower due to the overhead of Streams.  - May be hard to understand for those not familiar with functional programming. | O(n) | O(n) |
| 4\. Using Deque | \- Well covers the concept of data structures. | \- Uses extra space for the Deque and StringBuilder. | O(n) | O(n) |

### Notes:

* In the time complexity, 'n' is the length of the input string.
    
* All of the mentioned approaches have the same time complexity **(O(n))** because they all traverse the string once.
    
* All the other approaches use additional data structures and thus, have a linear space complexity **(O(n))**. Also, space complexity includes both fixed and variable space.
    
* [GitHub](https://github.com/rakesh-vardan/daily-practice/blob/master/src/test/java/com/me/coding/problems/leetcode/string/ReverseWordsInString.java)