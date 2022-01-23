# Sliding Window
Given a data structure - an array or a string or any sequence.
Find
1. A list of windows from the array matching certain criteria - candidate solutions
2. A window among candidate solutions with further constraints

## Sample Problem
https://leetcode.com/problems/minimum-window-substring/

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.

Example 1:

Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
Example 2:

## Approach
### Step 1 - Indentification
We need to find a substring in s which contains all characters of string t. We need to traverse string linearly and look for some windows matching the criteria. If we start with a window of size k, then next window can be formed by adding the elements on right and discarding the elements from left. While we traverse through the array, window will slide across it.

Only linear unidirectional movement is required. So, array or string remains primary data structure of the problem.

### Step 2 - Characterization
Identify characteristics of the window:
- Target string t should be smaller in length than source string s.
- Order in which characters appear in the windows is not important.
- Window should contain all characters of target string t.
- Window may contain other characters which are not present in the target string t. So window size should be greater than equal to length of t.
- Window should contain all characters from target string which means that if target has duplicate characters then frequency of each character in t should be greater than equal to the frequency of those characters in the window.
- Problem specific requirement - Input will contain only upper case English characters. If it's not given, then we should be ready to solve it for all types of characters.

### Step 3 - Types of candidates
Find possible type of candidate solutions (windows).
- A window substring with characters in same order as t.
    Ex - s = "ABCBECOD", t = "ABC".
    ABC appears exactly in same order in window starting at index 0 and length 3.
    Window - ABC

- A window substring with characters in different order as t.
    Ex - s = "CABBECOD", t = "ABC".
    ABC appears in different order in window starting at index 0 and length 3.
    Window - CAB

- A window substring with extra characters in window.
    Ex - s = "CXABBECOD", t = "ABC".
    ABC appears in different order in window starting at index 0 and length 4.
    Window - CXAB

- A string may contain multiple windows.
    a) Candidate window at start of source string.
    b) Candidate window somewhere in source string.
    c) Candidate window towards end of source string.
    d) Overlapping windows anywhere in source string
    Ex - s = "ANBCYCXZABBECODABCA", t = "ABC".
    Windows - ANBC (length 4), CXZAB (length 5), ABC (length 3), BCA (length 3)

### Step 4 - Selection
Additional requirement of the problem:
Find the window of minimum size matching the criteria.
Ex - There were 2 windows of length 3 in above example and those are our finally desired windows.

## Template
1. Define window start and window end at start of the source substring.
2. Move the window end towards right to expand the window. Keep moving till end of source.
3. Check the incoming element and determine if the expanded window is a candidate of not.
    - If current window is a candidate.
        - Mark the window
        - Shrink the window by moving the window start pointer towards right.
    - If not, expand the window further.
4. Process the result and provide the answer.


`def slidingWindow(source, target):
    # Define window start and end
    window_start, window_end = 0, 0

    # Move window_end towards right
    for window_end in range(len(source)):
        incoming_element = source[window_end]
        # Check if current window is a candidate

        # Shrink the window by checking as long it's possible
        while windowShrinkCondition:
            # Check and process information
            # Move window_start pointer to right
            window_start += 1
    
    # Process and return the result
    return result`


## Solution
### Window Characterization
Since the problem requires that frequency of each character in target string should be greater than equal to frequency of those characters in window, we start by calculating the frequency of target first.

### Auxiliary Data Structure
We can store frequency of characters in two ways:
1. An array with number of possible characters as it's size. Each index is related to particular character and value is frequency of the character.
For ex - If string can consists of characters from a to e only, then frequency list of "acaacd" will be
[3, 0, 2, 1, 0]
2. A Hashmap/Dictionary with character as key as frequency as value.
For ex - Frequency map of "acaacz" will be {
    a: 3,
    c: 2,
    z: 1
}

Additionally the length of window can't be less than length of target. We can use this property to keep a track of how many characters we have already seen and how many are missing in the window.

def minWindow(s, t):
    need = collections.Counter(t)            # hash table to store char frequency
    missing = len(t)                         # total number of chars we care
    start, end = 0, 0
    i = 0

    for j, char in enumerate(s, 1):          # index j from 1
        if need[char] > 0:
            missing -= 1
        need[char] -= 1
        
        if missing == 0:                     # match all chars
            while i < j and need[s[i]] < 0:  # remove chars to find the real start
                need[s[i]] += 1
                i += 1
            need[s[i]] += 1                  # make sure the first appearing char satisfies need[char]>0
            missing += 1                     # we missed this first char, so add missing by 1
            if end == 0 or j-i < end-start:  # update window
                start, end = i, j
            i += 1                           # update i to start+1 for next window
    return s[start:end]

## Test Cases
1. Invalid input, empty strings.
2. Length of target > length of source.
3. Sources with one window containing elements in order. Window can be at any position in source.
4. Window containing elements not in order.
5. Window with elements not in order and extra characters.
6. Window requiring multiple occurences of same character i.e. target containing duplicate characters.
7. Multiple candidate windows. For ex - out of 3 windows, 2nd one can be smaller than other two.
8. Substring of target appearing just before or after the window.
    For ex - s = "ABABXCNOPAYP", t = "ABC"
    Substring of t, "AB" appears as prefix of window "ABXC".
9. Source with multiple ups and downs across multiple windows.
    For ex 
	Window 1 - length 6
	Window 2 - length 4
	Window 3 - length 5
	Window 4 - length 3
10. Large strings

## Time and Space complexity