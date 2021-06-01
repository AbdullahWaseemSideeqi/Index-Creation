# Index-Creation
 Create Index of files present in the directory given as input

## FLow of Notebook
This notebook accepts three separate directories ( ./corpus/corpus1/1/, ./corpus/corpus1/2/and ./corpus/corpus1/3/) as input and make the index of all files present in these three directories separately. It creates two separate files named as index_no_terms.txt and index_no_posting.txt for each directory, the first file i,e. index_no_terms.txt conains the information related to terms present in that directory along with starting byte postion of that term posting list in index_no_postings.txt file and no of bytes taken by the posting list of that term in the index_no_postings.txt file . So the format of index_no_terms.txt file is as follows
                   term, starting byte postion of posting list in index_no_postings.txt file, no of bytes taken by the posting list of that term in the index_no_postings.txt file

Second file i.e. index_no_postings.txt file contains information that any particular term is present in how many document within a directory (and it is known as document frequence) followed by document id and term frequency indicating how many times that particular word came in one particular document and finally followed by positon list indicating the positon of that particular word in the one document. So the format of index_no_postings.txt file is as follows
                   document frequency, document id, term frequency, positon list of term in document
Here a point should be noted that document id and position list in encoded using gamma encoding. Gamma Encoding is explained with the help of following example.
   #### Example
   For example if term tropical occurred in document 1 at position 1, 7 and 6 and in document 2 at position 6,17 and 197 at in document 3 at position 1 then its posting list will 
   be
        Document frequency = 3, Document ID = 1, Term frequency = 2, postion list =  1, 6|  Document ID = 1, Term frequency = 3, Position list = 6, 11, 180| Document id = 1, 
        Term frequency = 1, Position list = 1

As a result of all this 6 files will be created with name as 
1) index_1_terms.txt
2) index_2_terms.txt
3) index_3_terms.txt
4) index_1_postings.txt
5) index_2_postings.txt,
6) index_3_postings.txt
where 1, 2, 3 represent the name of subdirectory on which index was created.

In addition to all this there is another file which this notebook creates and it name is docInfo.txt which contains some extra information about every document of the direcctory. This extra information includes the document id, document directory/document name, length of document and magnitude of document.
                Here 
                          Length of Document = No of characters in the document
                          Magintude of Dodument = Square root of ( sum of square of ( no of times each word comes in the document) )
_________________________________________________________________________________________________________________________________________________________________________________

## Post Processing on the data of documens before creating index of those documents
The first step in creating an index is tokenization. So the document is converted into Tokens suitable for indexing.Hence the tokenizer follow the steps as follows:
1) It accepts a directory name as a input, and process all files found in that directory.
2) It extracts the document text with an HTML parsing library named BeautifulSoup, ignoring the headers at the beginning of the file and all HTML tags.
3) It splits the text into tokens (for this purpose natual language toolkit library is used).
4) Then it converts all tokens to lowercase (this is not always ideal, but indexing intelligently in a case-sensitive manner is tricky).
5) After this it applies stop-wording to the document by ignoring any tokens found in this list (stop words given in stoplist.txt on cactus). Stop words actually means 
   there are certain common words like a, the. above, also and on etc so you don't have to crate token for these words to save the memory hence you don;t form token of these 
   words.
6) Finally it applies stemming to the document using any standard algorithm – Porter, Snowball or KStem stemmers are appropriate. You should use a stemming library for this 
   step. Stemming actually means converting every single word to its origin word i.e development word will be changed develop, similarly eating word will be changed to eat when
   we apply stemming on it.

The resulting tokens will be used to create the index of all the documents of the directories given in input as explained under "Flow of Norebook" heading.
