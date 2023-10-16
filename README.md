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

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.

The base case here is incredibly simple to figure out within the first three lines of code. A simple return for if n <= 1 runs in constant time so we have a time complexity of O(1) regardless. Next, the two recursive calls cut the problem size into 1/3 giving us the following recurrence relation: $T(n) = 2*T(n/3)$ (see above). The next (and perhaps most difficult) part revolves around the behavior of the inner loops. Each of the three may run from 0 to n^2 times. As I learned in 2030, the time complexity of this should be $O(n^5)$ as that's what the product of the loops' operations come out to. ($n^2$ for the first, n for the second, and $n^2$ for the third). Finally we see one more recursive call of mystery cutting the problem space into 1/3 again. Putting it all together we get a final reccurence relation of $T(n) = 3*T(n/3) + O(n^5)$.

Solving the reccurence relation: Instead of going down the traditional path of utilizing substitution, I decided to give the master theorem a shot. Let's begin by defining our parameters to get started: a = 3, b = 3, and $f(n) = O^5$. Now, we can compare f(n) to $n^(log_{b}(a))$. We see that $n^(log_{3}(3)) = n^1$. Comparing f(n) to $n^(log_{3}(3))$, we see that $f(n) = O(n^5)$ is obviously greater than n^1, which means we are to utilize the third case of the Master Theorem. According to the Master Theorem, the time complexity of the recurrence relation $T(n) = 3*T(n/3) + O(n^5)$ is $T(n) = O(n^5 * log(n))$ giving us our tight big-O bound of $O(n^5 * log(n))$
