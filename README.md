# WhatsApp-Chat-Analyzer

## Methodology
### *Data Retrieval & Preprocessing*
#### Beginning. How to export conversations from WhatsApp?

<p align="center">

</p>

- The first step is **Data Retrieval & Preprocessing**, that is to **gather the data**. WhatsApp allows you to **export your chats** through a **.txt format**. 

- Go to the respective chat, which you want to export!

- Tap on **options**, click on **More**, and **Export Chat.**

- I will be Exporting **Without Media.**

#### NOTE:
- Without media: exports about **40k messages **
- With media: exports about *10k messages along with pictures/videos* 
- While exporting data, *avoid including media files* because if the number of media files is greater than certain figure then not all the media files are exported.
- This analyzer assumes that the chats are in 24 hour format.
#### Opening this .txt file up, you get messages in a format that looks like this:

<img src="assets/extras/textfile.png" align="center">


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

<p align="center">
<img src="assets/code_snippets/carbon (1).png">
</p>

#### *Preparation and reading data*

Since WhatsApp texts are multi-line, you cannot just read the file line by line and get each message that you want. Instead, you need a way to identify if a line is a new message or part of an old message. You could do this use regular expressions, but I went forward with a more simple method, which splits the time formats and creates a DataFrame from a Raw .txt file.

While reading each line, I split it based on a comma and take the first item returned from the `split()` function. If the line is a new message, the first item would be a valid date, and it will be appended as a new message to the list of messages. If it’s not, the message is part of the previous message, and hence, will be appended to the end of the previous message as one continuous message.

<p align="center">
<img src="assets/code_snippets/carbon (0).png">
</p>

### *Pre-Processing*

Firstly, let’s load our .txt into a DataFrame.

<p align="center">
<img src="assets/code_snippets/carbon (2).png">
</p>

The dataset now contains 3 columns - DateTime String, User, and Message sent and their respective entries in 13655 rows.

**Let’s create some helper columns for better analysis!**

<p align="center">
<img src="assets/code_snippets/carbon (3).png">
</p>

Now that we have a clean DataFrame to work with, it’s time to perform analysis on it. **Let’s start Visualizing!**

<p align="center">
<img src="assets/extras/1-gina.gif" width=350>
</p>

## *Exploratory Data Analysis*

At this point, I think I’m ready to start my analysis so I will plot a simple line graph to see the frequency of messages over the months. 

### The overall frequency of total messages on the group

<p align="center">
<img src="assets/code_snippets/carbon (4).png">
</p>
<p align="center">
<img src="assets/code_snippets/carbon (5).png">
</p>
<p align="center">
<img src="assets/plots/msg_plots.png">
</p>

### Top 10 Most Active Days
Grouping the data set by date and sorting values according to the number of messages per day.


<p align="center">
<img src="assets/code_snippets/carbon (6).png">
</p>

<p align="center">
<img src="assets/code_snippets/carbon (7).png">
</p>

<p align="center">
<img src="assets/plots/top10_days.png">
</p>


### Top 10 active users on the group

Before analyzing, the top users, let’s find out how many ghosts are there in the group!


<p align="center">
<img src="assets/code_snippets/carbon (8).png">
</p>

#### Now, *pre-processing the top 10 active users.*

Grouping the dataset by the user, and sorting according to the message count.

<p align="center">
<img src="assets/code_snippets/carbon (9).png">
</p>

And, we will be *replacing names by their initials* for **Better Visualization**, and also to maintain anonymity.


<p align="center">
<img src="assets/code_snippets/carbon (10).png">
</p>


<p align="center">
<img src="assets/code_snippets/carbon (11).png">
</p>

**First plot will be the total number of messages sent per person.** For this, a simple *seaborn countplot* will suffice.

<p align="center">
<img src="assets/code_snippets/carbon (13).png">
</p>

<p align="center">
<img src="assets/plots/top10users.png">
</p>

#### *Comparing the top 10 users!*

### The Top 10 users who send the most media

The exported chats were exported without any media files. Any message that contained media was indicated with *‘<Media Omitted> ’*. **We can use this to filter out and see who sends the most media.**

<p align="center">
<img src="assets/code_snippets/carbon (18).png">
</p>

### Which user sends the most media?
Again, a simple plot using seaborn, but a different Color Palette: *CMRmap*.

<p align="center">
<img src="assets/code_snippets/carbon (19).png">
</p>

<p align="center">
<img src="assets/plots/top10media.png">
</p>

<p align="center">
<img src="https://media.giphy.com/media/l0MYt5jPR6QX5pnqM/giphy.gif" width=400>
</p>

### Top 10 most used Emojis

Will be using the `emoji` module, that was imported earlier.

<p align="center">
<img src="assets/code_snippets/carbon (20).png">
</p>

Will create another helper column using `emoji.demojize("<emoji>")`, since **emojis will not be rendered in the plots**.

<p align="center">
<img src="assets/code_snippets/carbon (21).png">
</p>

Since the emojis **will not be rendered into the plots**, here is how the *top10emojis dataset looks like*!

<p align="center">
<img src="assets/extras/top10emojis_df.png">
</p>

<p align="center">
<img src="assets/extras/3-gina-emoji.gif">
</p>

#### Which Emoji is the most used in the chat?

This time, it will be plotted a bit differently. Numbers will be plotted on x-direction.

<p align="center">
<img src="assets/code_snippets/carbon (22).png">
</p>

<p align="center">
<img src="assets/plots/top10emoji.png">
</p>

- Not that it is worth anything, but “😂” beats everyone by a *huge margin!*

### Most active days, most active hours, most active months.

Now, I will be analyzing the timely usage of the groups.

##### Pre-processing for most active hours.

<p align="center">
<img src="assets/code_snippets/carbon (23).png">
</p>

<p align="center">
<img src="assets/code_snippets/carbon (24).png">
</p>

#### Which hour of the day are most messages exchanged?

<p align="center">
<img src="assets/plots/most_active_hours.png">
</p>

Interestingly, the group is most active around **midnight**, followed by *afternoon*.

#### Pre-processing Weekdays and Months

Now, irrespective of the number of messages per day or month, we want the order to be remain the same, hence we will be using the order argument in seaborn.


<p align="center">
<img src="assets/code_snippets/carbon (25).png">
</p>

- Plotting multiple charts using `plt.subplots`.

<p align="center">
<img src="assets/code_snippets/carbon (26).png">
</p>


#### Visualization

Now, we will be plotting ***grouped by day and respective group by month simultaneously***, to see some interesting results.

<p align="center">
<img src="assets/plots/days_and_month.png">
</p>



### Most Used Words in the whole chat.

I will be using the `wordcloud` module, to create a WordCloud of the **most used words**! I will be *adding some common words, to the stopwords*, such that it will not be included the WordCloud.

<p align="center">
<img src="assets/code_snippets/carbon (28).png">
</p>

#### Most Used Words in the chat

<p align="center">
<img src="assets/plots/wordcloud.png">
</p>

### *Conclusion*

**That’s it from my end! I hope you learned and enjoyed a lot!**

<p align="center">
<img src="assets/extras/5-the-office.gif">
</p>

## Live Link
Live Link - https://whatsapp--chat--analyzer.herokuapp.com/
