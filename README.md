java c
Unix  C Programming (COMP1000)
Semester 2, 2024
Assignment
Due: Sunday, 20 October 2024, 11:59 PM (GMT+8)
Weight: 30% of the unit
Your task for this assignment is to design, code (in C89 version of C) and test a program.  In summary, your program will:
•  Able to create dynamically-allocated 2D char array to make a simple ASCII-based game.
•  Receive user input to control the game.
•  Utilize pre-written random number generator and terminal behaviour configuration for the game.
•  Write a proper makefile.  Without the makefile, we will assume it cannot be compiled and it will negatively affect your mark.
•  Extract information from a text file to initialize the game.
•  Utilise linkedlist data structure to keep track the game progress, allowing the player to undo the steps taken.
1 Code Design
You must comment your code sufficiently in a way that we understand the overall design of your program. Remember that you cannot use “//” to comment since it is C99-specific syntax.
You can only use “/*………*/” syntax.Your code should be structured in a way that each file has a clear scope and goal. For example, “main.c” should only contain a main function, “game.c” should contain functions that handle and control the game features, and so on. Basically, functions should be located on reasonably appropriate files and you will link them when you write the makefile. DO NOT put everything together into one single source code file. Make sure you use the header files and header guard correctly. NEVER #include .c files directly, instead #include .h files only.Make sure you free all the memories allocated by themalloc() function. Use valgrind to detect any memory leak and fix them. Memory leaks might not break your program, but penalty will still be applied if there is any. If you do not use malloc at all, you will lose marks.Please be aware of our coding standard (can be found on Blackboard and the marking criterias) Violation of the coding standard will result in penalty on the assignment. Keep in mind that it is possible to lose significant marks with a fully-working program that is written messily, has no clear structure, full of memory leaks, and violates many of the coding standards. The purpose of this unit is to teach you a good habit of programming.
2 Academic IntegrityThis is an assessable task, and as such there are strict rules.  You must not ask for or accept help from anyone else on completing the tasks. You must not show your work at any time to another student enrolled in this unit who might gain unfair advantage from it.  These things are considered plagiarism or collusion.  To be on the safe side, never release your code to the public.Do NOT just copy some C source codes from internet resources (including AI tools such as ChatGPT). If you get caught, it will be considered plagiarism.  If you put the reference, then that part of the code will not be marked since it is not your work.Staff can provide assistance in helping you understand the unit material in general, but nobody is allowed to help you solve the specific problems posed in this assignment.  The purpose of the assignment is for you to solve them on your own, so that you learn from it.  Please see Curtin’s Academic Integrity website for information on academic misconduct (which includes plagiarism and collusion).The unit coordinator may require you to attend quick interview to answer questions about any piece of written work submitted in this unit. Your response(s) maybe referred to as evidence in an Academic Misconduct inquiry. In addition, your assignment submission may be analysed by systems to detect plagiarism and/or collusion.


3 Task Details
3.1 Quick PreviewFirst of all, please watch the supplementary videos on the Assignment link on Blackboard. These videos demonstrates what we expect your program should do.  You will implement a simple game that you can play on Linux terminal. Further details on the specification will be explained on the oncoming sections.
3.2 Command Line Arguments
Please ensure that the executable is called “treasure” . Your executable should accept only one command-line parameter/argument:
./treasure is the text file name containing the information to initiate your game.  It is your responsibility to check whether the file can be opened properly. You can assume the content of the file is valid to initiate the game.
If the amount of the arguments are too many or too few, your program should print a message on the terminal and end the program accordingly (do NOT use exit() function).
Note: Remember, the name of the executable is stored in variable argv[0].


3.3 Main InterfaceOnce you run the program, it should clear the terminal screen (watch the video about the method to clear the screen) and print the 2D map along with the Walls, Player, Treasure, Lantern, and Snake:
Figure 1: Main visual interface of the game.You can type the command to move the Player.  The Snake will move one grid randomly on 8 directions for every Player movement (See Section 3.6.3).  Every time the user inputs the command, the program should update the map accordingly and refresh the screen (clear the screen and reprint the map). Please refer to Section 3.6.1for more detail on the user input.
3.4 Character on the Map
You have to use the correct characters on the game interface:
•  Use char ’*’ for the map border.
•  Use char ’O’ for the Wall.
•  Use char ’@’ for the Lantern.
•  Use char ’P’ for the Player.
•  Use char ’~’ for the Snake.
•  Use char ’$’ for the Treasure.
3.5 File I/O
Please watch the supplementary video about the File I/O. The text file will look like this: 
(a) content of map text file                      (b) corresponding interface
Figure 2: This is an example of the map text file and the game interface created from it.The two integers on the first line are the playable row and column map size respectively. These mapsizes do not include the border surrounding the map. The integer on second line indicates the amount of the Walls in the game.  Feel free to use this information to allocate sufficient memory to store Wall location. Afterwards, the remaining lines are the content of the 2D map array. You can assume the text file contains valid information with valid datatype. Each integer represents:
•  0 represents ’ ’ (An empty space)
•  1 represents ’O’ (The Wall)
•  2 represents ’@’ (The Lantern)
•  3 represents ’P’ (The Player)
•  4 represents ’~’ (The Snake)
•  5 represents ’$’ (The Treasure)The integers on first line will match the amount of rows and columns of the map.  Similiarly, the integer on second line will match the correct amount of Walls on the map. Your program should be able to read it correctly to initialize the game interface.Note: Keep in mind that we will use our own map text file when we mark your assign- ment. So,please make sure your file I/O implementation works with various map files. Feel free to create your own map file.
3.6 Gameplay
Please watch the supplementary videos for visual demonstration.  The main gameplay is to move the Player to reach the Treasure while avoiding being eaten by the Snake.
3.6.1 User Input
The user only needs to type 1 character for each command (lower case). Here is the list of the possible commands:
• ‘w’ moves the player one block above.
• ‘s’ moves the player one block below.
• ‘a’ moves the player one block left.
• ‘d’ moves the player one block right.
• ‘u’ undoes the movement of the Player and Snake. This includes returning the Lantern’s original position.
•  Any other character should be ignored, and nothing changes on the map.  Feel free to print a warning message such as ”invalid key” .Your program should be able to receive each input without pressing ”enter” key everytime. Please refer to the supplementary video for a sample code to disable the Echo and Canonical feature on Linux terminal temporarily. If your terminal is stuck on this mode, you only need to re-open a new terminal.
Note: Every valid action should update the 2D map array, clear the terminal screen and reprint the map.
3.6.2 Winning/Losing the GameThe game will continue playing until either the Player manages to reach the Treasure (win- ning) OR the Snake manages to eat the Player (losing). You can print message such as ”You win!” or ”You lose!” when the game ended.  Please do NOT use exit() or break to end the program. You must free all the malloc’ed memories and ensure the program reaches the end of the main function properly.
3.6.3    Snake Movements
Whenever the Player makes a valid movement, the Snake will also move for one grid on the map.  However, the Snake’s movement is randomized.  You have to use the random number generator function for the Snake to decide 1 out of 8 possible directions. If the Snake is at the edge of the map OR near the Walls, then some movements will be restricted. (Please be careful here. Moving the Snake outside the map might crash your program due to unauthorized out of bound memory access)
Figure 3: The Snake can move to 1 out of 8 directions.Figure 4: If the Snake is at the edge/border of the map OR near the Walls, then the choice of movements will be reduced. Please ensure that the Snake does not move out of bound of the map.
However, if the Player moves to a grid on which it is exactly ONE grid away from the Snake, then the Snake will just move to eat the Player without any random movement. 
(a) Player moves closer to the Snake.                   (b) Snake moves to eat the Player.
Figure 5: The Snake will move in the direction of the Player only if the Player is one grid away from the Snake. (after Player movement)
3.7 DARK mode with Makefile Conditional CompilationYou need to implement an additional feature that allows for higher game difficulty. This DARK mode is activated via Conditional Compilation with Makefile (e.g when you execute ”make DARK=1”). When the keyword DARK is defined when compiling, the Player will have a lim- ited visibility range.By default, th代 写Unix & C Programming (COMP1000) Semester 2, 2024R
代做程序编程语言e Player can only see as far as 3 blocks distance away from the Player’s position. Any object that is not within the visibility range of the Player will be invisible (print out empty space ’ ’ instead).  If there is an empty space (no object) within the Player’s visibility range, print out char ’.’ instead. (This will make it easier to see the Player’s visibility boundary)You will use Manhattan Distance for the Player’s visibility range (also known as Taxicab Dis- tance OR City Block Distance). Manhattan Distance refers to a method to measure the distance between two points (within 2D grid-based array in this context). Instead of drawing a straight line between both points and measure it, Manhattan Distance sums the absolute distance of both points in vertical direction and horizontal direction.  For example, if we have the Player located at row 1 and column 5, and the Treasure is located at row 7 and column 2, then:
Manhattan Distance = |Player row − Treasure row| + |Player col − Treasure col|
= |1 − 7| + |5 − 2|
= 6 + 3
= 9(blocks distance)
When the Player reaches the Lantern ’@’, the Lantern will dissappear and the vision distance will be increased to 6 blocks. This effect will last until the game ends. 
(a) 3 blocks visibility range.                             (b) 6 blocks visibility range.
Figure 6:  In DARK mode, the Player only has visibility range of 3 blocks.  After obtaining Lantern, the range increases to 6 blocks.
3.8 Struct and Generic Linked List to implement UNDO featureIn this section, you will need to write a generic linked list with appropriate struct to implement UNDO feature. This feature is triggered when the user enters the key ’u’. Everytime the key is pressed, the player and the snake will move back to their previous position. The player can use the undo feature at any point of the game as long as the game is not over. It should be possible to keep undo-ing up to the initial state of the game.Please refer to supplementary video for some advices regarding this implementation. The sum- mary is that you can use the linkedlist node to store any information that you think are useful. For example, you can store the previous player ’P’ location and the snake ’~’ location (This is only one example, you can store more information if you need it). You will need to create a new linkedlist node everytime the player moves (with malloc()) and store it in the linkedlist. When the player presses ’u’, your program should be able to retrieve the mostrecent node and update the map to the previous state. If game state is back to the initial configuration (when you just started the game), pressing ’u’ should NOT do anything.  One possible way to confirm this is by checking if the linkedlist is empty.  (Once again, this depends on your implementation. Implement what you need to achieve this task.)Note: Remember, if you UNDO the game back until the point the Player reaches the Lantern, then the Lantern will be put back on the ground, and the vision distance is reduced back to 3 again.Note: Please do not just copy the whole linkedlist code from someone else because it might lead to collusion academic misconduct.
3.9 Makefile
You should manually write the makefile according to the format explained in the lecture and practical. It should include the Make variables, appropriate flags, appropriate targets, correct prerequisites, Conditional Compilation, and clean rule. We will compile your program through the makefile. If the makefile is missing, we will assume the program cannot be compiled, and you will lose marks. DO NOT use the makefile that is auto-generated by the VScode. Write it yourself.
3.10 Assumptions
For simplification purpose, these are assumptions you can use for this assignments:
•  The content of the text files are valid. (If there is NO error when opening it)
•  The size of the map in the text file is at least 10 rows AND at least 10 columns.
•  All game objects exist.  There will be ONLY one Player, ONLY one Snake, ONLY one Treasure, and ONLY one Lantern. The Wall is AT LEAST one.
•  There will be a winning path from the Player to the Treasure.  Similiarly, there is also a path from the Snake to the Player.  It means each map will always have a possibility of winning and losing.
•  You can still win the game even without grabbing the Lantern.  Lantern just makes it easier when DARK mode is activated.
•  Snake cannot step on the same grid as the Treasure and Lantern.
•  Both Player and Snake cannot step inside the Wall.
•  The size of the map will be reasonable to be displayed on terminal. For example, we will not test the map with gigantic size such as 300 x 500.
•  You only need to handle lowercase inputs (’w’, ’s’, ’a’, ’d’, ’u’). The other keys should be ignored (feel free to add warning message).
Note: When you create your own custom maps for testing, please make sure to follow these assumptions.
4 Marking Criteria
This is the marking distribution for your guidance:
•  Properly structured makefile according to the lecture and practical content (5 marks)
•  Program can be compiled with the makefile and executed successfully without immedi- ate crashing and showing reasonable output (5 marks)
•  Usage of header guards and reasonable in-code commenting (2 marks)
•  The whole program is readable and has reasonable framework.  Multiple files are uti- lized with reasonable category. If you only have one c file for the whole assignment (not
counting terminal.c and random.c), you will get zero mark on this category. (3 marks)
•  Avoiding bad coding standard. (5 marks) Please refer to the coding standard on Black- board. Some of the most common mistakes are:
– Using global variables
– Calling exit() function to end the program abruptly
– Using “break’‘ NOT on the switch case statement
– Using “continue’‘
– Using goto
– Having multiple returns on a single function
– #include the .c files directly instead of the .h files
•  No memory leaks (10 marks) Please use valgrind command to check for any memory leak.  If your program is very sparse OR does not use any malloc(), you will get zero
mark on this category.
• FUNCTIONALITIES:
– Correct command line arguments verification with proper response. This includes detecting the success/failure of opening the text file. (5 marks)
– Proper memory allocation for the 2D map array. (5 marks)
– Able to clear the screen and re-print the map on every action. (5 marks)
– Able to move the Player with the keyboard input from the user without pressing ’Enter’ key. (and cannot go through Wall) (5 marks)
– The Snake moves randomly 1 out of 8 directions after every Player movement cor- rectly. (and not hitting Border and Walls) (5 marks)
– When the Player moves too close to the Snake, the Snake immediately eats the player. (5 marks)
– Winning when the Player reaches the Treasure AND Losing when the Snake eats the Player. (5 marks)
– File IO is done successfully to retrieve all game information from the map text file. (10 marks)
– Able to UNDO the game utilizing linkedlist. (15 marks)  (If linkedlist is NOT generic, only 10 marks max)
– DARK mode is implemented correctly in makefile and the game. (including lantern usage to increase visibility range) (10 marks)
Note: Remember, if your program fails to demonstrate that the functionality works, then the mark for that functionality will be capped at 50%. If your program does not compile at all OR just crashed immediately upon running it, then the mark for all the function- alities will be capped at 50% (For this assignment, it means maximum of 35 out of 70 marks on functionalities). You then need describe your code clearly on the short report on the next section.
5    Short ReportIf your program is incomplete/imperfect, please write a short report explaining what you have done on for each functionality.  Inform. us which function on which file that is related to that functionality.  This report is NOT marked, but it will help us a lot to mark your assignment. Poorly-written report will not help us to understand your code.  Please ensure your report reflects your work correctly (no exaggeration). Dishonest report will lead to academic miscon- duct.
6 Final Check and SubmissionAfter you complete your assignment, please make sure it can be compiled and run on our Linux lab environment. If you do your assignment on other environments (e.g on Windows operating system), then it is your responsibility to ensure that it works on the lab environment.  In the case of incompatibility, it will be considered “not working’‘ and some penalties will apply. You have to submit a single zip file containing ALL the files in this list:
• Declaration of Originality Form. – Please fill this form. digitally and submit it. You will get zero mark if you forget to submit this form.
• Your essential assignment files – Submit all the .c  .h files and your makefile.  Please do not submit the executable and object (.o) files, as we will re-compile them anyway. Do not create any sub-directory within the directory. Every file should exist within the same directory.
• Brief Report (if applicable) - If you want to write the report about what you have done on the assignment, please save it as a PDF or TXT file.
Note: Please compress all the necessary files into a SINGLE zip file. So, your submission should only has one zip file.  If you do not compress the files, we reserve the right to refuse your submission.
The name of the zipped file should be in the format of:
___Assignment.zip Example: Antoni_12345678_Michael-Bay_Assignment.zipIf you forget your tutor’s name, please put the lecturer’s name.  Please make sure your sub- mission is complete and not corrupted. You can re-download the submission and check if you can compile and run the program again. Corrupted submission will receive instant zero. Late submission will receive zero mark. You can submit it multiple times, but only your latest sub- mission will be marked.







         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
