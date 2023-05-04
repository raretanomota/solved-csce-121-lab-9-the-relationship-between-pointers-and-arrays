Download Link: https://assignmentchef.com/product/solved-csce-121-lab-9-the-relationship-between-pointers-and-arrays
<br>
The purpose of this lab is to help clarify the relationship between pointers and arrays; you will look at arrays of pointers, pointers to arrays, and the C/C++ treatment of multi-dimensional arrays.

This lab provides example code and you’re expected to work through it, poking at it to try make sense of what’s going on. The text should guide you in that process. By <em>“make sense”</em> I mean that you should be able to draw a diagram showing blocks in memory, and arrows for pointers like I’ve done in the lectures.

Pointers to elements within an array

Copy and paste this into your editor, then build the code and run the result.

This program has an array of chars that stores a string. That string is passed to two functions, which are effectively identical: they print their sole argument. The functions differ only in the type that they expect that argument to be.

#include &lt;iostream&gt;using namespace std; void bracket_function(char str[]){    // This line will print out what is passed on the call-stack to the function:    //C1: cout &lt;&lt; “str = ” &lt;&lt; (void *)str &lt;&lt; endl;    cout &lt;&lt; “Bracket Function: “;    cout &lt;&lt; str &lt;&lt; endl;} void star_function(char *str){    // This line will print out what is passed on the call-stack to the function:    //C2: cout &lt;&lt; “str = ” &lt;&lt; (void *)str &lt;&lt; endl;    cout &lt;&lt; “Star Function: “;    cout &lt;&lt; str &lt;&lt; endl;} int main(){    char lyric[] = “…there’s a Spirit can ne’er be told…”;     star_function(lyric); // L1: Maybe this is a trifle surprising?    bracket_function(lyric);     star_function(&amp;lyric[0]);    bracket_function(&amp;lyric[0]); // L2: And also this?     // These two show how it can actually be rather useful:    // C3: star_function(&amp;lyric[32]);    // C4: bracket_function(&amp;lyric[32]);         return 0;}

As we’ve seen in class, arrays degrade to pointers, so the functions are equivalent.

<ol>

 <li>The array can be passed to the function expecting a pointer (see line L1).</li>

 <li>A pointer can be passed the function expecting an array (see line L2). (The pointer is just the address of the first element of the array.)</li>

 <li>Uncomment C1 and C2, and see what it does. The input is cast (like we’ve seen for ints and floats) in order to lose the typing information. We do this because cout, by default, will show what is at the address when you give it a char[] or char *. Put another way: if cout receives a string, it will automatically dereference it. To print the actual address, not what is at that address, we pretend that it doesn’t contain chars. (If you find the void weird, you can make it an int or some other type, too.)</li>

 <li>Next, uncomment C3. Before you run the result, think about the addresses being passed and form an hypothesis of what you think it’ll do. Did you get it?</li>

 <li>Finally, uncomment C4. Will that compile?</li>

</ol>

If any of the preceding is troubling to you, try to draw a memory diagram. Look at the memory addresses; does the numerical difference make sense to you? How would it differ if the arrays were of ints, floats, or some sort of struct?

<h2>Arrays with pointers to arrays</h2>

Copy and paste the following code into your editor, then build the code and run the result.

In class, we (briefly) examined an example of a 2D array. Unlike many other languages C/C++ does not allocate a sequential block for a multi-dimensional array. Instead, in the innermost level it has standard 1D arrays. The second dimension is a 1D array of pointers. The following code is intended to help clarify how this works.

#include &lt;iostream&gt;using namespace std; int main(){    float row_one[2] = {3.0, 1.4};    float row_two[3] = {4.5, 7.2, 5.6};    float row_three[4] = {15.6, 18.4, 22.2, 105.0};     float *different_sized[3] = {row_one, row_two, row_three}; // D1     cout &lt;&lt; “different_sized := ” &lt;&lt; endl;    cout &lt;&lt; “t” &lt;&lt; different_sized[0][0] &lt;&lt; “t” &lt;&lt; different_sized[0][1] &lt;&lt; endl;    cout &lt;&lt; “t” &lt;&lt; different_sized[1][0] &lt;&lt; “t” &lt;&lt; different_sized[1][1] &lt;&lt; “t” &lt;&lt; different_sized[1][2] &lt;&lt; endl;    cout &lt;&lt; “t” &lt;&lt; different_sized[2][0] &lt;&lt; “t” &lt;&lt; different_sized[2][1] &lt;&lt; “t” &lt;&lt; different_sized[2][2] &lt;&lt; “t” &lt;&lt; different_sized[2][3] &lt;&lt; endl &lt;&lt; endl;     float **synonym = different_sized; // that’s also a pointer to a pointer     cout &lt;&lt; “synonym := ” &lt;&lt; endl;    cout &lt;&lt; “t” &lt;&lt; synonym[0][0] &lt;&lt; “t” &lt;&lt; synonym[0][1] &lt;&lt; endl;    cout &lt;&lt; “t” &lt;&lt; synonym[1][0] &lt;&lt; “t” &lt;&lt; synonym[1][1] &lt;&lt; “t” &lt;&lt; synonym[1][2] &lt;&lt; endl;    cout &lt;&lt; “t” &lt;&lt; synonym[2][0] &lt;&lt; “t” &lt;&lt; synonym[2][1] &lt;&lt; “t” &lt;&lt; synonym[2][2] &lt;&lt; “t” &lt;&lt; synonym[2][3] &lt;&lt; endl &lt;&lt; endl;     return 0;}

An advantage of the C/C++ method is that all the elements within a certain dimension needn’t be of the same size. This is illustrated by with 1D variables named row_one, row_two, and row_three. The 2D array different_sized is declared to have 3 elements, each of which we set up to point to each row. It is useful to draw a memory diagram, the rows should just be data stored linearly, and the different_sized elements should have three arrows departing it.

We can see that indexing works left to right: different_sized[i][j] first accesses the pointer stored in the i<sup>th</sup> entry of different_sized. The pointer is followed, giving an array. In that array, the j<sup>th</sup> element is accessed. (It might be useful to follow this two-step process on your diagram.)

In the previous question, we saw that a char[] is equivalent to a char *. That is general, so for any type T we have that T[] = T*. Now consider the definition on line D1 which reads float *different_sized[3]. If we think of that as (float *) different_sized[3], then we can think of a type float **. That is why the code defining synonym works similarly. The two layers of indexing essentially become a double dereferencing operation.

You might find it helpful to print different_sized and synonym to check their memory addresses. You can also check the addresses after one level of indexing. But note that &amp;different_sized ≠ &amp;synonym, which are of type float ***.

<table>

 <tbody>

  <tr>

   <td>

    <table>

     <tbody>

      <tr>

       <td><strong>?</strong></td>

       <td>HINT</td>

      </tr>

     </tbody>

    </table></td>

   <td>

    <table>

     <tbody>

      <tr>

       <td>    This is how I did that:cout &lt;&lt; “&amp;different_sized t= ” &lt;&lt; &amp;different_sized&lt;&lt; endl;cout &lt;&lt; “&amp;synonym tt= ” &lt;&lt; &amp;synonym &lt;&lt; endl;cout &lt;&lt; “different_sized t= ” &lt;&lt; different_sized&lt;&lt; endl;cout &lt;&lt; “synonym tt= ” &lt;&lt; synonym &lt;&lt; endl;cout &lt;&lt; “different_sized[1] t= ” &lt;&lt; different_sized[1] &lt;&lt; endl;cout &lt;&lt; “synonym[1] tt= ” &lt;&lt; synonym[1] &lt;&lt; endl;</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<h2>Arrays of pointers</h2>

Your job in this question is to do a bit of detective work to understand how this program operates. Start with the following code, which should compile and run out of the box. It provides an example of how thinking of arrays as pointers can actually be rather handy.

#include &lt;iostream&gt;using namespace std; bool valid_word_char(char ch){    if ((‘A’ &lt;= ch) &amp;&amp; (‘Z’ &gt;= ch)) return true;    if ((‘a’ &lt;= ch) &amp;&amp; (‘z’ &gt;= ch)) return true;    if ((‘0’ &lt;= ch) &amp;&amp; (‘9’ &gt;= ch)) return true;    if ((‘” == ch) || (‘.’ == ch))  return true;    return false;} void print_single_word(char str[]){    int i = 0;    while (valid_word_char(str[i]))         cout &lt;&lt; str[i++];} int calc_word_length(char str[]){    int i = 0;    while (valid_word_char(str[i]))        i++;    return i;} char *increment_word(char str[]){    int i = 0;    while (valid_word_char(str[i]))         i++; // Pass through word    while ((str[i] != ‘ ’) &amp;&amp;  (!valid_word_char(str[i])))         i++; // To next word    return &amp;str[i]; // This is a pointer to the next word} int main(){    const int max = 200;    char sentence[max] = “The rhino is a homely beast,
For human eyes he’s not a feast.
Farewell, farewell, you old rhinoceros,
I’ll stare at something less prepoceros.”; // (of Ogden Nash)    char *words[max];     cout &lt;&lt; sentence &lt;&lt; endl;     int w = 0;    words[0] = sentence;    while (words[w][0] != ‘ ’) {        words[w+1] = increment_word(words[w]);        w++;    }     cout &lt;&lt; “—- Total words w = ” &lt;&lt; w &lt;&lt; ” ——-” &lt;&lt; endl;     for (int i = 0; i &lt; w; i++) {           print_single_word(words[i]);        cout &lt;&lt; endl;    }    return 0;}

If you run it, you’ll see what it does.

<ol>

 <li>To understand how it does what it’s doing, first look at the function print_single_word. I wrote that rather than just calling cout, can you see why? (If not, put cout in the code and see what it does.)</li>

 <li>Though the calc_word_length function isn’t called, looking at it can help see how the string is being processed.</li>

 <li>It can help to draw a diagram. Mine has a series of characters for sentence and then another with a series of boxes, each as the source for an arrow, for words. If you step through a short example, you’ll get the gist of how those arrows get set.</li>

</ol>

<h2>Understanding arguments passed to your program</h2>

When I run most commands from the console/shell, I typically provide them with command line arguments. Those might be files to operate on, or flags to control the program’s behavior, or other such things. Thus far, we’ve not actually been declaring our code’s main function in a way that allows processing of those arguments.

We should write our main function like this (and from now on we will):

int main(int argc, char *argv[]){  …}

or, equivalently:

int main(int argc, char **argv){  …}

The first argument, usually called argc, tells the program how many command line arguments were passed to the program on its invocation. The second argument, argv, is an array of strings containing the arguments themselves.

Write a main which prints out the arguments passed to the program. For example, when I run my program like this:

./prog apples bananas oranges

it outputs:

Argument #0 is ./progArgument #1 is appleArgument #2 is bananasArgument #3 is oranges