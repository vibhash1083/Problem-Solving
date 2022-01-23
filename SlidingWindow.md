# Sliding Window
Given 
    Primary Data structure - An array or a string or any sequence.
Find
    i) A list of windows from the array matching certain criteria - candidate solutions
    ii) A window among candidate solutions with further constraints

## Observations
i) Only linear unidirectional movement is required. So, array or string remains primary data structure of the problem.
ii) Auxialiary data struture needs to be determined for finding candidate solutions as well as the unique window among candidates if asked for.

## Sample Problem
https://leetcode.com/problems/minimum-window-substring/

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.

Example 1:

Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
Example 2:

Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
Example 3:

Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

## Approach
Step 1: Identify that it's a sliding window problem. We need to find a substring in s which contains all characters of string t. We need to traverse string linearly and look for some windows matching the criteria.

Step 2: Identify characteristics of the window:
- Target string t should be smaller in length than source string s.
- Order in which characters appear in the windows is not important.
- Window should contain all characters of target string t.
- Window may contain other characters which are not present in the target string t. So window size should be greater than equal to length of t.
- Window should contain all characters from target string which means that if target has duplicate characters then frequency of each character in t should be greater than equal to the frequency of those characters in the window.
- Problem specific requirement - Input will contain only upper case English characters. If it's not given, then we should be ready to solve it for all types of characters.

Step 2: Find possible type of candidate solutions (windows).
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

### Additional requirement of the problem:
Find the window of minimum size matching the criteria.

Ex - There were 2 windows of length 3 in above example and those are our finally desired windows.

## Template to solve the sliding window problem.
1. Define window start and window end at start of the source substring.
2. Move the window end towards right to expand the window. Keep moving till end of source.
3. Check the incoming element and determine if the expanded window is a candidate of not.
    - If current window is a candidate.
        - Mark the window
        - Shrink the window by moving the window start pointer towards right.
    - If not, expand the window further.
4. Process the result and provide the answer.

class Solution:
    def isWindowACandidate(window_start, window_end):
        # Check if window is a candidate
        return flag

    def slidingWindow(source, target):
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
        return result

## Solution:
Since the problem requires that frequency of each character in target string should be greater than equal to frequency of those characters in window, we start by calculating the frequency of target first.

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

    



