% Preparing for Interview
% Ravikiran K.S.
% January 1, 2006

# Preparing for interview

## Overview
* What a company expects out of an ideal candidate?
    - Smart and gets things done
* Research about the company you want to work with
    - Read their blog posts, social media, finance reviews and discover problems they have
    - Get insider referral. Network well before need, network often; give first, ask later
    - Join programmer/freelancer/entrepreneur communities in city, volunteer, help others
* Research about the job profile & the interviewer
    - Read job description carefully, talk to insider about team, work culture, & business
* General guidelines for any interview
    - Never lie or make up an answer. If you don't know, say it. Show interest to know answer
    - Remain calm, focussed, and smiling. Give attention to all interviewers, not just a few
    - People skills are as important. We are humans, we are emotional, we hire those we like
    - Never, ever, ever call someone's baby ugly. People are attached a lot to their creation
    - No one is going to hear you, until they know they have been heard. Again, again & again
PS: 99% of people you criticize will not see it their fault - mainly because it
hurts their self-worth & stops them from believing in themselves. A man
convinced against his will is of the same opinion still. - Benjamin Franklin

## Algo Problems
* Problem composition
    - Mostly use basic maths, even when explanation appears highly mathematical
        - Often have all required details hidden within the question
    - Are mostly simple at core, no matter how lengthy the description Or choice of word is
        - You can break them into simple steps, no matter how cryptic it sounds
    - Commonly have a corner/edge case that require a special handling
        - Set of inputs/criteria that are valid, but require special handling of that case
* They test ability to solve problems, use programming language, and perform under pressure
* Basic process of solving problem     (nothing can be automated by computer what can't be solved manually)
    - Read through the problem completely, atleast twice
    - Solve the problem manually with 3 sets of sample data
    - Optimize the manual steps, if you can
    - Write manual steps in comments
    - Re-write logic in comments using real code
    - Optimize the real code, if you can

Example: Implement a function that will take a title in form of a string and return the string with correct capitalization for the title according to these rules:
    * Always capitalize first word in a title
    * Always capitalize last word in a title
    * Lower case these words unless they are first/last words: "a","the","to","at","in","with","and","but","or"
    * Upper case any words not in list above
Sample Data:
    * "i love solving problems and it is fun" to "I Love Solving Problems and It Is Fun"
    * "wHy DoeS A biRd Fly?" to "Why Does a Bird Fly?"
Solve problem:
* Understand the problem
    - tokenize words from a given string called title
    - identify & capitalize first & last words from string
        - capitalize: change first letter to capital, and all remaining letters to small
    - capitalize any other words in middle unless they fall under set of words to be kept lower
    - lower case words that match given set of words
    - combine words back into the sentence
* Solve problem manually with 3 sets of data
    - tokenize: "i love solving problems and it is fun" -> "i" "love" "solving" "problems" "and" "it" "is" "fun"
    - capitalize first/last: "I" "love" "solving" "problems" "and" "it" "is" "Fun"
    - upper case middle words not in list: "I" "Love" "Solving" "Problems" "and" "It" "Is" "Fun"
    - lower case words matching list: <NA>
    - tokenize: "wHy DoeS A biRd Fly?" -> "wHy" "DoeS" "A" "biRd" "Fly?"
    - capitalize first/last: "WHy" "DoeS" "A" "biRd" "Fly?"
    => Doesn't seem correct. "WHy" should've been "Why". So, define capitalize. With that try capitalize again
    - capitalize first/last: "Why" "DoeS" "A" "biRd" "Fly?"
    - capitalize middle words not in list: "Why" "Does" "A" "Bird" "Fly?"
    - lower case words matching list: "Why" "Does" "a" "Bird" "Fly?"
* Optimize manual step
    - tokenize words from a given string called title
    - capitalize all words, except those in the list
        - to capitalize, first lower case all characters in word
        - then upper case the first character
    - for those matching list, lower case all the characters
    - combine words back into the sentence
* Write psuedo-code in comments
    /** title casing logic **/
    /* tokenize words from given string */
    /* capitalize fist word */
    /* capitalize last word */
    /* for each word[1] to word [n - 1]
        /* if in the list */
            /* lower case the word */
        /* otherwise */
            /* capitalize the word */
    /* combine words back into string */

    /** capitalize word **/
    /* lower case the given word */
    /* capitalize first character */

    /** lower case word **/
    /* for all characters in given word */
        /* lower case the character */
* Convert into a real code in C
    title-case(char *title)
    {
        char *p = strtok(str, " ");
        ret += sprintf(ret, "%s", capitalize(p));
        while (p) {
            p = strtok(NULL, " ");
            if (match-list(p)) {
                ret += sprintf(ret, "%s", lowercase(p));
            } else {
                ret += sprintf(ret, "%s", capitalize(p));
            }
        }
    }

    capitalize(char *str)
    {
        lowercase(str);
        str[0] = toupper(str[0]);
    }

    lowercase(char *str)
    {
        char *p;
        for (p = str; p != '\0'; p++) {
            *p = tolower(*p);
        }
    }
* Optimize the real code
    title-case(char *title)
    {
        char *l = strrchr(title, " ");
        char *p = strtok(title, " ");

        if (!title || !p) return;

        while (p) {
            /* first word OR last word OR not in list */
            if ((p == title) || (!strcmp(p, l, strlen(p)) || !match-list(p)) {
                ret += sprintf(ret, "%s", capitalize(p));
            } else {
                ret += sprintf(ret, "%s", lowercase(p));
            }
            p = strtok(NULL, " ");
        }
    }

    capitalize(char *str)
    {
        lowercase(str);
        str[0] = toupper(str[0]);
    }

    lowercase(char *str)
    {
        char *p;
        for (p = str; p != '\0'; p++) {
            *p = tolower(*p);
        }
    }
* More algorithmic problems at: topcoder.com, projecteuler.net, & codility.com

* Common Mistakes
    - Not understanding the problem completely; solving wrong problem
    - Skipping manual steps and jumping right into the code
    - Facts about working under pressure (time limits, competition, motivation)
        - tasks where physical strength & effort is important, time constraints & rewards tend to increase the pace at which someone finishes the task. Example, athletes push their limits in competition
        - tasks where mental & creative effort is important, time constraints & rewards tend to diminish one's ability to finish a task. Example, solving problems, composing great music.
        - hence it depends on both the individual & type of task they are performing. In very tense situations, some "rise to the occasion" and some "shrink under pressure". Motivation & self-confidence affect the psychology.

## Typical Questions
* For subject questions, a precise definition, usage context, & examples are sufficient.
    - Ultimate parent of all classes - Object Class. Everything is an object, can be used to pass on inputs as generic parameters by typecasting them to an Object, provides common accessor api's that can be reused/extended.
    - Encapsulation: Restricting acccess to data and placing data + accessor methods together in a single place.
    - Polymorphism: Same object that can play different roles, can be extended by different sub-classes.
    - Composition: One class is made up of several other classes. Structure in a structure, class in a class.
    - Inheritance: Inherit the properties of base class, which can be resued or extended by inheriting class.
    - Design Pattern: A common pattern/template/method for solving particular type of problem, a best practice.
        - Singleton Pattern: At any point in time, there can only be a single instance of a given class.
* Personality questions are asked for various purposes, to know different things about you
    - Why are you looking for change? - Can be asked to know when you would leave a job, what demotivates you, can you handle pressure? would you stand firm at tough times? What to expect if they hire you?
        - Don't make it negative: remarks on either company, group, team, manager, or individual don't sell
        - Talk about what attracts you in target company/job/prospects? what motivates you? what can you contribute?
    - What are your strengths/weaknesses? Tricky slope and very subjective. Same thing can be seen as a strength or weakness. Nit-picking or detail-oriented, perfectionist or arrogant, fast or poor quality.
        - Strength is "capacity to look at big picture", weakness "detail oriented"
        - Give example of when you/team had a disagreement and how it was resolved. You can find many people who want their view to be accepted as correct, but very few who have humility & patience to know other's point of view.

## Data Structure Algorithms
* Data structure & algorithms are rarely implemented from scratch, have less significance in everyday work
* Still these are most commonly asked questions because
    - They form fundamentals of computer knowledge and good tool to filter out competition
    - Space & time efficiency is demanded in few fields, even to extent of make/break deal
    - Interviewer only knows those questions. He'll ask them regardless what you think of it

* Big O notation - space & time efficiency
    - O(1) Constant Time - It takes same X time quantum regardless of the size of dataset
    - O(log N) Logarithmic Time - Increase of time take for increase in dataset is in logarithm scale
        - Ex. Avg. CPU usage over last 5mins. Size of dataset to be operated doesn't change with no. of samples.
    - O(N) Linear Progression - If it takes X time to handle 1 element, takes ``N*X time to handle N elements``
        - Example is simple loop from 0 to N. Keeps on linearly increasing with N
    - ``O(N^2) Quadratic Progression - It take N*N`` time quantum to do N operations
        - Example is nested loops: one inside another, each running till N
    - O(C^N) Exponential Progression - With increase in dataset, the number of operations multiplies by some fold
        - Example is fibonacci series.

* Bit manipulation
    - Convert decimal to binary - 
        * Lengthy method
        1. For given decimal number N, find largest power of 2 (P) less than N
        2. Subtract P from N & store reminder back in N (N - P = R, N = R). Write 1.
        3. Divide P by 2 (P = P/2) until P < N. For each failed case write 0.
        4. Again when P < N, do N - P = R and N = R. Write 1.
        5. Repeat steps 2 to 4 till N diminishes to 0.
        Ex. Convert 2551 to binary. N = 2551
        - P = 2048 is largest power of 2 <= 2551. So, 2551 - 2048 = 503, N = 503. Write 1.
        - Divide P = P/2 until P <= N. 1024, 512 > 503. Write 00.
        - P = 512/2 = 256 is largest power of 2 <= 503. So, 503 - 256 = 247, N = 247. Write 1.
        - Divide P = P/2 until P <= N. 256/2 = 128 <= 247. So, 247 - 128 = 119, N = 119. Write 1.
        - Divide P = P/2 until P <= N. 128/2 = 64 <= 119. So, 119 - 64 = 55, N = 55. Write 1.
        - Divide P = P/2 until P <= N. 64/2 = 32 <= 55. So, 55 - 32 = 23, N = 23. Write 1.
        - Divide P = P/2 until P <= N. 32/2 = 16 <= 23. So, 23 - 16 = 7, N = 7. Write 1.
        - Divide P = P/2 until P <= N. 16/2 = 8 > 7. Write 0.
        - P = 8/2 = 4 is largest power of 2 <= 7. So, 7 - 4 = 3, N = 3. Write 1.
        - Divide P = P/2 until P <= N. 4/2 = 2 <= 3. So, 3 - 2 = 1, N = 1. Write 1.
        - Divide P = P/2 until P <= N. 2/1 = 1 <= 1. So, 1 - 1 = 0, N = 0. Write 1.
        - Output 100111110111
        * Easy method
        1. For any given decimal number, keep on dividing it by 2 until remainder 1 is divided.
        2. For each division, either remainder will be 1 or 0. Whatever it is, note down.
        3. When all divisions are done, group those remainders starting from latest division first.
        Ex. Convert 2551 to binary.
        - 2551/2 = 1275,1; 1275/2 = 637,1; 637/2 = 318,1; 318/2 = 159,0; 159/2 = 79,1;
        - 79/2 = 39,1; 39/2 = 19,1; 19/2 = 9,1; 9/2 = 4,1; 4/2 = 2,0; 2/2 = 1,0; 1/2 = 0,1
        - Organizing last to first: 100111110111
    - Binary to Decimal
        - 2 to the power method. Start from right most bit in the number.
        - Assign value of 2 to power of given bit position (starting index 0)
        - Multiple given power of two at given position with bit value (0 or 1)
        - Add all values together to find decimal value.
        - Example: 1101 => 2^3 * 1 + 2^2 * 1 + 2^1 * 0 + 2^0 * 1 = 8 + 4 + 0 + 1 = 13
        - Doubling method is much easier. Start from left most 1 in the number.
        - double the previous total and add current digit. To start with previous total is 0.
        - Example: 1101 => 0 * 2 + 1 = 1; 1 * 2 + 1 = 3; 3 * 2 + 0 = 6; 6 * 2 + 1 = 13
    - Bitwise Operators
        - AND: 1 & 1 = 1, 1 & 0 = 0, 0 & 1 = 0, 0 & 0 = 0
        - OR:  1 | 1 = 1, 1 | 0 = 1, 0 | 1 = 1, 0 | 0 = 0
        - XOR: 1 ^ 1 = 0, 1 ^ 0 = 1, 0 ^ 1 = 1, 0 ^ 0 = 1
        - NOT: !1 = 0, !0 = 1
        - Left Shift: Drop left most bit, add 0 on right. Result: Multiply by 2.
        - Right Shift: Drop right most bit, add 0 on left. Result: Divide by 2.
        Example: Count the number of 1's in given number
        - Brute force: Right shift till (size * 8) bits, checking for right most bit for 1 & counting.
        - Best Way: While N is non-zero, do N = N & (N - 1), increment the count.
            - N = 7: 7(111) & 6(110) = 110; 6(100) & 5(011) = 100

* Data Structures
    - Arrays
        - Reverse an array in place
            - start ptr from 0 & nth index towards each other swapping values until pointers cross/touch each other
        - Shift array elements one place to left
            - use temp variable to hold current value at index N, move N+1 value to N, N+2 to N+1, and so on.
        - Find all pairs in array that add upto X. O(n) and in-place
            - if unsorted, make a sorted array.
            find_pair(int sorted_arr[], int sum)
            {
                int i = 0, j = sizeof(sorted_arr)/sizeof(sorted_arr[0]);

                while (i < j) {
                    if ((sorted_arr[i] + sorted_arr[j]) == sum) {
                        printf("%d,%d\n", sorted_arr[i], sorted_arr[j]);
                    } else if ((sorted_arr[i] + sorted_arr[j]) < sum) {
                        i++;
                    } else if ((sorted_arr[i] + sorted_arr[j]) > sum) {
                        j--;
                    }
                }
            }
        - Remove all duplicates from array

    - Linked Lists
        - Reverse a linked list
        reverse_list(node *head)
        {
            node *new_head = NULL;

            while (head) {
                node *next = head->next;
                head->next = new_head;
                new_head = head;
                head = next;
            }
            return new_root;
        }
        - Detect loops in a linked list (Floyd Method)
            - Use two ptrs. Move one by one, & other by 2. If these two meet at some node, there is a loop.
        detect_loop(node *head)
        {
            node *slow = head, *fast = head;
            while(slow && fast && fast->next) {
                slow = slow->next;
                fast = fast->next->next;
                if (slow == fast) {
                    return 1;
                }
            }
            return 0;
        }

    - Trees
        - Traverse tree in any of the 3 orders - pre-order, in-order, post-order
            - pre-order traversal: root, left, right
            - in-order traversal: left, root, right
            - post-order traversal: left, right, root
        - Find depth of a given tree
        tree_maxdepth(node *root)
        {
            int ldepth = 0, rdepth = 0;

            if (root == NULL) {
                return 0;
            } else {
                ldepth = tree_maxdepth(root->left);
                rdepth = tree_maxdepth(root->right);
                if (ldepth > rdepth) {
                    return ldepth + 1; /* +1 for self/root node */
                } else {
                    return rdepth + 1; /* +1 for self/root node */
                }
            }
        }
        - Find the number of elements in given tree. Traverse in any of the 3 orders
            - At each recursion, if (root == NULL) return 0; Otherwise return left + right + 1;

    - Stack (LIFO)
        - Implement push, pop, and peek. peek reads (not pop) top most element in stack.

    - Queue (FIFO)
        - Implement enqueue and dequeue
        - Implement a queue using two stacks
            * Enqueue costly operation
            - enqueue
                - while stack1 is not empty, push everything from stack1 to stack2
                - push x to stack1, push everything in stack2 back to stack1
            - dequeue
                - if stack1 is not empty, pop an item and return
            * Dequeue costly operation
            - enqueue
                - if stack1 isn't full, push incoming element into stack1
            - dequeue
                - if stack2 is not empty, pop an item from stack2 and return
                - otherwise, while stack1 isn't empty, push everything from stack1 to stack2

    - Sorting
        - Bubble sort: O(N^2). Compare two adjacent indexes & swap (if in wrong order). Repeat until no swap is done
        for (i = 0; i < n; i++) {
            swapped = 0;
            for (j = n; j > (i + 1); j--) {     /* array[0..i] in final position */
                if (array[j] < array[(j - 1)]) {
                    swap(array[j], array[(j - 1)]);
                    swapped = 1;
                }
            }
            if (!swapped) break;
        }
        - Selection sort: O(n^2). For each 0..n-1, find/select the next smallest in unsorted list and swap it
        for (i = 0; i < n; i++) {
            k = i;
            for (j = (i + 1); j < n; j++) {
                if (array[j] < array[k]) k = j;     /* k is smallest of array[i..n] */
            }
            if (i != k) swap(array[i], array[k]);   /* move smallest to final position */
        }
        - Merge sort: O(n*log(n)). Divide array into two, sort them individually and merge them back by comparison
        m = (n / 2);    /* split array into half */
        sort(array[0], m);      /* sort first half */
        sort(array[m+1], n);    /* sort other half */
        /* merge two sorted arrays comparing elements in each */
        merge(array[0], m, array[m+1], n);
        - Insertion sort: O(n^2). 
        for (i = 2; i < n; i++) {
            for (j = i; j > 1 && (array[j] < array[(j - 1)]); j--) {
                swap(array[k], array[(k - 1)]);
            }
        }
        - Quick sort: typical O(n*log(n)), worst case O(n^2). Select a pivot, move items smaller than pivot to left, larger than pivot to right. Repeat until sorted.

    - Searching
        - Linear Search: Traverse through entire list searching for given element
        - Binary Search: In a sorted array/list, find middle element. If search value is less than middle element, repeat the steps in left half; if greater, repeat the steps in right half. Continue until either searched element is found or size of sub-list is <= 1.

    - SMP Concepts
        - Race Condition: A behavior where output is dependent on sequence or timing of other uncontrollable events.
        - VM Thrashing: A condition when virtual memory subsystem is constantly paging there by avoiding other applications from running. Can happen if multiple processes of VM size much greater than available RAM are run at the same time.
        - Cache Thrashing: A condition where CPU is busy reloading cache due to frequent cache misses. Can happen if code and data locality of reference is wide spread in memory region leading to frequent cache misses and reload of the cache.
