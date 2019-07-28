# My_JavaScript_NoteBook
This is my JavaScript notes.

# Private fields + Public accessors == encapsulation
```
    public class A {
        public int a;
    }
```
 It violates the encapsulation approach
 
 ```
    public class A {
            private int a; 

            public void setA(int a) { 
                this.a =a;
            } 

            public int getA() { 
                return this.a;
            }

    }
```

# WTF is memoization?
   -  Similar to caching
   - Caching is everywhere
  For example, L1, L2, L3 cache in the CPU, Database buffer (InnoDB buffer pool in MySQL), local cache in the browser (e.g local storage, IndexDB), HTTP cache control…etc.


  # Pure Function
  1. Same Input => Same Output
    ```
    const add = (x, y) => x + y;

    add(2, 4); // 6

    ```
    


# Why to use 'this'?
    To access the information stored in object, we need to call object name.
    For example user.sayHi(), we need to call user
    At this point, we could also use this.sayHi(). Here this refers to user

    But why we use 'this' instead of direct name of object?
    Let see another example
    ```
        let user = {
        name: "John",
        age: 30,

        sayHi() {
            alert( user.name ); // leads to an error
        }

        };

        let admin = user;
        user = null; // overwrite to make things obvious

        admin.sayHi(); // Whoops! inside sayHi(), the old name is used! error!

    ```
    Explanation: Here user is copied to admin object and calling admin method, it 
                  will access wrong object i.e user
  
    * return the object itself from every call
    ```
        let ladder = {
            step: 0,
            up() {
                this.step++;
            },
            down() {
                this.step--;
            },
            showStep: function() { // shows the current step
                alert( this.step );
            }
        };

        ladder.up();
        ladder.up();
        ladder.down();
        ladder.showStep(); // 1
    ```
    To call ladder.up().up().down().showStep(); // error

    ```
    let ladder = {
        step: 0,
        up() {
            this.step++;
            return this;
        },
        down() {
            this.step--;
            return this;
        },
        showStep() {
            alert( this.step );
            return this;
        }
        }

        ladder.up().up().down().up().down().showStep(); // 1

    ```

# Global object in JavaScript
In a browser it is window and for node.js it is global
Recently, it has been globalThis to support for all environment

Generally global variables are discouraged. There should be few global variables.


# JavaScript Execution on Browser
When you run code on javaScript Engine. Each code is executed on a certain 
Execution Context.
2 Types of Execution Context
    - Global Execution Context
    - Function Execution Context


# Currying and partial in JavaScript
    Currying is a transformation of functions that translates a function from
    callable as f(a,b,c) into callable as f(a)(b)(c).
    Currying doesn't call a function. It just transforms it.

    ```
    function curry(f) { // curry(f) does the currying transform
        return function(a) {
            return function(b) {
            return f(a, b);
            };
        };
    }

    // usage
    function sum(a, b) {
        return a + b;
    }

    let carriedSum = curry(sum);

    alert( carriedSum(1)(2) ); // 3

    ```

    Here sum function doesn't lose anything, it can be directly called too

# Async and await
    Async means it always returns a promise.    

    Await makes JavaScript wait until that promise settles and returns its result
    Await cannot use in regular function
    ```
    async function f() {
        let promise = new Promise((resolve, reject) => {
            setTimeout(() => resolve("done!"), 1000)
        });

        let result = await promise; // wait till the promise resolves (*)

        alert(result); // "done!"
    }

    f();

    ```

    Converting .then/Catch into async/await
    ```
    function loadJson(url) {
        return fetch(url)
            .then(response => {
            if (response.status == 200) {
                return response.json();
            } else {
                throw new Error(response.status);
            }
            })
    }

        loadJson('no-such-user.json') // (3)
        .catch(alert); // Error: 404
    ```

    Converted
    ```
    async function loadJson(url) { // (1)
        let response = await fetch(url); // (2)

        if (response.status == 200) {
            let json = await response.json(); // (3)
            return json;
        }

        throw new Error(response.status);
    }

        loadJson('no-such-user.json')
        .catch(alert); // Error: 404 (4)
    ```

    * Call async from non-async
    ```
        async function wait() {
            await new Promise(resolve => setTimeout(resolve, 1000));
            return 10;
        }

        function f() {
            // ...what to write here?
            // we need to call async wait() and wait to get 10
            // remember, we can't use "await"
        }
    ```
    Solution
    ```
        async function wait() {
            await new Promise(resolve => setTimeout(resolve, 1000));
            return 10;
        }

        function f() {
            // shows 10 after 1 second
            wait().then(result => alert(result));
        }

        f();
    ```

# Modules in JavaScript
Module is just a file, a single script.
It has 2 directives
    - export
    - import
 To work on modules we need to declare module in script tag
  '<script type="module"></script>'       
  Then we can work as 
  ```
    <!doctype html>
    <script type="module">
        import {sayHi} from './say.js';

        document.body.innerHTML = sayHi('John');
    </script>
  ```
    Features
    - Modules always `use strict` mode by default. So top-level 'this' is undefined
    - old browsers don't support modules
    - Async works on inline scripts.
    -To load external scripts from another origin (domain/protocol/port), CORS     headers are needed.
    - Duplicate external scripts are ignored.
    - Module code is executed only once. Exports are created once and shared between importers.