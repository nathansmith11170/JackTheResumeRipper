# Jack The Resume Ripper

"Jack The Resume Ripper" is an application for parsing resumes and displaying AI driven metrics on them. Team "*Meh*." built this project for HACK OH/IO (Ohio State University's flagship hackathon) in November 2019. Behind the scenes, "Jack" sends the resume image to an AWS Lambda function which leverages several Azure Cognitive Services API's for text recognition and analysis. The whole thing was a phenomenal learning experience and a ton of fun.
## Demo
[A quick demo of the project in use, recorded for hackathon judging](https://www.youtube.com/watch?v=vfvaiR9Y6nI)
## Tools
* AWS Lambda - *Acting as an intermediary between front end and Azure*
* AWS API Gateway - *For setting up Lamda functions to be executed as an API*
* Microsoft Azure Cognitive Services - *For processing of resumes*
* Vue.js - *For all user interactions*
* Vuetify - *For front end widget designs*

## Notes
- The code in the server folder is not the real backend, but it is a copy of the AWS Lamba functions, meant as an archive. 
- Given the time-constrained nature of the hackathon, we only displayed correlation with provided categories to the resume as a whole. 

## Contributors
* Jeffery Morhous - Front End Design
* Nathan Smith - Middleware and Project Architecture
* Jonathan Soldan - Backend and Data Analysis
