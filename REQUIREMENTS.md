# Requirements
## End User 1 — Real Estate Profesional 

The first client is a real estate professional. They provide an unbiased, thoughtful, professional estimate of a home’s market value. They must be aware of the market, and they frequently visit and measure houses. They form their opinion of the value of a house using their knowledge and judgement, and they support that with analysis of comparable sales. 


This system should be able to perform QC compliance review to make sure the work meets professional and lender requirements— USPAP, FNMA, and individual lender guidelines. It should also have reliable, structured MLS market data analysis using Python to analyze the local market data, and SQL to query the data. It should be able to draft the narrative part of the reports that are grounded in style examples of the client’s past work. Lastly, it should have paired sales analysis to make work more efficient and accurate. 


Currently, this end user spends 15 minutes checking the reports and requirements for each client, and sometimes makes mistakes; this system will be useful  if it reduces this task to a few seconds. It will be useless if it gives them information that is already easily accessible. Searching for MLS data is a frustrating task for this end user currently, so this system should provide direct query access to the MLS database via the MLS Grid API. This user already utilizes AI like Google Gemini for regression analyses on the market, so this local system that is being created should improve this end user’s existing AI workflow. It should be a work around the token limit that this end user deals with when using other AIs. Another reason this project is necessary is confidentiality. USPAP: The confidentiality rule states that this end user cannot share conclusions of an appraisal report to anyone other than the client. Since using an online source like Gemini is monitored by people, that could be considered a violation of the confidentiality rule. A local LLM would let this end user avoid breaking the rule while getting QC assistance in writing the appraisal report. 


## End User 2 — Academic Researcher 

The next client is an academic researcher. They are a university affiliated professional who expands the specific field's body of knowledge through research, writing, and interviews. 


The main need from this project is a secure way to analyze transcript data. This system will help them draft literature review sections by retrieving multiple passages from relevant journal articles and books. It will also be able to transcribe a long interview with the research participant, being able to separate the interviewer and interviewee; this will let this end user analyze the transcript.  This end user struggles with NVivo security issues; this project will help remove that issue.  This end user’s current workflow uses Google Scholar and the university library to retrieve important documents. This system would be useful if it makes a 90-minute interview transcription go from 2 hours of work to less than 10 minutes.  It would not be useful if it is inefficient or fails to provide relevant feedback or documents. 

