# VerifyLinkedList-
Verification of Linked List insert and delete operations using Z3 prover

<b>1_Sorts.theory</b> contains all the type declaration <br />
<b>2_ReadList.theory</b> contains all the axioms that capture the effects of traversing through the list <br />
<b>3_UpdateList.theory</b> contains all the axioms capturing the effects of linking a node to another node in the list <br/>
<b>z3.exe :</b> The Z3 Theorem Prover itself built for Windows 10 machines

Note: bulk of the axioms are actually inertia/frame axioms stating what does not change after an action/instruction happens

I have classified the file types into two categories:

<b> .theory </b> files contains axioms while <b> .program </b> files represent the axioms of the program to be verified

<b> Testing: </b>

Suppose we want to test Insert.program

1) <b>Concatenate</b> the contents of 1_Sorts.theory, 2_ReadList.theory, 3_UpdateList.theory, Insert.program into a single file (in that order)
2) <b>Copy</b> the contents of the merged file into clipboard
3) <b>Issue command "z3.exe -in" </b> to cmd,  z3 now accepts all the axioms from standard input 
4) <b>Paste </b> the axioms. 
5) <b>Try (check-sat):</b> <br/> 
             &nbsp;&nbsp;&nbsp;      <ul> 
                    <li> If z3 says "sat" then we are so far good with our target program: Insert.program </li>
                    <li> If z3 says "unsat" then the program Insert.program already violates some rule in our theory. <br /> 
                      Therefore it would be a wrong program. </li>
                   </ul>
6) <b>Issue (push): </b> <br/>
                 <p align="left">
                 Here we can add extra assertions to test whether the program meets the specification <br />
                 For Insert.program admissibility of temp node in the final state (s4) is crucial
                 (assert (not (admissible temp s4))) ;we want to check admissiblity, therefore we verify it using proof by refutation ie. we issue (not (admissible temp s4)) to Z3
                 </p>
                 
7) <b>Try (check-sat): </b> <br/>
          <p> "sat" implies a correct program w.r.t to the last assertion added, "unsat" implies a wrong program w.r.t to the  last assertion added </p>
8) <b>(pop) :</b> <br />
            </p> Now more properties can be tested. We can also add all the properties to be tested on the final state in a logical and 
             clause as:  (assert (not (and (admissible temp s4) (member target_key s4))))
            </p>
            
<b>Correct programs: </b> <br/>
                           <p>Insert.program - check for admissiblity of temp and membership of target_key in final state s4
                          <br/> Delete.program - check for non-admissibility of curr in final state s3. Equivalently check for membership                                          target_key in final state s3
                           </p>
                           
                           
<b> Wrong programs: </b>  Insert_wrong1.program, Insert_wrong2.program, Delete_wrong1.program, Delete_wrong2.program

<b>PPTs:</b> <br/> 
         <p> The two ppts contain the explanation of how I approached the verification. "Verify Linked List.pptx" has more info than  "Verify Linked List demo.pptx" </p>
         
<b>Report.pdf</b> has the report of challenges faced and conclusion
