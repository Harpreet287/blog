---
title: 'Minimum flips to make xy string'
author: 'Harpreet Singh'
math: true

---

# 1. Minimum flips to make xy string

## Problem Statement

You are given a string consisting of the letters `x` and `y`, such as `xyxxxyxyy`. 
In addition, you have an operation called `flip`, which changes a single `x` to `y` or vice versa.
Determine how many times you would need to apply this operation to ensure that all `x`'s come before all `y`'s.
In the preceding example, it suffices to flip the second and sixth characters, so you should return 2.
Before reading further, I recommend to try it on your own!

## Approach
### Two Pass
A simple yet a beautiful way is for each element, find the number of `y`'s on the left in one pass and number of `x`'s on the next half. For a valid element, all elements on it's left are converted to `x` and vice versa, all elements on the right are converted to `y`. We look out for element for which sum of both(aka our cost) is minimum.

#### Code
```cpp
#include <bits/stdc++.h>
using namespace std;
string s;
void solve(){
    vector<int> cst(s.size(), 0);
    for(int i=1 ; i< s.size(); i++){
        if(s[i-1]=='y')
            cst[i]=1+cst[i-1];
        else
            cst[i]=cst[i-1];
    }
    vector<int> cstx(s.size(), 0);
    for(int i=s.size()-2 ;i>=0; i--){
        if(s[i+1]=='x')
            cstx[i]=1+cstx[i+1];
        else
            cstx[i]=cstx[i+1];
    }
    // Combine two vectors using transform
    vector<int> combined(s.size());
    transform(cst.begin(), cst.end(), cstx.begin(), combined.begin(), plus<int>());
    

    auto it = min_element(combined.begin(), combined.end());
    int res = *it;
    cout<<res<<endl;

}
int a = 1e9+7;
int main(){

    cin>>s;
    solve();
}
```
##### Time Complexity
O(n): Only 2 pass are needed.
##### Space Complexity
O(n) : Extra space used by `cst` and `cstx` and `combined`(completely not needed, just tried STL feature).

### DP
Let `x` = 0 and `y` = 1.
We can use a dp where `dp[i][j]` denotes `i`th index with letter `j`.

And return the minimum of `dp[n-1][0]`and `dp[n-1][1]`.
#### Code
```cpp
#include <bits/stdc++.h>
using namespace std;
string s;

void solve() {
    vector<vector<int>> dp(s.size(), vector<int>(2));
    dp[0][0] = (s[0]=='x'?0:1);
    dp[0][1] = (s[0]=='y'?0:1);
    for (int i = 1; i < s.size(); ++i) {
        // all previous should be x and this one should also be x
        dp[i][0] = dp[i-1][0] + (s[i]=='y'?1:0);
        
        // either start y from this point or start y from the begining
        dp[i][1] = min(dp[i-1][0], dp[i-1][1])+(s[i]=='x'?1:0);
    }
    cout<< min(dp[s.size()-1][0], dp[s.size()-1][1])<<endl;
}
int a = 1e9+7;
int main(){

    cin>>s;
    solve();
}
```
##### Time Complexity
O(n): As we only need to iterate through string once.
##### Space Complexity
O(2*n) : Extra space used by `dp`.
