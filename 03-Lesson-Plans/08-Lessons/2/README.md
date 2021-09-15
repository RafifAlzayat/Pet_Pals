# Module 8 Class 2: Regular Expressions

## Overview

Before class, walk through this week's Challenge assignment in office hours. In class, the students will use regular expressions to search for matches in text.

## Learning Objectives

By the end of class, students will be able to:

* Use sets, wildcards, and escaping in their regular expressions
* Use special characters to grab text from the beginning and end of a text search in regular expressions
* Group their regular expressions to grab more information from their regular expression results
* Combine regular expressions and functions to make their searches more reusable.

- - -

## Instructor Notes

* The activities in this class will complement Lessons **8.3.9: Write Regular Expressions** through **8.3.12: Clean the Kaggle Data**.  The students will benefit from these activities if they‘ve progressed through these lessons, which cover the following concepts, techniques, and tasks:

 * Character types, sets, matching, and escaping with regular expressions
 * Grouping with regular expressions
 * Character counting
 * Chain methods and functions



## Slides

[ETL Day 2 slideshow](https://docs.google.com/presentation/d/16YT2jtS0Aa33-s9NZUDmaYVIxey9Lf1qa1kQTDS_X4w/edit?usp=sharing)

## Student Resources

Share the following [activity resources](https://2u-data-curriculum-team.s3.amazonaws.com/data-viz-online-lesson-plans/08-Lessons/8-2-Student_Resources.zip) with the students.


- - -

## Before Class

### 0. Office Hours

| Activity Time: 0:30       |  Elapsed Time:     -0:30  |
|---------------------------|---------------------------|

<details>
 <summary><strong>📣 Instructor Do: Challenge Instruction Walkthrough</strong></summary>

Let the students know that you’ll walk through the Challenge requirements and rubric during the first few minutes of Office Hours, while also providing helpful tips to ensure they know exactly what they need in order to be successful.

Open the Challenge in Canvas and go through the high-level instructions and requirements with your class. Be sure to check for understanding.

Scroll down to the Rubric in Canvas, go through the Mastery column with the class, and show how it maps back to the requirements for each deliverable. Be sure to check for understanding.

Review the following tips to ensure clarity on the Challenge:

For **Deliverable 1: Write an ETL Function to Read Three Data Files**, they will use their knowledge of Pandas DataFrames, functions, and reading in CSV files to load in three separate data files.

Go over the [Module 8 ETL function solution](../../../01-Assignments/08-ETL/Challenge_Solution/ETL_Deliverable1_ETL_function.ipynb) and compare it to the [ETL Challenge starter code](../../../01-Assignments/08-ETL/Resources/ETL_Deliverable1_starter_code.ipynb). Show the students the commented steps where they will be adding code to complete the challenge.


For **Deliverable 2: Extract and Transform the Wikipedia Data**, they will use their knowledge of Pandas, list comprehensions, lambda functions, try-except blocks, and regular expressions to extract, transform, and refactor the Wikipedia data so it can be merged with the Kaggle data.

Go over the [Module 8 ETL function solution](../../../01-Assignments/08-ETL/Challenge_Solution/ETL_Deliverable2_clean_wiki_movies.ipynb) and compare it to the [ETL Challenge starter code](../../../01-Assignments/08-ETL/Resources/ETL_Deliverable2_starter_code.ipynb). Show the students the commented steps where they will be adding code to complete the challenge.

For **Deliverable 3: Extract and Transform the Kaggle data**, the learners will be performing a process similar to what they did for Deliverable 2, only this time they’ll use the Kaggle data. After the cleaning process, they will merge the results from each deliverable to create the `movies_with_ratings_df` DataFrame.

For **Deliverable 4: Create the Movie Database**, the learners will be writing the final DataFrame from the previous two steps to a PostgreSQL database. They will refactor some of the code in previous steps to streamline the whole process.

Encourage your class to begin the Challenge as soon as possible, if they haven’t already, and to use the Learning Assistants channel and the remainder of Office Hours with their instructional team for help as they progress through their work. If they feel like they need context to understand documentation or instructions throughout the week, this is where they can get it.

Open the floor to discussion and be sure to answer any questions they may have about the Challenge requirements before moving onto other areas of interest.

</details>

<details>
 <summary><strong>📣  Instructor Do: Office Hours</strong></summary>

For the remaining time, remind the students that this is their time to ask questions and get assistance from their instructional staff as they’re learning new concepts and working on the Challenge assignment.

Expect that students may ask for assistance with the following:

* Challenge assignment
* Further review on a particular subject
* Debugging assistance
* Help with computer issues
* Guidance with a particular tool

</details>

- - -

## During Class

### 1. Getting Started

| Activity Time:       0:10 |  Elapsed Time:      0:10  |
|---------------------------|---------------------------|

<details>
 <summary><strong>📣 1.1 Instructor Do: Foundation Setting (0:05)</strong></summary>

* Welcome students to class.

* Direct students to post individual questions in the Zoom chat to be addressed by you and your TAs at the end of class.

* Open the slideshow and use slides 1-6 to walk through the foundation setting with your class.

* **This Week - ETL:** Talk through the key skills that students will learn this week, and let them know that they are continuing to build on their data-engineering skills.

* **Today's Objectives:** Now, outline the concepts covered in today's lesson. Remind students that they can find the relevant activity files in the “Getting Ready for Class” page in their course content.

</details>

<details>
 <summary><strong>🎉 1.2 Everyone Do: Check-In (0:05)</strong></summary>

* Ask the class the following questions and call on students for the answers:

   * **Q:** How are you feeling about your progress so far?

   * **A:** We are adding to our Python skill set. It's important to look back and see what we accomplished, and acknowledge that it's a lot! It’s also okay to feel overwhelmed as long as you don’t give up. The more you practice, the more comfortable you'll be coding.

   * **Q:** How comfortable do you feel with this topic?

   * **A:** Let's do "fist to five" together. If you are not feeling confident, hold up a fist (0). If you feel very confident, hold up an open hand (5).

</details>

<sub>[Having issues with this activity? Report a bug!](https://bit.ly/2XGYrLS)</sub>

- - -

### 2. Regex Wildcards

| Activity Time:       0:25 |  Elapsed Time:      0:35  |
|---------------------------|---------------------------|

<details>
 <summary><strong> 📣 2.1 Instructor Do: Sets, Wildcards, and Escaping (0:05)</strong></summary>

* Open [SetsWildcardsAndEscaping.ipynb](Activities/01-Ins_SetsWildcardsEscaping/Solved/SetsWildcardsAndEscaping.ipynb) in Jupyter Notebook and run through the code with the class.

 * Wildcards let us match any characters (letters, digits, whitespace, etc.). For example, the dot wildcard, `.`, allows us to match any character.

  ```python
  # find all lines of text that start with any character and then include 'ought' elsewhere in the line
  p = '.ought'
  sample_df[sample_df['text'].str.contains(p)]]
  ```

 * `\w` lets us match any letter, digit, or underscore.

 ```python
  # Use \w to find any letter, digit, or underscore followed by ought
  p = '\wought'
  sample_df[sample_df['text'].str.contains(p)]
 ```

 * If we want to match any character that isn't a letter, digit, or underscore, we use `\W`.

 ```python
  # find all lines of text with the strings 'bought', 'fought', and 'sought'
  p = '[bfs]ought'
  sample_df[sample_df['text'].str.contains(p)]]
 ```

 * We can also use square brackets when  we want to match certain characters. `[bfs]` will match any one of the following terms ending in -ought: `[bfs]ought` will match 'bought', 'fought', and 'sought'.

 * If we ever watch to match a certain character that also happens to be a regular expression, we use a delimiter to match the actual representation and not the regular expression. For example, when trying to match a period in a sentence, we would use `\.`.

* Send out the solution [SetsWildcardsAndEscaping.ipynb](Activities/01-Ins_SetsWildcardsEscaping/Solved/SetsWildcardsAndEscaping.ipynb) file for students to refer to later.

* Answer any questions before moving on to the student activity.

</details>

<details>
 <summary><strong> ✏️ 2.2 Students Do: Sets, Wildcards, and Escaping (0:15)</strong></summary>

* In this exercise, the students will use regular expression characters to find lines of text that meet specific criteria.

* Make sure the students can download the [instructions](Activities/02-Stu_SetsWildcardsEscaping/README.md), [starter code](Activities/02-Stu_SetsWildcardsEscaping/Unsolved/RegexSetsWildcardsEscaping_Unsolved.ipynb), and [data file](Activities/02-Stu_SetsWildcardsEscaping/Resources/alice.txt).

* Divide students into groups of 3 to 5. They should work on the solution by themselves but can reach out to others in their group for help.

* Let students know that they may be asked to share and walk through their work at the end of the activity.

</details>

<details>
 <summary><strong> ⭐ 2.3 Review Sets, Wildcards, and Escaping (0:05)</strong></summary>

* Once time is up, ask for volunteers to walk through their solution. Remind them that it is perfectly alright if they didn't finish the activity.

* To encourage participation, you can open the [starter code](Activities/02-Stu_SetsWildcardsEscaping/Unsolved/RegexSetsWildcardsEscaping_Unsolved.ipynb) and ask the students to help you write the code for the regex string.

* If there are no volunteers, open up [RegexSetsWildcardsEscaping.ipynb](Activities/02-Stu_SetsWildcardsEscaping/Solved/RegexSetsWildcardsEscaping.ipynb) in Jupyter Notebook and run through the code with the class line by line, making sure to hit upon the following points:

 * Character sets will match any of the characters inside the brackets. So, to match 'cat', 'hat', and 'eat', the bracket `[che]` should be used.

 * The period is a wildcard character that will match anything. To search for a period specifically, it needs to be escaped with a backslash.

 ```python
 # find all lines of text with a word that matches the form "w*ll"
 p = '\Ww.ll\W'
 alice_df[alice_df['text'].str.contains(p)]
 ```

* Send out the [RegexSetsWildcardsEscaping.ipynb](Activities/02-Stu_SetsWildcardsEscaping/Solved/RegexSetsWildcardsEscaping.ipynb) solution file for students to refer to later.

* Ask the class the following questions and call on students for answers:

 * **Q:** What change to the `[che]at\.' regular expression could be made to match 'cat', 'hat', and 'eat' anywhere in the text.

 * **A:** In order to find the text anywhere, we would drop the condition `\.`, which looks for a period.

 * **Q:** What can we do if we don't completely understand this?

 * **A:** We can refer to the lesson plan and reach out to the instructional team for help.

* Answer any questions before proceeding to the next activity.

</details>

<sub>[Having issues with this activity? Report a bug!](https://bit.ly/3ieNCda)</sub>

- - -

### 3. Regex Special Characters

| Activity Time:       0:25 |  Elapsed Time:      1:00  |
|---------------------------|---------------------------|

<details>
 <summary><strong> 📣 3.1 Instructor Do: Special Characters (0:05)</strong></summary>

* In regular expressions, there are special characters that allow more options when searching through text. Open [SpecialCharacters.ipynb](Activities/03-Ins_SpecialCharacters/Solved/SpecialCharacters.ipynb) in Jupyter notebook and go through the code with the class.

 * The questions mark, `?`, allows us to match either none or one of the preceding characters. For example, `heard?` will match both 'hear' and 'heard'.

   ```python
   # find all lines of text that contain hear or heard
   p = 'heard?'
   sherlock_df[sherlock_df['text'].str.contains(p)]
   ```

 * The asterisk, `*`, allows us to match either none, one, or more than one of the preceding characters. For example, `tell*` would match 'tel', 'tell', 'telll' and so on.

 ```python
 # find all lines of text that contain tell or tell
 p = 'tell*'
 sherlock_df[sherlock_df['text'].str.contains(p)]
 ```

 * The caret, `^`, lets us match lines that start with the preceding expression. For example, `^Watson` will match all lines that start with 'Watson'.

   ```python
   # find all lines of text that start with the string 'Watson'
   p = '^Watson'
   sherlock_df[sherlock_df['text'].str.contains(p)]
   ```

 * The dollar sign, `$`, lets us match lines that end with the preceding expressions. For example, `\.$` will match all lines that end with a period.

   ```python
   # find all lines of text that end with a period
   p = '\.$'
   sherlock_df[sherlock_df['text'].str.contains(p)]
   ```

 * The pipe, `|`, lets us put a conditional in our search to say match the term preceding it or the term after it.

 ```python
 # Use | to match lines that end with either a period or a question mark
 p = '\.$|\?$'
 sherlock_df[sherlock_df['text'].str.contains(p)]
 ```

* Send out the [SpecialCharacters.ipynb](Activities/03-Ins_SpecialCharacters/Solved/SpecialCharacters.ipynb) file for students to refer to later.

* Answer any questions before moving on to the student activity.

</details>

<details>
 <summary><strong> ✏️ 3.2 Students Do: Special Characters (0:15)</strong></summary>

* In this exercise, the students will use special characters to find lines of text that meet specific criteria.

* Make sure the students can download the [instructions](Activities/04-Stu_SpecialCharacters/README.md), [starter code](Activities/04-Stu_SpecialCharacters/Unsolved/SpecialCharacters_Unsolved.ipynb), and [data file](Activities/04-Stu_SpecialCharacters/Resources/alice.txt).

* Divide students into groups of 3 to 5. They should work on the solution by themselves but can reach out to others in their group for help.

* Let students know that they may be asked to share and walk through their work at the end of the activity.

</details>

<details>
 <summary><strong> ⭐ 3.3 Review: Special Characters (0:05)</strong></summary>

 * Once time is up, ask for volunteers to walk through their solution. Remind them that it is perfectly alright if they didn't finish the activity.

* To encourage participation, you can open the [starter code](Activities/04-Stu_SpecialCharacters/Unsolved/SpecialCharacters_Unsolved.ipynb) and ask the students to help you write the code for the regex string using special characters.

* If there are no volunteers, open up [SpecialCharacters.ipynb](Activities/04-Stu_SpecialCharacters/Solved/SpecialCharacters.ipynb) in Jupyter Notebook and run through the code with the class line by line, making sure to hit upon the following points.

 * The caret (`^`) is a zero-length match for the beginning of the string. Here, the string is an entire line of text, so the caret matches the beginning of the entire line.

 * Similarly, the dollar sign (`$`) matches the end of the entire line.

 * The pipe character (`|`) is used like a logical OR to match one expression or another.

</details>

<sub>[Having issues with this activity? Report a bug!](https://bit.ly/38Jrvsg)</sub>

- - -

### 4. Regex Grouping

| Activity Time:       0:30 |  Elapsed Time:      1:30  |
|---------------------------|---------------------------|

<details>
 <summary><strong> 📣 4.1 Instructor Do: Regex Groups (0:05)</strong></summary>

* For this next activity, students will learn about extracting information from matches by using regex groups.

* Let students know that we can further extract information from matches by using parentheses. Matches inside parentheses are considered a group.

* Open [Groups.ipynb](Activities/05-Ins_Groups/Solved/Groups.ipynb) in Jupyter Notebook and go over the code with the class.

 * `\s` allows us to match whitespace, and putting a number inside curly brackets allows us to match a specific number of occurrences.

 ```python
 # The \s lets us match any white space and {} lets us match exact ranges or characters
 # Here we find Holmes followed by a space then a 6 letter word then another space.
 p = 'Holmes\s\w{6}\s'
 sherlock_df[sherlock_df['text'].str.contains(p)]
 ```

 * We can set a range of occurrences of a character as well.

 ```python
 # Adding a second number to the {} lets us find a range
 # Here it matches all words of length 4 to 6
 p = 'Holmes\s\w{4,6}\s'
 sherlock_df[sherlock_df['text'].str.contains(p)]
 ```

 * To match all uses of the word 'Holmes' followed by any text that is 6 letters or longer, we would use the regex `'Holmes \s*\w{6,}`.

 * Say we now want to break this down further by counting the number of times each word after Holmes appears. We create two groups: the first group will match the first appearance of the word 'Holmes', and the second group will match zero or more space characters followed by any word of 6 or more characters.

 * Then, we display the groups by using the `extractall()` method.

 ```python
 # Create capture groups
 p = '(Holmes)(\s*\w{6,})'

 # Extract the groups
 holmes_df = sherlock_df['text'].str.extractall(p)
 holmes_df
 ```

 * We see from the resulting DataFrame that there are two groups. Selecting the second group and using the `value_counts()` method will allow us to display the amount of times each word was used.

 ```python
 # Grab the value counts for each word in the second capture group
 holmes_df[1].value_counts()
 ```

 * We can extract all wrod that start with 'z'. In order to pull all words that begin with either a lowercase or uppercase 'z', the parameter `flags=re.I` is passed in. This tells the regex to ignore the case when matching.

  ```python
  # extract all words that start with 'z' or 'Z'
  p = '\W(z\w*)\W'
  sherlock_df['text'].str.extractall(p, flags=re.I).dropna()
  ```

  * We can also tell our regex to not match a group with `(?:)`. The following regex will match the word 'Holmes' but not group it, but will group the following 6 letter or longer word after.

  ```python
  # Find all the words 6 letters or longer that come after "Holmes" in the text and count their frequency
  p = '(?:Holmes)(\s*\w{6,})'
  sherlock_df['text'].str.extractall(p, flags=re.I)[0].value_counts()
  ```

  * We can also not match certain things. The `(?!)` pattern is a negative look ahead, which says don't match whats follows. In `(?!.+ly)` it says to not match any character, `.`, of one or more, `+`, that ends with `ly`.

  ```python
  # Find all the words 6 letters or longer that don't end in "ly" that come after "Holmes"
  # in the text and count their frequency
  p = '(?:holmes)(?!.+ly)(\s*\w{6,})'
  sherlock_df['text'].str.extractall(p, flags=re.I)[0].value_counts()
  ```

* Answer any questions before moving on to the student activity.

</details>

<details>
 <summary><strong> ✏️ 4.2 Partners Do: Groups (0:20)</strong></summary>

* In this exercise, students will use capture groups to further refine regular expression matches.

* Make sure the students can download and open the [instructions](Activities/06-Stu_Groups/README.md), the [starter code](Activities/06-Stu_Groups/Unsolved/RegexGroups_Unsolved.ipynb), and the [data file](Activities/06-Stu_Groups/Resources/alice.txt).

* Go over the instructions with the students and answer any questions before breaking the students out in groups.

* Divide students into groups of 3 to 5. They should work on the solution by themselves but can reach out to others in their group for help.

* Let students know that they may be asked to share and walk through their work at the end of the activity.

</details>

<details>
 <summary><strong> ⭐ 4.3 Review: Regex Groups (0:05)</strong></summary>

* Once time is up, ask for volunteers to walk through their solution. Remind them that it is perfectly alright if they didn't finish the activity.

* To encourage participation, you can open the [starter code](Activities/06-Stu_Groups/Unsolved/RegexGroups_Unsolved.ipynb) and ask the students to help you write the code for regex groups.

* If there are no volunteers, open up [RegexGroups.ipynb](Activities/06-Stu_Groups/Solved/RegexGroups.ipynb) in Jupyter Notebook and run through the code with the class line by line, making sure to hit upon the following points.

 * The default group in regex is a “capture” group. For matching purposes, it doesn’t make a big difference, but when using regex for extracting substrings, only the substrings in capture groups get extracted.

 * Non-capture groups are used to find context for substrings to be extracted. For example, finding words that come after “Alice” requires a non-capturing group to look for “Alice”.

 * Negative lookahead groups are non-capturing groups that also help find the context for other substrings. For example, to find words that come after “Alice” that are not adverbs (words ending in “ly”), we need to add a negative lookahead group that looks ahead of “Alice” and negates the match if it finds a word ending in “ly”.

 * Answer any questions students might have.

</details>

<sub>[Having issues with this activity? Report a bug!](https://bit.ly/2KguLC7)</sub>

- - -

### 5. Regex with Functions

| Activity Time:       0:25 |  Elapsed Time:      1:55  |
|---------------------------|---------------------------|

<details>
 <summary><strong> ✏️ 5.1 Everyone Do: Functional Regex (0:25)</strong></summary>

* In this exercise, students will combine what they have learned about regular expressions and functions to allow them to reuse the search across two different text files.

* Make sure students can download and open the [instructions](Activities/07-Evr_FunctionalRegex/README.md), the [starter code](Activities/07-Evr_FunctionalRegex/Unsolved/Functional_Regex.ipynb), [alice text file](Activities/07-Evr_FunctionalRegex/Resources/alice.txt) and [sherlock text file](Activities/07-Evr_FunctionalRegex/Resources/sherlock.txt).

* Go over the instructions with the students, then let the students work on their solution for 5 to 7 minutes.

* When time is up, open the [starter code](Activities/07-Evr_FunctionalRegex/Unsolved/Functional_Regex.ipynb) file and ask students to help you write.

* If there are no volunteers, open up the [solved file](Activities/07-Evr_FunctionalRegex/Solved/Functional_regex.ipynb) and go over the solution with the class, answering whatever questions students may have.

* Key points to cover during this discussion:

 * The first function needs to work on different text files, so one argument for that is passed in.

 * The second function loads the file into a DataFrame and uses the regex expression `\?$` to find lines that end with a question mark.

 * A length of the results is returned.

 ```python
 # Create a function to find how many questions are asked in a text
 def question_count(text_file):
    # Load text file into a DataFrame
    text_df = pd.read_csv(text_file, sep='\n', header=None)
    text_df.columns = ['text']

    # Create regex to find lines that end in a ?
    p = '\?$'
    question_df = text_df[text_df['text'].str.contains(p)]

    return len(question_df)
 ```

 * The second function takes in two arguments, one for the text file and another for the character to search.

 * We load in the file and create a regex string to search for that character.

 * We extract the groups and return the value counts.

 ```python
 def word_count(text_file, character):

    # Load text file into a DataFrame
    text_df = pd.read_csv(text_file, sep='\n', header=None)
    text_df.columns = ['text']

    # Create regex string for the character
    p = '('+ character + ')' + '(\s*\w{6,})'

    # Group the character and the words
    word_groups_df = text_df['text'].str.extractall(p)

    # Return
    return word_groups_df[1].value_counts()
 ```

* Send out the [solution](Activities/07-Evr_FunctionalRegex/Solved/Functional_regex.ipynb) for the students to refer to later.

* Answer any questions before ending class.

</details>

<sub>[Having issues with this activity? Report a bug!](https://bit.ly/38K1xVG)</sub>

- - -

### 6. Ending Class

| Activity Time:       0:05 |  Elapsed Time:      2:00  |
|---------------------------|---------------------------|

<details>
 <summary><strong>📣  6.1 Instructor Do: Review </strong></summary>

* Before ending class, review the skills that were covered today and mention where in the module these skills are used.
 * Using regular expressions with sets, wildcards, and escaping was covered in **Lesson 8.3.9**.
 * Using regular expressions with special characters was covered in **Lesson 8.3.9**.
 * Using regular expressions with grouping was covered in **Lesson 8.3.9**.
 * Combining the use of regular expressions and functions was covered in **Lesson 8.3.10**.

* Answer any questions the students may have.

</details>

<sub>[Having issues with this section? Report a bug!](https://bit.ly/2XJDjnS)</sub>

- - -

© 2021 Trilogy Education Services, LLC, a 2U, Inc. brand.  Confidential and Proprietary.  All Rights Reserved.
