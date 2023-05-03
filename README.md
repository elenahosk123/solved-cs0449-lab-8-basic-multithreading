Download Link: https://assignmentchef.com/product/solved-cs0449-lab-8-basic-multithreading
<br>
<h2>Getting started</h2>

<a href="/teaching/classes/cs0449/labs/lab8.c">Right click and download this link.</a> This is the skeleton of your lab.

What it will do

A common use for multiple threads is to <strong>do something that takes a long time on a second thread,</strong> so that the main thread can continue working.

This program supports 3 commands:

<ul>

 <li>exit – exits the main thread.</li>

 <li>status – shows how many alarm threads are pending.</li>

 <li>alarm n – sets an alarm for n seconds from now.

  <ul>

   <li>when the alarm goes off, it prints a message.</li>

  </ul></li>

</ul>

This seems like a silly program, but the way it works is <em>exactly</em> how more complex programs work. Once you learn this pattern, you can apply it to many other kinds of problems, like:

<ul>

 <li>doing complicated calculations in the background</li>

 <li>downloading files in the background</li>

 <li>periodically reminding the user of something</li>

 <li>pretty much any long-running task in a GUI application</li>

</ul>

Here is an example interaction with the program:

$ ./lab8&gt; status        0 alarm(s) pending.&gt; alarm 5&gt; alarm 8&gt; status        2 alarm(s) pending.&gt; (RING RING!) status        1 alarm(s) pending.&gt; sta(RING RING!)tus        0 alarm(s) pending.&gt; alarm 3&gt; exit        still 1 alarm(s) pending…(RING RING!) $

Above, you can see it act kind of funny… the (RING RING!) are the alarms going off. Sometimes, they go off <em>in the middle of the user typing something.</em> Yep! That’s the point! &#x1f600;

<h2>What to do</h2>

Compile it like so: gcc -lpthread -Wall -Werror –std=c99 -o lab8 lab8.c

Don’t forget the -lpthread gcc flag!

If you compile and run it right now, it won’t do much. <strong>The functions you have to implement are at the bottom.</strong> See below for details.

<ul>

 <li>Refer to the threading examples I gave in class on the examples page, particularly 21_thread_test.c.</li>

 <li>Some of the functions really are “that easy” and only take a couple lines of code.</li>

 <li>If you are only <em>reading</em> a shared global variable, technically you don’t have to lock its mutex…</li>

 <li>You can print out the a escape character to make your console go “ding” &#x1f642;</li>

</ul>

<strong>Don’t do everything at once.</strong> Implement and test each function below in the order given.

<ul>

 <li>exit_main_thread() – for the exit command

  <ul>

   <li>If there are any threads running, it should say <strong>how many there are left.</strong></li>

   <li>Then you can <em>safely</em> exit the main thread with pthread_exit(). Look it up!

    <ul>

     <li>When you use this, the other threads will keep running.</li>

     <li>If there are no other threads running, then this behaves like normal exit().</li>

    </ul></li>

  </ul></li>

 <li>show_status() – for the status command

  <ul>

   <li>This should show how many alarm threads are running.</li>

   <li>I gave you the num_threads variable to count them.</li>

  </ul></li>

 <li>change_thread_counter(int delta)

  <ul>

   <li>Read the comment.</li>

   <li>This should properly lock/unlock the mutex around its code.</li>

   <li>See 22_thread_cooperating.c in the examples.</li>

  </ul></li>

 <li>set_alarm(long duration) – for the alarm command

  <ul>

   <li>Read the comments.</li>

   <li>Write a new function to serve as the thread’s main function.

    <ul>

     <li>See 21_thread_test.c to see how we “sneak” an integer into the thread’s main function.</li>

    </ul></li>

   <li>The thread’s main function should:</li>

  </ul></li>

</ul>

<ul>

 <li style="list-style-type: none;">

  <ul>

   <li style="list-style-type: none;">

    <ol>

     <li>sleep()</li>

     <li>print a message

      <ul>

       <li>use fflush(stdout) after printing, if you don’t use 
!</li>

      </ul></li>

    </ol></li>

  </ul></li>

</ul>

decrement the number of threads using change_thread_counter