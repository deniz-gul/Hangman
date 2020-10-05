# Hangman
Hangman Game (MIT Assignment)
# Problem Set 2, hangman.py
# Name: Deniz G.


import random
import string

WORDLIST_FILENAME = "words.txt"


def load_words():
    """
    Returns a list of valid words. Words are strings of lowercase letters.
    
    Depending on the size of the word list, this function may
    take a while to finish.
    """
    print("Loading word list from file...")
    # inFile: file
    inFile = open(WORDLIST_FILENAME, 'r')
    # line: string
    line = inFile.readline()
    # wordlist: list of strings
    wordlist = line.split()
    print(len(wordlist), "words loaded.")
    return wordlist



def choose_word(wordlist):
    """
    wordlist (list): list of words (strings)
    
    Returns a word from wordlist at random
    """
    return random.choice(wordlist)

# end of helper code

# -----------------------------------

# Load the list of words into the variable wordlist
# so that it can be accessed from anywhere in the program
wordlist = load_words()


def is_word_guessed(secret_word, letters_guessed):
    '''
    secret_word: string, the word the user is guessing; assumes all letters are
      lowercase
    letters_guessed: list (of letters), which letters have been guessed so far;
      assumes that all letters are lowercase
    returns: boolean, True if all the letters of secret_word are in letters_guessed;
      False otherwise
    '''
    for i in secret_word:
        if (i in letters_guessed) == False:
            guessed = False
            break
        
        else :
            guessed = True
    
    return guessed

def get_guessed_word(secret_word, letters_guessed):
    '''
    secret_word: string, the word the user is guessing
    letters_guessed: list (of letters), which letters have been guessed so far
    returns: string, comprised of letters, underscores (_), and spaces that represents
             which letters in secret_word have been guessed so far.
    '''
    matching_letters = [] 
    guessed_string = str()  
    
    
    for i in letters_guessed:
        #fills the matching_letters list with letters guessed which match with the secret_word's letters
        if i in secret_word:
            matching_letters.append(i)
    
    for i in secret_word:
        #shows letters that match in order
        if i in matching_letters:   
            guessed_string += i
        #creates underscores for guessed letters which do not match with the secret_word
        else:
            guessed_string += "_ "
    
    return guessed_string


def get_available_letters(letters_guessed):
    '''
    letters_guessed: list (of letters), which letters have been guessed so far
    returns: string (of letters), comprised of letters that represents which letters have not
      yet been guessed.
    '''
    letters = string.ascii_lowercase
    available_letters = str()
    
    #checks if a letter in letters string has been guessed, if not adds it to available_letters string
    for i in letters:
        if i not in letters_guessed:
            available_letters += i
    return available_letters
    
    

def hangman(secret_word):
    
    print('Welcome to the game Hangman!')
    len_word = len(secret_word)
    print( 'I am thinking of a word that is', len_word, 'letters long.' )
    print('-------------')
    
    guesses_left = 6 
    warnings_left = 3
    letters_guessed = []
    vowels = ['a', 'e', 'i', 'o', 'u']
    num_guesses = 0
    
   
    while guesses_left > 0:
        print( 'You have', guesses_left, 'guesses left.' )
        print( 'Available letters:', str((get_available_letters(letters_guessed))))

        input_letter = str(input('Please guess a letter:')).lower()

        available_letters = get_available_letters(letters_guessed)
        
        if input_letter in available_letters :
            if input_letter in list(secret_word):
                letters_guessed.append(input_letter)
                print('Good guess: ', get_guessed_word(secret_word, letters_guessed))
            else :
                letters_guessed.append(input_letter)
                print('Oops! That letter is not in my word: ', \
                    get_guessed_word(secret_word, letters_guessed))
                if input_letter in vowels :
                    guesses_left -= 2
                else :
                    guesses_left -= 1
        #checking if letter is guessed before            
        elif input_letter in letters_guessed :
            letters_guessed.append(input_letter)
            if warnings_left > 0 :
                warnings_left -= 1
                print("Oops! You've already guessed that letter. You now have", warnings_left,\
                "warnings:", get_guessed_word(secret_word, letters_guessed))
            #if there is no warnings left
            else :
                guesses_left -=1
                print("Oops! You've already guessed that letter. You now have", guesses_left, \
                "gueses:", get_guessed_word(secret_word, letters_guessed)) 
        #if input_letter is not in available_letters list and is not guessed before
        #if input is not a letter
        else :
            letters_guessed.append(input_letter)
            if warnings_left > 0 :
                warnings_left -= 1
                print('Oops! That is not a valid letter. You have', warnings_left, \
                'warnings left: ', get_guessed_word(secret_word, letters_guessed))
            else :
                guesses_left -= 1
                print('Oops! That is not a valid letter. You have', guesses_left, \
                'guesses left: ', get_guessed_word(secret_word, letters_guessed))

        num_guesses += 1
        comparison = len((get_guessed_word(secret_word, letters_guessed))) - \
                    (get_guessed_word(secret_word, letters_guessed)).count("_") - \
                    (get_guessed_word(secret_word, letters_guessed)).count(" ")

        if comparison == len_word :
            print('Congratulations, you won!')
            print('Your total score for this game is: ', num_guesses)
            break

        print('----------')
    
    if guesses_left == 0 :
        print('Sorry, you ran out of guesses. The word was else.')

    return


secret_word = choose_word(wordlist)
hangman(secret_word)
