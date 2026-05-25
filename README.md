# Cse_341_microprocessorproject_number_guessing_game

## Language & Tools Used
- **Programming Language**: 8086 Assembly Language
- **IDE/Emulator**: emu8086
- **Course**: CSE 341 - Microprocessor

AS A PART OF OUR MICROPROCESSOR COURSE WE HAVE CREATED THIS SMALL AND FUN PROJECT WHICH GAVE US AN HANDS ON EXPERIENCE ON HOW ASSEMBLY LANGUAGE WORKS AND FOR IDE WE USED EMULATOR HERE(emu8086).WE WERE 3 MEMBERS IN OUR GROUP AND WORKED FOR 2 FEATURES EACH. 
THE GAME FEATURES ARE GIVEN BELOW;
NUMBER GUESSING GAME

Feature 1: Secret Number Initialization The program initializes a fixed secret number which the user must guess. Since 8086 Assembly does not support built-in random functions easily, a predefined value is stored in a register and used as the target number. 

Feature 2: User Input System The system accepts user input using DOS interrupt INT 21H. The entered ASCII value is converted into a numeric value for comparison with the secret number. 

Feature 3: Hint-Based Feedback System After each guess, the program compares the input with the secret number using the CMP instruction and provides feedback: “Too High” if guess is greater “Too Low” if guess is smaller “Correct Guess” if both match This helps guide the user toward the correct answer. 

Feature 4: Attempt Limitation System The game allows a limited number of attempts (e.g., 5 tries). A counter register is used to track remaining attempts. After each wrong guess, the counter decreases, and the game ends when attempts reach zero. 

Feature 5: Score Calculation System The score is based on the number of remaining attempts when the correct answer is guessed. Fewer attempts used results in a higher score, encouraging efficient guessing.

Feature 6: Replay Option After the game ends (win or lose), the user is given the option to play again. If the user enters ‘Y’, the game restarts; otherwise, the program exits.

