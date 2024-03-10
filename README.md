# important-coding-snippets
sliding window :-
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int i=0,j=0,n=nums.size();double ans=INT_MIN,sum=0;
        while(j<n){
            //sum+=nums[j];
            if((j-i+1)<k){
                j++;
            }
            else if((j-i+1)==k){
                //ans=max(ans,sum/k);
                //sum-=nums[i];
                i++;
                j++;
            }
        }
        //return ans;
    }
};

binary search :-
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int start=0;int end=nums.size()-1;int ans=-1;
        while(start<=end){
            int mid=(start+end)/2;
            if(target==nums[mid]){
                ans=mid;
                break;
            }
            else if(target>nums[mid]){
                start=mid+1;
            }
            else{
                end=mid-1;
            }
        }
        return ans;
    }
};

Two pointers :-
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n=nums.size();set<vector<int>>s;
        vector<vector<int>>v;
        sort(nums.begin(),nums.end());
        for(int i=0;i<n;i++){
            int l=i+1,h=n-1;
            while(l<h){
                int cur=nums[i]+nums[l]+nums[h];
                if(cur==0){
                    s.insert({nums[i],nums[l],nums[h]});
                    l++;
                    h--;
                }
                else if(cur<0){
                    l++;
                }
                else{
                    h--;
                }
            }
        }
        for(auto i:s){
            v.push_back(i);
        }
        return v;
    }
};

Heap :-
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        map<int,int>mp;
        for(auto i:nums){
            mp[i]++;
        }
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>>minh;
        for(auto i=mp.begin();i!=mp.end();i++){
            minh.push({i->second,i->first});
            if(minh.size()>k){
                minh.pop();
            }
        }
        vector<int>v;
        while(!minh.empty()){
            pair<int,int>p=minh.top();
            v.push_back(p.second);
            minh.pop();
        }
        return v;
    }
};

Monotonic Stack :-
class Solution {
public:
    vector<int> nsl(vector<int> &h){
        vector<int>v;stack<pair<int,int>>s;
        for(int i=0;i<h.size();i++){
            if(s.size()==0) v.push_back(-1);
            else if(s.size()>0 && s.top().first<h[i]){
                v.push_back(s.top().second);
            }
            else if(s.size()>0 && s.top().first>=h[i]){
                while(s.size()>0 && s.top().first>=h[i]){
                    s.pop();
                }
                if(s.size()==0) v.push_back(-1);
                else{
                    v.push_back(s.top().second);
                }
            }
            s.push({h[i],i});
        }
    return v;
    }
    vector<int> nsr(vector<int> &h){
        vector<int>v;stack<pair<int,int>>s;
        for(int i=h.size()-1;i>=0;i--){
            if(s.size()==0) v.push_back(h.size());
            else if(s.size()>0 && s.top().first<h[i]){
                v.push_back(s.top().second);
            }
            else if(s.size()>0 && s.top().first>=h[i]){
                while(s.size()>0 && s.top().first>=h[i]){
                    s.pop();
                }
                if(s.size()==0) v.push_back(h.size());
                else{
                    v.push_back(s.top().second);
                }
            }
            s.push({h[i],i});
        }
        reverse(v.begin(),v.end());
    return v;
    }
    int largestRectangleArea(vector<int>& h) {
        vector<int>v1,v2;int ans=0;
        v1=nsl(h);
        v2=nsr(h);
        for(int i=0;i<h.size();i++){
            ans=max(ans,(h[i]*(v2[i]-v1[i]-1)));
        }
        return ans;
    }
};

