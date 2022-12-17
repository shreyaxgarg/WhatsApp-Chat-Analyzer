# WhatsApp-Chat-Analyzer

## Methodology
### *Data Retrieval & Preprocessing*
#### Beginning. How to export conversations from WhatsApp?


- The first step is **Data Retrieval & Preprocessing**, that is to **gather the data**. WhatsApp allows you to **export your chats** through a **.txt format**. 

- Go to the respective chat, which you want to export!

- Tap on **options**, click on **More**, and **Export Chat.**

- I will be Exporting **Without Media.**

#### NOTE:
- Without media: exports about **40k messages **
- With media: exports about *10k messages along with pictures/videos* 
- While exporting data, *avoid including media files* because if the number of media files is greater than certain figure then not all the media files are exported.
- This analyzer assumes that the chats are in 24 hour format.

### *Exploratory Data Analysis*

#### *Importing Necessary Libraries*

Libraries used :
1. **Regex (re)** to extract and manipulate strings based on specific patterns.
    - References:
        - [Regex - Python Docs](https://docs.python.org/3/library/re.html)
        - [Regex cheatsheet](https://www.rexegg.com/regex-quickstart.html)
        - [Regex Test - live](https://regexr.com/)
        - [Datetime Format](http://strftime.org/)
2. **pandas** for analysis.
3. **matlotlib** and **seaborn** for visualization.
4. **emoji** to deal with emojis.
    - References:
        - [Python Docs](https://pypi.org/project/emoji/)
        - [Emoji](https://github.com/carpedm20/emoji)
        - [EMOJI CHEAT SHEET](https://www.webfx.com/tools/emoji-cheat-sheet/)
5. **wordcloud** for the most used words.
6. **datetime** for datetime manipulation.


#### *Preparation and reading data*

Since WhatsApp texts are multi-line, you cannot just read the file line by line and get each message that you want. Instead, you need a way to identify if a line is a new message or part of an old message. You could do this use regular expressions, but I went forward with a more simple method, which splits the time formats and creates a DataFrame from a Raw .txt file.

While reading each line, I split it based on a comma and take the first item returned from the `split()` function. If the line is a new message, the first item would be a valid date, and it will be appended as a new message to the list of messages. If it’s not, the message is part of the previous message, and hence, will be appended to the end of the previous message as one continuous message.

### *Pre-Processing*

Firstly, load .txt into a DataFrame.

The dataset now contains 3 columns - DateTime String, User, and Message sent and their respective entries.

**Creating some helper columns for better analysis!**

Now that we have a clean DataFrame to work with, it’s time to perform analysis on it. 

## *Exploratory Data Analysis*

### The overall frequency of total messages on the group
![image](https://user-images.githubusercontent.com/75696894/208251511-077e9556-9856-47df-bda5-3923593aeafd.png)

![image](https://user-images.githubusercontent.com/75696894/208251523-37c73e52-1a9e-409b-b366-5fb0698e23d1.png)

![image](https://user-images.githubusercontent.com/75696894/208251537-674183e8-d869-4a7f-8e03-21c5bf0eef52.png)

### Top 10 Most Active Days
Grouping the data set by date and sorting values according to the number of messages per day.

![image](https://user-images.githubusercontent.com/75696894/208251570-68c95885-b2a1-4ef3-bc59-606e28e59406.png)
![image](https://user-images.githubusercontent.com/75696894/208251579-b6f75ba3-e026-4bfd-944d-cf57956129b7.png)
![image](https://user-images.githubusercontent.com/75696894/208251595-bd6f3059-e00e-4e85-99a3-a96a3ab95610.png)


### Top 10 active users on the group

![image](https://user-images.githubusercontent.com/75696894/208251619-3617ff82-53e9-411a-9213-2ca51be2a83e.png)

#### Now, *pre-processing the top 10 active users.*

Grouping the dataset by the user, and sorting according to the message count.

![image](https://user-images.githubusercontent.com/75696894/208251637-0f77d165-9ebe-43ab-80d4-a96b0e8bfca1.png)


And, we will be *replacing names by their initials* for **Better Visualization**, and also to maintain anonymity.

![image](https://user-images.githubusercontent.com/75696894/208251662-1f7fa8f3-d89a-4a32-97da-7c841ce45df7.png)

**First plot will be the total number of messages sent per person.** For this, a simple *seaborn countplot* will suffice.

![image](https://user-images.githubusercontent.com/75696894/208251723-608d63c6-28be-4618-b757-82c94ccc642d.png)

![image](https://user-images.githubusercontent.com/75696894/208251736-583aaa9a-7ef3-425d-9ee4-2b9592065247.png)

#### *Comparing the top 10 users!*

### The Top 10 users who send the most media

The exported chats were exported without any media files. Any message that contained media was indicated with *‘<Media Omitted> ’*. **We can use this to filter out and see who sends the most media.**

![image](https://user-images.githubusercontent.com/75696894/208251746-6405641d-0b33-4e55-81d0-7ed7d376077a.png)

### Top 10 most used Emojis

Will be using the `emoji` module, that was imported earlier.

![image](https://user-images.githubusercontent.com/75696894/208251780-b29ff79a-e570-4ba2-8965-b74f83d94e74.png)

Will create another helper column using `emoji.demojize("<emoji>")`, since **emojis will not be rendered in the plots**.

![image](https://user-images.githubusercontent.com/75696894/208251792-32d1260d-ff0b-4eec-bd10-0c2fdc355cff.png)

#### Which Emoji is the most used in the chat?

This time, it will be plotted a bit differently. Numbers will be plotted on x-direction.

![image](https://user-images.githubusercontent.com/75696894/208251824-0f0279f2-e5ec-47c6-8803-7afc3e3da896.png)

![image](https://user-images.githubusercontent.com/75696894/208251845-86d55188-4e39-4505-a0c6-4e297e7e163f.png)

- Note that it is worth anything, but “😂” beats everyone by a *huge margin!*

### Most active days, most active hours, most active months.

##### Pre-processing for most active hours.

![image](https://user-images.githubusercontent.com/75696894/208251860-558c7fa2-38b1-4265-867b-3e496b0c0cc5.png)

![image](https://user-images.githubusercontent.com/75696894/208251878-bfcaa30a-5fd6-458e-8caa-5080b79efd2f.png)

#### Pre-processing Weekdays and Months

Now, irrespective of the number of messages per day or month, we want the order to be remain the same, hence we will be using the order argument in seaborn.
![image](https://user-images.githubusercontent.com/75696894/208251897-ea19e3f8-8f34-47b8-8fc0-dbf5eb1385ef.png)
    
- Plotting multiple charts using `plt.subplots`.

![image](https://user-images.githubusercontent.com/75696894/208251911-b99fb5f3-23fb-486c-9485-d1e61ff46564.png)


#### Visualization

Now, we will be plotting ***grouped by day and respective group by month simultaneously***, to see some interesting results.

![image](https://user-images.githubusercontent.com/75696894/208251924-702b2ebc-1e33-43cf-b701-ef3172876aa6.png)

### Most Used Words in the whole chat.

I will be using the `wordcloud` module, to create a WordCloud of the **most used words**! I will be *adding some common words, to the stopwords*, such that it will not be included the WordCloud.

#### Most Used Words in the chat
![image](https://user-images.githubusercontent.com/75696894/208251939-b15c61d7-54e0-4c49-b197-43140bf1b619.png)

## Live Link
Live Link - https://whatsapp--chat--analyzer.herokuapp.com/
