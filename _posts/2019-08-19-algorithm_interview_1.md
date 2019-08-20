---
layout:     post
title:      algorithm interview questions
subtitle:   medium -- recent
date:       2019-08-19
author:     BY
header-img: img/post-bg-kuaidi.jpg
catalog: true
tags:
    - Xcode
    - iOS
---

## Questions
#1 Deliver the message
> Description:
Given the information of a company's personnel. The time spent by the ith person passing the message is t[i] and the list of subordinates is list[i]. When someone receives a message, he will immediately pass it on to all his subordinates. Person numbered 0 is the CEO. Now that the CEO has posted a message, find how much time it takes for everyone in the company to receive the message? 

- idea:find the time spent to speread everyone is equivalent to find the last one that receive the message !
construct message_time array to accumulate spent time in a manner of BFS
```
int deliverMessage(vector<int> &t, vector<vector<int>> &subordinate) {
        // Write your code here
        int length = t.size(), result = 0;
        queue<int> state_queue;
        state_queue.push(0);
        vector<int> dist(length, 0);
        while(!state_queue.empty()) {
            int top = state_queue.front();
            state_queue.pop();
            for(int i = 0; i < subordinate[top].size(); ++i) {
                if(subordinate[top][i] == -1) continue;
                dist[subordinate[top][i]] = t[top] + dist[top];
                state_queue.push(subordinate[top][i]);
            }
        }
        for(int i = 0; i < length; ++i) result = max(result, dist[i]);
        return result;
    }
```
#2 Circular Array Loop
> Description: You are given an array of positive and negative integers. If a number n at an index is positive, then move forward n steps. Conversely, if it's negative (-n), move backward n steps. Assume the first element of the array is forward next to the last element, and the last element is backward next to the first element. Determine if there is a loop in this array. A loop starts and ends at a particular index with more than 1 element along the loop. The loop must be "forward" or "backward'

- idea: solve the problem in O(n) time with O(1) storage. 
Since we need to find either increasing all the way or decreasing all the way and given
that no zero, thus idx * val > 0 will satisfy the condition.
And we set all visited index value be zero to avoid repeating loop !
```
    int get_next(vector<int>& nums, int idx, int length) {
        return ((idx + nums[idx]) % length + length) % length;
    }
    
    bool circularArrayLoop(vector<int> &nums) {
        // Write your code here
        int length = nums.size();
        for(int idx = 0; idx < length; ++idx) {
            if(nums[idx] == 0) continue;
            int val = nums[idx], slow = idx, fast = get_next(nums, idx, length);
            while(val * nums[fast] > 0 && val * nums[get_next(nums, fast, length)] > 0) {
                if(slow == fast) {
                    if(slow == get_next(nums, slow, length)) break;
                    return true;
                }
                slow = get_next(nums, slow, length);
                fast = get_next(nums, get_next(nums, fast, length), length);
            }
            slow = idx;
            while(val * nums[slow] > 0) {
                int next = get_next(nums, slow, length);
                nums[slow] = 0;
                slow = next;
            }
        }
        return false;
    }
```

