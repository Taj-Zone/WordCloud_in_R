# WordCloud_in_R
Generating Word Cloud in R Programming

Word Cloud is a data visualization technique used for representing text data in which the size of each word indicates its frequency or importance. Significant textual data points can be highlighted using a word cloud. Word clouds are widely used for analyzing data from social network websites.

Why Word Cloud?
The reasons one should use word clouds to present the text data are:

Word clouds add simplicity and clarity. The most used keywords stand out better in a word cloud
Word clouds are a potent communication tool. They are easy to understand, to be shared, and are impactful.
Word clouds are visually engaging than a table data.
Implementation in R
Here are steps to create a word cloud in R Programming.

Step 1: Create a Text File
Copy and paste the text in a plain text file (e.g:file.txt) and save the file.

Step 2: Install and Load the Required Packages
# install the required packages
install.packages("tm")           # for text mining
install.packages("SnowballC")    # for text stemming
install.packages("wordcloud")    # word-cloud generator
install.packages("RColorBrewer") # color palettes
  
# load the packages
library("tm")
library("SnowballC")
library("wordcloud")
library("RColorBrewer")
Step 3: Text Mining
Load the Text:
The text is loaded using Corpus() function from text mining(tm) package. Corpus is a list of a document.

Start by importing text file created in step 1:
To import the file saved locally in your computer, type the following R code. You will be asked to choose the text file interactively.
text = readLines(file.choose())
Load the data as a corpus:
# VectorSource() function 
# creates a corpus of 
# character vectors
docs = Corpus(VectorSource(text))   
Text transformation:
Transformation is performed using tm_map() function to replace, for example, special characters from the text like “@”, “#”, “/”.
toSpace = content_transformer
             (function (x, pattern)
              gsub(pattern, " ", x))
docs1 = tm_map(docs, toSpace, "/")
docs1 = tm_map(docs, toSpace, "@")
docs1 = tm_map(docs, toSpace, "#")
Cleaning the Text:
The tm_map() function is used to remove unnecessary white space, to convert the text to lower case, to remove common stopwords. Numbers can be removed using removeNumbers.
# Convert the text to lower case
docs1 = tm_map(docs1, 
        content_transformer(tolower))
  
# Remove numbers
docs1 = tm_map(docs1, removeNumbers)
  
# Remove white spaces
docs1 = tm_map(docs1, stripWhitespace)
Step 4: Build a term-document Matrix
Document matrix is a table containing the frequency of the words. Column names are words and row names are documents. The function TermDocumentMatrix() from text mining package can be used as follows.

dtm = TermDocumentMatrix(docs)
m = as.matrix(dtm)
v = sort(rowSums(m), decreasing = TRUE)
d = data.frame(word = names(v), freq = v)
head(d, 10)
Step 5: Generate the Word Cloud
The importance of words can be illustrated as a word cloud as follows.

wordcloud(words = d$word, 
          freq = d$freq,
          min.freq = 1, 
          max.words = 200,
          random.order = FALSE, 
          rot.per = 0.35, 
          colors = brewer.pal(8, "Dark2"))
