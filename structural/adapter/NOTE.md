# Pluggable Adapter

Sometimes you may want to support multiple adaptees without knowing the interface of each one.

In this case, you probably want to add an in-built interface adaption which can help whoever create the adaptor class adapt different adaptees much easier.

You probably want to define a set of narrow interface. That is the minimalist operations that any adaptees might be performing regardless of what implementation they have.

Based on that we have
    - use abstract operations
    - use delegate object
    - Use parametrized adapter ( this can only be done in functional programming language or language that supports reflection)

- A distinguishing feature of pluggable adapters is that the name of a method called by the client and that existing in the ITarget interface can be different. The adapter must be able to handle the name change. In the previous adapter variations, this was true for all Adaptee methods, but the client had to use the names in the ITarget interface.

- What characterizes a pluggable adapter is that it will have constructors for each of the types that it adapts. In each of them, it does the delegate assignments (one, or more than one if there are further methods for rerouting).

```c#
1    using System;
2
3   // Adapter Pattern - Pluggable           Judith Bishop  Oct 2007
4      // Adapter can accept any number of pluggable adaptees and targets
5      // and route the requests via a delegate, in some cases using the
6      // anonymous delegate construct
7
8      // Existing way requests are implemented
9      class Adaptee {
10        public double Precise (double a, double b) {
11          return a/b;
12        }
13      }
14
15      // New standard for requests
16      class Target {
17        public string Estimate (int i) {
18          return "Estimate is " + (int) Math.Round(i/3.0);
19        }
20      }
21
22          // Implementing new requests via old
23         class Adapter : Adaptee {
24           public Func <int,string> Request;
25
26           // Different constructors for the expected targets/adaptees
27
28           // Adapter-Adaptee
29           public Adapter (Adaptee adaptee) {
30           // Set the delegate to the new standard
31           Request = delegate(int i) {
32               return "Estimate based on precision is " +
33               (int) Math.Round(Precise (i,3));
34           };
35         }
36
37         // Adapter-Target
38         public Adapter (Target target) {
39           // Set the delegate to the existing standard
40           Request = target.Estimate;
41         }
42       }
43
44      class Client {
45
46        static void Main (  ) {
47
48          Adapter adapter1 = new Adapter (new Adaptee(  ));
49          Console.WriteLine(adapter1.Request(5));
50
51          Adapter adapter2 = new Adapter (new Target(  ));
52          Console.WriteLine(adapter2.Request(5));
53
54        }
55      }
56/* Output
57     Estimate based on precision is 2
58     Estimate is 2
59     */
```
