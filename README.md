Download Link: https://assignmentchef.com/product/solved-csci-4160-project-6
<br>
<strong>Description: </strong>

This is a team project that build on the previous project. In this project, you are required to perform full type checking for Tiger language.

.

<strong>What to do in this project? </strong>

Your major task in this project is to provide implementation for most member functions of class TypeChecking to detect all static semantic errors. The member functions to be implemented are specified by the comments. No other files need to be modified.




When you implement each member function, please refer to the slides at http://www.cs.mtsu.edu/~zdong/4160/public/slides/TypeChecking.pdf




<strong>Classes in this project </strong>

The new class introduced in this project is types::Type and its descendants, which are used to define types associated to variables and type definitions in Tiger language

<ul>

 <li>Class Type: the abstract base class. It defines two virtual member functions o types::Type* actual(): It returns the real data type. See NAME type. o Bool coerceTo(const Type *t): it returns true if t can be converted to current type, otherwise returns false. This function is needed in order to check type compatibility.</li>

 <li>Class ARRAY: represent array data type.</li>

 <li>Class RECORD: represent record data type.</li>

 <li>Class FUNCTION: represent function signature.</li>

 <li>Class INT: represent integer data type</li>

 <li>Class STRING: represent string data type</li>

 <li>Class NIL: represent NIL constant</li>

 <li>Class VOID: represent void type</li>

 <li>Class NAME: represent the alias name of an existing data type. For example, the statement below defines a new data type in Tiger language:</li>

</ul>

<em>type    money = int </em>

Then, a NAME object can be created to represent the money type.

NAME            *n = new NAME(“money);

n-&gt;bind( new types::INT() );

In this case, the statement n-&gt;actual() will return a pointer to types::INT object, which is the real data type of money.




<strong>Tips for the project </strong>

<ul>

 <li>When should I use the member function Type::actual()?</li>

</ul>

ANSWER: Everytime when you return a variable (say t) of (type::Type *) in TypeChecking::visit function, you should return t-&gt;actual() instead of t. For example, see TypeChecking::visit(const VarExp *) in TypeChecking.cpp file provided by the instructor ..

<ul>

 <li>Assume we have the declaration: type::Type *t; after some assignment statement to variable t; how can we check if it actually points to an object of some type, say FUNCTION?</li>

</ul>

ANSWER: Use dynamic_cast. For example, if the Boolean expression below is true, it means t points to a FUNCTION object.

dynamic_cast&lt;const types::FUNCTION *&gt;(t) != NULL

<ul>

 <li>Assume we have declarations: type::Type *t1, *t2; after some assignment statement to both variables, how to check if these two types are compatible?</li>

</ul>

ANSWER: use coerceTo function. If t1.coerceTo(t2) is true, it means type represented by t2 can be converted to the type represented by t1. If t2.coerceTo(t1) is true, it means type represented by t1 can be converted to the type represented by t2.

<ul>

 <li>Each TypeChecking::visit function returns a pointer to types::Type. So if an expression contains a semantic error, what should be returned? For example: expression 1 + “a string” is semantically wrong, and if it is passed to TypeChecking::visit(const OpExp * e), what is supposed to return? ANSWER: If an expression contains a semantic error (undefined variable used, type doesn’t match), always treat the type of the expression as INT.</li>

</ul>




<strong>Test Report: </strong>

In this project, implementation of the project and test case design can be performed simultaneously. Don’t wait too long to design test cases. In this project, please provide a tiger program called: test.tig to cover all of the following scenarios:

<ul>

 <li>Type checking on OpExp like: exp1 operator exp2 o If operator is +, -, *, or /

  <ul>

   <li>exp1 is not an integer                                                 (test case 1)</li>

   <li>exp2 is not an integer                                                 (test case 2)</li>

  </ul></li>

 <li>If operator is =, or &lt;&gt;

  <ul>

   <li>Types of exp1 and exp2 doesn’t match                         (test case 3)</li>

   <li>Exp1 is NIL but exp2 is not an array or record             (test case 4)</li>

   <li>Both exp1 and exp2 are NIL                                    (test case 5)</li>

  </ul></li>

 <li>If operator is &lt;, &lt;=, &gt;, or &gt;=

  <ul>

   <li>One operand is neither INT nor STRING                 (test case 6)</li>

   <li>One operand is INT, and the other one is STRING (test case 7)  Type checking on CallExp like: fname(exp1, exp2, …, expn)</li>

  </ul></li>

 <li>fname is not defined                                                                    (test case 8) o fname is defined as a variable                                                         (test case 9) o too many arguments                                                                         (test case 10) o less arguments than required                                                         (test case 11) o one expression has wrong data type                                        (test case 12)</li>

</ul>

<ul>

 <li>Type checking on AssignExp like: lvalue := exp

  <ul>

   <li>Types of lvalue and exp don’t match                                     (test case 13)</li>

  </ul></li>

 <li>Type checking on IfExp like: if exp1 then exp2 else exp3

  <ul>

   <li>exp1 is not an integer                                                             (test case 14) o if there is no else-clause, exp2 is not VOID type                         (test case 15) o if there is else-clause, types of exp2 and exp3 don’t match         (test case 16)</li>

  </ul></li>

 <li>Type checking on WhileExp o The body of the loop is not VOID type                                     (test case 17)</li>

 <li>Type checking on ForExp like: for varname := exp1 to exp2 do exp3

  <ul>

   <li>exp1 is not an integer expression                                                 (test case 18) o exp2 is not an integer expression                                                  (test case 19) o exp3 is not VOID type                                                                        (test case 20)</li>

  </ul></li>

 <li>Type checking on FunctionDec like: function fname (p1:Type1, …, pn:Typen) : ReturnType = exp

  <ul>

   <li>fname already exists                                                             (test case 21) o The type of an argument is not defined                                               (test case 22) o Two parameters have the same name                                       (test case 23) o The type of exp doesn’t match ReturnType                                   (test case 24)</li>

  </ul></li>

 <li>Type checking on VarDec like: var vname : TypeName := exp       (test case 25) o Type of exp doesn’t match TypeName                                           (test case 26) o If TypeName is omitted, and exp is NIL.                                     (test case 27)</li>

</ul>




Please put the test report, and test.tig in the project6/testing folder of team repository.







<strong>Instructor provided files in the class repository </strong>




The following files are provided by the instructor:

<ul>

 <li>TypeCheckingProject folder. This contains a sample Visual Studio 2010 project.</li>

 <li>pdf: this file</li>

 <li>doc: the rubric used to grade this assignment.  example.txt. sample output</li>

 <li>doc: format of testing report for this project</li>

</ul>





