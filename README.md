# VerifyLinkedList-
Verification of Linked List insert and delete operations using Z3 prover

1_Sorts.theory contains all the type declaration \n
2_ReadList.theory contains all the axioms that capture the effects of traversing through the list
3_UpdateList.theory contains all the axioms capturing the effects of linking a node to another node in the list
z3.exe : The Z3 Theorem Prover itself built for Windows 10 machines

Note: bulk of the axioms are actually inertia/frame axioms stating what does not change after an action/instruction happens

I have classified the file types into two categories:

.theory files contains axioms while .program files represent the axioms of the program to be verified

Testing:

Suppose we want to test Insert.program

1) Concatenate the contents of 1_Sorts.theory, 2_ReadList.theory, 3_UpdateList.theory, Insert.program into a single file (in that order)
2) Copy the contents of the merged file into clipboard
3) Issue command "z3.exe -in" to cmd. z3 now accepts all the axioms from standard input 
4) Paste the axioms. 
5) Try (check-sat): If z3 says "sat" then we are so far good with our target program: Insert.program
                    If z3 says "unsat" then the program Insert.program already violates some rule in our theory. Therefore it would be
                    a wrong program
6) Issue (push): Here we can add extra assertions to test whether the program meets the specification
                 For Insert.program admissibility of temp node in the final state (s4) is crucial
                 (assert (not (admissible temp s4))) ;we want to check admissiblity, therefore we verify it using proof by refutation
                                                     ;ie. we issue (not (admissible temp s4)) to Z3
7)  Try (check-sat): "sat" implies a correct program w.r.t to the last assertion added, "unsat" implies a wrong program w.r.t to the
                      last assertion added
8) (pop) : Now more properties can be tested. We can also add all the properties to be tested on the final state in a logical and clause
           as:  (assert (not (and (admissible temp s4) (member target_key s4))))
           
PPTs: The two ppts contain the explanation of how I approached the verification. "Verify Linked List.pptx" has more info than "Verify Linked List demo.pptx"
