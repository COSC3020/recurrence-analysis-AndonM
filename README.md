[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-718a45dd9cf7e7f842a935f5ebbe5719a5e09af4491e668f4dbf3b35d5cca122.svg)](https://classroom.github.com/online_ide?assignment_repo_id=11974149&assignment_repo_type=AssignmentRepo)
# Recurrence Analysis -- Mystery Function

Analyze the running time of the following recursive procedure as a function of
$n$ and find a tight big $O$ bound on the runtime for the function. You may
assume that each operation takes unit time. You do not need to provide a formal
proof, but you should show your work: at a minimum, show the recurrence relation
you derive for the runtime of the code, and then how you solved the recurrence
relation.

```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}
```
The base case here is incredibly simple to figure out within the first three lines of code. A simple return for if n <= 1 runs in constant time so we have a time complexity of O(1) regardless. Next, the two recursive calls cut the problem size into 1/3 giving us the following recurrence relation: T(n) = 2*(T(n/3)). The next (and perhaps most difficult) part revolves around the behavior of the inner loops. Each of the three may run from 0 to $n^2$ times. As I learned in 2030, the time complexity of this should be $O(n^5)$ as that's what the product of the loops' operations come out to. ($n^2$ for the first, n for the second, and $n^2$ for the third). Finally we see one more recursive call of mystery cutting the problem space into 1/3 again. Putting it all together we get a final reccurence relation of $T(n) = 3*T(n/3) + O(n^5)$.

Solving the reccurence relation:

$3T(n/3)+n^5$  
$9T(n/9)+n^5+3(n/3)^5$  
$27T(n/27)+n^5+3(n/3)^5+9(n/9)^5$  

Which may be simplified down to:  

$27T(n/27)+n^5+(n^5/3^4)+(n^5/9^4)$  

Rewriting as a summation that runs for i iterations we get:  

$n^5+(n^5/3^4)+(n^5/9^4)

We see a pattern emerges here with every term containing an n^5 in the numerator and each successive term dividing this n^5 by increasing powers of three all raised to the fourth power. Thus, we can represent this behavior with the following sum: \sum_{n=1}^{i}\ (1/3)^{4n}. We can stick an n^5 in front of our sum to account for both the first term and numerator of every term that follows:

n^5 \sum_{n=1}^{i} (1/3)^{4n}$  
$3^iT(n/3^i)+n^5 \sum_{n=1}^{i} (1/3)^{4n}$  

Letting $i = log_3n$, our expression becomes:  

$nT(1)+n^5 \sum_{n=1}^{log_3n} (1/3)^{4n}$  

We may ignore the sum with $log_3n$ as it's bounded by a constant. In conjuction with discarding lower order terms, we get a final complexity as follows:  

$T(1)+n^5 \in \theta (n^5)$
