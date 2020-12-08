@def title = "206. Reverse Linked List"
@def published = "26 September 2019"
@def tags = ["Leetcode", "Solution"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

__problem description:__

[https://leetcode.com/problems/reverse-linked-list/](https://leetcode.com/problems/reverse-linked-list/)

__thought:__

iteration solutions:

initial two pointers(`prev`&`cur`) as `NULL`, `head->next` for iteration.

reversing `head` and  iteration.

__solution:__

## iteration
### C++

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev=NULL;
        ListNode* cur;
        while(head!=NULL){
            cur=head->next;
            head->next=prev;
            prev=head;
            head=cur;
        }
        return prev;
    }
};

```

### Python

```py
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur,prev=head,None
        while cur:
            cur.next,prev,cur=prev,cur,cur.next
        return prev
```


## recursion

### C++

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL||head->next == NULL)
            return head;
        ListNode* cur = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return cur;
    }
};
```
