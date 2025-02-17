# Istinomer Fact Checker

Project Description

A chrome extension to have the people over at istinomer.rs fact check text that has been highlighted on a website

Technical Instructions

    Linux Distribution Example: Ubuntu 18.04 LTS t
    MongoDB 3.2.x
    Python 2.7.x

Initial Setup
# 2. Local Installation (UBUNTU)

First create a folder in your desktop called dev:

    $ cd ~
    $ mkdir dev
    $ cd dev

## Getting the project in your local machine:

    $ git clone https://github.com/crtarsorg/istinomer-proveri-me.git

    $ cd istinomer-proveri-me

create a config.cfg inside api directory and paste the contents of config-template.cfg to the new file that you created

    $ cd api 
    $ touch config.cfg 
    $ cat config-template.cfg > config.cfg 

open the config.cfg file and pass the following arguments to it 

[Application]
#put any port that you wish which is not in use example 8000
SERVER_PORT = 8000

#put any string that you wish as a secret key, example: 'mySecretKey'
SECRET_KEY = 'mySecretKey'

[Mongo]

DB_NAME = 'Mongodb'

[Logging]
#this will create a directory named log and a logfile errors.log in the app directory 
PATH = /logs/errors.log 
LEVEL = ERROR 


    $ bash install.sh 

(this script will check whether your system consists python 2.x, if it does (if it does not, it will install python 2.7.3) then it will create a virtualenvironment with python2.7.x and inside that virtualenvironment it will install the python dependecies that are required to setup the app)

# Install the requirements and run the app:

    $ bash install.sh
    $ bash run-debug.sh


Note: In oder to test the installed extension you need to install POSTMAN in your system and make the POST request through it

- Run 
    $ bash create_user.sh
- install POSTMAN API https://www.getpostman.com/downloads/
- make a POST request adding the url in which the server is running in your local system example: http://0.0.0.0:8000/api/entry/submit
- At the body select JSON file application and paste the following JSON payload
- login as admin to view the request


# 3. Build from source
Install the extension manually in your browser
### For chrome
    1. Make sure you comment the local api in the chrome-ext dir at the files containing api-s (background.js, popup.js and chrome-ext/js/content-scripts/sites/inject-css.js) and uncomment the production api-s.
    2. Visit chrome://extensions (via omnibox or menu -> Tools -> Extensions).
    3. Enable Developer mode by ticking the checkbox in the upper-right corner.
    4. Click on the "Load unpacked extension..." button.
    5. Select the directory containing your unpacked extension /dev/istinomer-proveri-me/chrome-ext.
    6. Install the extension
    7. You can try and send requests from selecting text from sites mentioned in chrome-ext/sites.js
    
### For firefox
    Go to your dev directory
        $ cd dev
        $ cd istinomer-proveri-me
    Copy the mozilla-ext directory to your Desktop
        $ cp -r mozilla-ext /home/<name>/Desktop
        $ cd ~/Desktop
        $ cd mozilla-ext/
    Built the add-on with this command
        $ zip -r -FS ../mozilla-ext.zip * --exclude *.git*
        1. Make sure you comment the local api in the chrome-ext dir at the files containing api-s (background.js, popup.js and chrome-ext/js/content-scripts/sites/inject-css.js) and uncomment the production api-s.
        2. Visit about:debugging:
        3. Click on Load Temporary add-on and select the zip/xpi folder that will be created on your system
        4. You can try and send requests from selecting text from sites mentioned in mozilla-ext/sites.js.
      #NOTE: for testing the installation and getting the requests in your local machine uncomment the local api-s and comment the production ones.
## Save Entry
### POST  /api/entry/save
#### Sample JSON Payload - Truthfulness
```json 

{
 "domain": "kurir.rs",
 "url": "https://www.kurir.rs/planeta/3272197/tramp-otkrio-plan-krvavog-napada-na-iran-hteli-smo-da-gadjamo-3-cilja-a-onda-su-mi-rekli-da-bi-poginulo-150-ljudi",
 "text": "Američki predsednik je otkrio kako bi tekao plan američkog vojnog napada na Iran.",
 "chrome_user_id": "xzy",
 "classification": "Truthfulness",
 "grade": "True",
 "category": "Politics",
 "date": 1561128329.148,
 "article": {
   "author": "Carl Bernstein",
   "date": 1561128329.148
 },
 "quote": {
   "author": "Richard Nixon",
   "politician": true,
   "date": 1561128329.148
 }
}


``` 







#### Sample JSON Payload - Promise
```json 
{
  "classification": "Promise",
  "grade": "Fulfilled",
  "category": "Politics",
  "article": {
    "author": "Carl Bernstein",
    "date": "18/04/1973"
  },
  "quote": {
    "author": "Richard Nixon",
    "politician": true,
    "date": "17/04/1973"
  },
  "promise": {
    "due": "01/05/1973"
  }
}
``` 

#### Parameter Options
##### classification 
 - Backlog (Backlog)
 - Consistency (Doslednost)
 - Notepad (Beležnica)
 - Promise (Obecanja)
 - Truthfulness (Istinitost)


##### grade 
###### Truthfulness (Istinitost)
 - False (Neistina)
 - Half true (Poluistina)
 - Mostly false (Skoro neistina)
 - Mostly true (Skoro istina)
 - Pants on fire (Kratke noge)
 - True (Istina)

###### Promise (Obecanja)

 - Almost fulfilled (Skoro ispunjeno)
 - Fulfilled (Ispunjeno)
 - In progress (Radi se na tome)
 - Not started (Ni započeto)
 - Stalled (Krenuli pa stali)
 - Unfulfilled (Neispunjeno)

###### Consistency (Doslednost)
 - Consistent (Dosledno)
 - Inconsistent (Nedosledno) 
 - In between (Nešto između)

##### category 
 - Culture (Kultura)
 - Politics (Politika)
 - Economy (Ekonomija)
 - Healthcare (Zdravstvo)
 - Society (Drustvo)


## Fetch Entries
### POST  /api/entry/get
#### JSON Payload - Filter Parameters 

| Property          | Data Type     | Description                                                   |
|-------------------|---------------|---------------------------------------------------------------|
| classifications   | List\<String\>| The classifications.                                          |
| grades            | List\<String\>| The grades.                                                   |
| categories        | List\<String\>| The categories.                                               |
| article.authors   | List\<String\>| The article authors.                                          |
| article.from      | Date          | The publication _from_ date.                                  |
| article.to        | Date          | The publication _to_ date.                                    |
| quote.authors     | List\<String\>| The quote authors.                                            |
| quote.politician  | Boolean       | Indication whether the quote author is a politician or not.   |
| quote.affiliations| List\<String\>| The quote authors' affiliations.                              |
| quote.from        | Date          | The quote _from_ date.                                        |
| quote.to          | Date          | The quote _to_ date.                                          |
| promise.dueFrom   | Date          | The promise _due from_ date.                                  |
| promise.dueTo     | Date          | The promise _due to_ date.                                    |


#### Sample JSON Payload
```json 
{
  "classifications": ["Truthfulness", "Promise", "Consistency"],
  "grades": ["Mostly true", "Fulfilled", "Consistent"],
  "categories": ["Politics"],
  "article": {
    "authors": ["Carl Bernstein", "Bob Woodward"],
    "date": {
      "from": "01/06/1972",
      "to": "01/01/1975"
    }
  },
  "quote": {
    "author": "Richard Nixon",
    "politician": true,
    "date": {
      "from": "01/06/1972",
      "to": "01/01/1975"
    }
  }
}
``` 

**Note:** Can only apply _promise.dueFrom_ and _promise.dueTo_ filters when classification only contains _"Promise"_ (i.e. `"classifications": ["Promise"]`.
