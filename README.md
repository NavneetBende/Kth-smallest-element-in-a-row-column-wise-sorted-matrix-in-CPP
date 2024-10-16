Kth smallest element in a row-column wise sorted matrix in C++
Here, in this page we will discuss the program to find Kth smallest element in a row-column wise sorted matrix in C++ programming language. We will discuss different approaches in this page.

Kth smallest element in a row-column wise sorted matrix in C++
Method Discussed :
Method 1 : Without finding GCD.
Method 2 : Using GCD approach.
Method 3 : Recursive approach
Let’s discuss above three methods in brief,

Method 1 :
Store the three elements in the array.

Set result = 1
Find a common factors of two or more array elements.
Multiply the result by common factor and divide all the array elements by this common factor.
Repeat steps 2 and 3 while there is a common factor of two or more elements.
Multiply the result by reduced (or divided) array elements.
Method 1 : Code in C++
Run
#include <bits/stdc++.h>
using namespace std;

struct HeapNode {
    int val; 
    int r; 
    int c; 
};
 
void minHeapify(HeapNode harr[], int i, int heap_size)
{
    int l = i * 2 + 1;
    int r = i * 2 + 2;
     if(l < heap_size&& r<heap_size && harr[l].val < harr[i].val && harr[r].val < harr[i].val){
            HeapNode temp=harr[r];
            harr[r]=harr[i];
            harr[i]=harr[l];
            harr[l]=temp;
            minHeapify(harr ,l,heap_size);
            minHeapify(harr ,r,heap_size);
        }
          if (l < heap_size && harr[l].val < harr[i].val){
            HeapNode temp=harr[i];           
            harr[i]=harr[l];
            harr[l]=temp;
            minHeapify(harr ,l,heap_size);
        }
}

int kthSmallest(int mat[4][4], int n, int k)
{
    if (k < 0 || k >= n * n)
        return INT_MAX;
 
    HeapNode harr[n];
    for (int i = 0; i < n; i++)
        harr[i] = { mat[0][i], 0, i };
 
    HeapNode hr;
    for (int i = 0; i < k; i++) {
    
        hr = harr[0];
 
        int nextval = (hr.r < (n - 1)) ? mat[hr.r + 1][hr.c]: INT_MAX;
 
        harr[0] = { nextval, (hr.r) + 1, hr.c };
 
        minHeapify(harr, 0, n);
    }
 
    return hr.val;
}

int main()
{
    int mat[4][4] = {
        { 10, 20, 30, 40 },
        { 15, 25, 35, 45 },
        { 25, 29, 37, 48 },
        { 32, 33, 39, 50 },
    };
    cout << "6th smallest element is "<< kthSmallest(mat, 4, 6);
    return 0;
}
Output
6th smallest element is 29
Method 2 :
By using a comparator, we can carry out custom comparison in priority_queue. We will use priority_queue<pair<int,int>> for this.

Method 2 : Code in C++
Run
#include<bits/stdc++.h>
using namespace std;
 
int kthSmallest(int mat[4][4], int n, int k)
{
    auto cmp = [&](pair<int,int> a,pair<int,int> b){
        return mat[a.first][a.second] > mat[b.first][b.second];
    };
    
    priority_queue<pair<int,int>,vector<pair<int,int>>,decltype(cmp)> pq(cmp);
    for(int i=0; i<n; i++){
        pq.push({i,0});
    }
    
    for(int i=1; i<k; i++){
        auto p = pq.top();
        pq.pop();
        
        if(p.second+1 < n) pq.push({p.first,p.second + 1});
    }
    
    return mat[pq.top().first][pq.top().second];
}
 
int main()
{
    int mat[4][4] = {
        { 10, 20, 30, 40 },
        { 15, 25, 35, 45 },
        { 25, 29, 37, 48 },
        { 32, 33, 39, 50 },
    };
    cout << "6th smallest element is " << kthSmallest(mat, 4, 6);
    return 0;
}
Output
6th smallest element is 29
