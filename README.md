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