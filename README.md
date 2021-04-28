# Merge k Sorted Lists problem solution (https://leetcode.com/problems/merge-k-sorted-lists/)
```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeKLists(lists []*ListNode) *ListNode {
    var result *ListNode
    for _, l := range lists {
        if result == nil {
            result = l
            continue
        }
        result = mergeSortedLists(result, l)
    }
    
    return result
}

func mergeSortedLists(first, second *ListNode) *ListNode {
    if second == nil && first != nil { return first }
    if first == nil && second != nil { return second }
    
    var aggregator, tail *ListNode
    
    for {
        if first == nil && second != nil {
            tail = appendToTail(tail, second)
            if aggregator.Next == nil {
                aggregator.Next = tail
            }
            break
        }
        
        if second == nil && first != nil {
            tail = appendToTail(tail, first)
            if aggregator.Next == nil {
                aggregator.Next = tail
            }
            break
        }
        
        current := findLowerNode(first, second)
        
        if aggregator != nil && tail != nil {
            tail = appendToTail(tail, current)  
        } else if aggregator != nil && tail == nil {
            tail = current
            aggregator.Next = tail
        } else if aggregator == nil { 
            aggregator = current 
        }
        
        if current == first {
            first = first.Next
        } else if current == second {
            second = second.Next
        }
    }
    
    return aggregator
}

func findLowerNode(first, second *ListNode) *ListNode {
    if first.Val < second.Val {
        return first
    }
    
    return second
}

func appendToTail(tail, node *ListNode) *ListNode {
    if tail == nil {
        return node
    }
    tail.Next = node
    return node
}
```
