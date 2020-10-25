This project provide information about refactoring and demonstrates how to refactor [SecretMEMO](https://github.com/Jomsaruj/PA4-SecretMEMO) application, using refactoring techniques.

This project is a part of [Individual Software Process 01219245](https://cpske.github.io/ISP/) course at at [Kasetsart University](https://ku.ac.th/th). 

## Table of Contents
* [Refactoring Documentation](#refactoring-documentation)
* [SecretMEMO Refactoring](#secretmemo-refactoring)

## Refactoring Documentation

#### What is refactoring

Refactoring is the process of modifying the existing code to improve it, without changing the external functionality.

#### Why refactoring

* Code is changed during development process.
* Improve the code, because your first version is not always the best.

#### When to refactoring

Signs that code needs refactoring sometime called "code smells". Here is some example of code smells.

* Duplicate code
* Long class, method or parameter list
* Divergent Change
* Feature Envy 
* Complex expression

#### Where is the refactoring goal

To answer the question "What is good code", we should backup and ask "What do customers(stakeholders) want". Here is some examples of good code.

* Easy to use - not too complex and understandable
* Does what user wants - easy to modify, changed according to feedback
* Efficient - avoid duplicate function, use caching
* No errors or defects - unit testing, CI

## SecretMEMO Refactoring

#### Problem
This is an example of duplicate code(one of the sign for refactoring). You have a temporary variable(CipherChar) that’s assigned the result of a simple expression and you place the result of an expression in a local variable for later use in your code.

#### Before refactor

```
char shiftKey(){

    char CipherChar=(char)0;
    
    if(Character.isLowerCase(originalChar)){
        CipherChar=(char)(originalChar+keys);
        if(CipherChar>'z'){
            CipherChar=(char)('a'+((CipherChar-1)-'z'));
        }
    }
    
    else if(Character.isUpperCase(originalChar)){
        CipherChar=(char)(originalChar+keys);
        if(CipherChar>'Z'){
            CipherChar=(char)('A'+((CipherChar-1)-'Z'));
        }
    }
}
```

#### Solution
You can implement 2 techniques to solve this problem. Which are [inline temp](https://refactoring.guru/inline-temp) and [replace temp with Query](https://refactoring.guru/replace-temp-with-query).

#### After refactor

```

```

#### Problem

Since method "readfile" is doing more than one thing, which are check source file status, decrypt the message and read text from file. Therefore, "readfile" method is classified as **Long method**(one of the sign for refactoring).

#### Before refactor

```
   /**
     * read a new .txt file which contain cipher text
     * @param source source file to read and decrypt
     * @param key key for decryption
     * @return boolean true if read file successful and return false if not
     */
    public String readFile(String source, String key, String strategy) throws Exception{
    
        // check file status
        File fs = new File(source);
        if(!fs.exists()||!fs.isFile()) {error("File is not a regular file"); return null;}
        if(!fs.canRead()) {error("File is unreadable"); return null;}
        
        // File decryption
        StringBuilder text = new StringBuilder();
        if(isValid(key, strategy)) {
            try {
                Cipher dec = new AlphabetShiftCipher();
                if (strategy.equals("s2")) dec = new UnicodeCipher();
                if (strategy.equals("s3")) dec = new KeyWordCipher();
                if(strategy.equals("s4")) dec = new AES();
                Scanner in = new Scanner(fs);
                String line;
                while (in.hasNext()) {
                    line = in.nextLine();
                    text.append(dec.decrypt(line, key));
                }
            } catch (IOException ioe) { error(ioe.getMessage());return null; }

        }
        return text.toString();
    }
```

#### Solution 

You can implement technique calles [Extract method](https://refactoring.guru/extract-method) and throw exception instead of return null.

```
readFile(String source) throws Exception{

    // check file status
    File fs = new File(source);

    if(!fs.exists()||!fs.isFile()) {
        error("File is not a regular file"); return null;
     }

    if(!fs.canRead()) {
        error("File is unreadable"); return null;
    }   
} 
```

## Resources

* [Refactoring Guru](https://refactoring.guru/refactoring)
* [Refactoring cpske site](https://cpske.github.io/ISP/assignment/week10/refactoring)
* [Intro to Refactoring](https://cpske.github.io/ISP/refactoring/Refactoring.pdf)



