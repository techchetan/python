# NLP processing
#### 1. Prerequisite for NLP learn
        You can use another tool also like vs code with python
        1. Download https://www.anaconda.com/download/
           There are detaill mention on website we need to follow the same for installing
        2. open conda prompt and install NLP related lib
        3. Activate the virtual environment
           $ conda activate  <vd or nevironment name>
           $ coda deactivate 
        4. jupyter notebook - It will open Jupyter   
#### 2. Basic Print formating using fstring
        1. print(f"Hello my name is {variable}")
        2. print(f"{auther:{10}} {topic:{30}})
           Its show minimum number of column or space like table column, in case you want allign in one side you can use below 
           syntex 
           print(f"{auther:{10}} {topic:{30}} {price:>{30}})
        3. Handle datetime formating
           from datetime import datetime
           today = datetime(year=2024,month=1,day=2)
           print(f"{todaye:%B}")

  ### You can use https://strftime.org/ for more formate
#### 3. Working with text file
        1. myfile = open(<path + fileName.txt>) use \\ for define path
           myfile.read()
           If you read multiple time then above command return string one time and next time it will show empty because curesor 
           is end of the file. To resolve this error we need to reset the cursor use below command
           
           myfile.seek(0)

           Always close the file using myfile.close()
        2. ** myfile.readline ** is read line by line and return array with all lines
        3. myfile = open('text.txt', 'w+') this option is overwrite complet file. if we use a+ it will create new file if does 
           not exist
        4. Context manager for open file
           with open('help.txt','r') as newFile:
               ...code
#### 4. Working with pdf file
        You need to use PyPDF2 library
        1. Install PyPDF2
                pip install PyPDF2
        2. Example
                import PyPDF2
                myfile = open('text.pdf',mode='rb')
                pdfReader = PYPDF2.PdfFileReader(myfile)
                pdfReader.numPages
                page1 = pdfReader.getPage(0)
                page1.extractText()
#### 5. Regular Expressions
        1. Search phone number r'\d{3}-\d{3}-\d{4}'
        2. text = "The Phone number id 444-564-9847"
           pattern = "Phone"
           match = re.search(pattern,text)
           match.span()
           >>>> resule is (4,9)

           You can try other function related **re**
        3. Other example
           re.findall(pattern,text)  --For multiple occurence, it will show value
           re.finditer(pattern,text)   -- it will show span, you can display using for loop

           for match in re.finditer(pattern,text):
                   print(match.span())

        4. You can try 
           1. r"(\d{3})-(\d{3})-(\d{4})'
           2. re.findall(r".at","there ai cat under the hat")
           3. text = "I am 4 person going to 24 building for running 2 km"
              re.findall(r"[^\d]+")   -- exclusion of digit so [] use for grouping and ^ use for exclusion
              >>>> result ["I am  ","person going to "," building for running "," km"]  
#### 6. Basics NLP
        1. Tokennization
        2. Stemming
        3. Lemmatization
        4. Stop words
#### 7. Spacy
        Spacy is natureal language propcessing open sourece library
        It is use for handle NLP task
        Spacy is release in 2015
   ##### NLTK is another natural language tool kit is very popular open source but it is less efficiant to implementation. NLTK is used for sentimental analysis
   ### Install Spacy on conda
           conda install -c conda-forge spacy
           python -m spacy download en
#### 8. NLP
        nlp() function from spaCy  automatically take text and run some operation for tag,parser and describe the data,
        part of speech en so on. This call as tokenization
        Example:
                import spacy
                nlp = spacy.load('en_core_web_sm')   # Loading language
                doc = nlp(u'Tesla is looking at buying U.S. startup for $6 million') -- get document with token each word is token
                ###### display token
                for token in doc:
                    print(token.text, token.pos_)   -- It shows list of token. pos is part of speech 
        >> Show sentence 
                for sentence in doc.sents:
                    print(sentence)

#### NLP Visulizer
        example
                from spacy import displacy
                doc = nlp("sdsadsa")
                displacy.render(doc, style='dep', jupyter=True, options = {'distance':110})

#### NLP Stemming
        Stemming is select similer word from document. Spacy is not support to stemming as it support lemmatization. you can us NLTK
        example
                from nltk.stem.porter import PorterStemmer
                
                p_stemmer = PorterStemmer()
                words = ['run','runner','ran','runs']

                for word in words:
                    print(word +'----->' + p_stemmer.stem(word))

                >>RESULT>> run -----> run
                           runner ----->runner
                           ran -----> ran
                           runs -----> run
                
                
#### 9. STOP words:
         Stop words is verty common words those are not adding any information insentence like The,it
         
         import spacy
         nlp = spacy.load('en_core_web_sm')   # Loading language
         print(nlp.Defaults.stop_words)   # it shows all stop words
         nlp.vocab('is').is_stop   >> output True
         nlp.Defaults.stop_words.add('hello')  **# Add new stop word**
         nlp.vocab('hello').is_stop = True
#### 10. Phrase Matching:
        import spacy
        nlp = spacy.load('en_core_web_sm')   # Loading language

        from spacy.matcher import Matcher
        matcher = Matcher(nlp.vocab)
        # Search SolarPower 
        pattern1 = [{'LOWER': 'solarpower'}]

        # Search Solar-Power 
        pattern1 = [{'LOWER': 'solar-power'}]

        matcher.add('SolarPower', None, pattern1, pattern2)
        doc = nlp(u"Ther Solar Power industry continues to grow a Solar-Power")
        found_docs= matcher(doc)
        print(found_docs)   # It shows numbers and start and end token position
        >>output [(123234545454353,1,3)]
