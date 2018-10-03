# 01-BRI-Summary


## Overview

The Baugh Relationship Index (BRI) automates the administration and interpretation of a relations style questionnaire. 

From the end user (therapist) point of view, the Baugh Relationship Index (BRI) is straight forward. The survey is administered typically by e-mailing a URL the client/patient. There is also a paper version that can be filled out by hand then coded by the clinician.    

The real work of the site is done in the background with scoring a test taker’s responses and saving the results. Later the end user retrieves the scores with interpretation which the BRI site produces in the form of a PDF about 15 pages long.

The BRI goes back to the 1970’s when development was started by a PhD psychologist in Jackson, MS. The counseling tool was later automated on PC. I migrated the survey and data to Windows then to the web in 2005. 

This 49-item inventory has two versions. A social version that is typically therapists and a work version that is usually used in the business setting.  The social version can produce and compatibly summary between two individuals that have completed the BRI.  


### Home Page 

The home page is simple with just log in and some related documents. The documents are not available for at the moment. 

![bri-home-700-border](http://johnvardaman.com/BRI-home-jv.jpg "BRI Home Page")

### Dashboard

After logging in the end user is presented with a dashboard and menu options. Most of the time the end user will be just administering and retrieving and interpretation in narrative format. 

![bri-dash-700-border](https://user-images.githubusercontent.com/23184069/45260993-f1b18780-b3bb-11e8-9b6a-14bb504f00bb.jpg "BRI dashboard")

### Send e-mail page

To e-mail the BRI, one just needs to provide name, email address and select the version. There are a few other options. The user can move-over the info icons  for a better explanation. I like to use info icons in case a user may not fully understand what is going on. 

![bri-send-700-border](https://user-images.githubusercontent.com/23184069/45260996-fb3aef80-b3bb-11e8-80c7-f623ca32377b.jpg "Send e-mail")

### Self-administration by e-mail

When the test-taker receives the BRI e-mail and clicks on the URL the person arrives at the page below. A form with 49 items is presented and also prompts for personal information. The items and statements within are randomized unless the therapist selects otherwise. On average the form takes about 30 minutes to complete. 

![bri-admin-720-border](https://user-images.githubusercontent.com/23184069/45260999-03932a80-b3bc-11e8-8aee-9700c975fd89.jpg "E-mail administration")

### Status of e-mails

The therapist end-user and check on the status of e-mails to clients. Backup options are detailed should a client say that no e-mail was received. 

![bri-stat-750-border](https://user-images.githubusercontent.com/23184069/45261001-07bf4800-b3bc-11e8-9d63-4136c3f6bce3.jpg "E-mail status")

### Narrative (Interpretive text) 

A lot of the PHP code is involved the complex algorithms that generate  the BRI interpretation. Below are samples of the cover, Interpretation and actual scores. The work version, as opposed to social, in this case. The PDF report is implemented with FPDF. Originally PDFlib was used.

![bri-narr1](https://user-images.githubusercontent.com/23184069/45261164-004e6d80-b3c1-11e8-8f0d-de5df333cd5b.jpg "Cover page")

![bri-narr2](https://user-images.githubusercontent.com/23184069/45261295-bb2c3a80-b3c4-11e8-805c-dcc6a9ca50b4.jpg "Interpretation" )

![bri-narr3](https://user-images.githubusercontent.com/23184069/45261166-09d7d580-b3c1-11e8-94c5-d16cf8a0cc09.jpg "Score Page")
