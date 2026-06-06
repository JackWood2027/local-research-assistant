## 2026-05-17 — Project setup and initial decisions                                                                                                          

This project is a privacy-first local LLM assistant built for data analysis and research assistance. This is local because the people that it is built for must keep their clients' information private based on research laws and ethical reasons. This is being built for two end users from different professional domains.

 I picked the name local-research-assistant because it covered the main idea of this project, it wasn't too complex for the average person to understand, and it wasn't too simple either. I thought about private-rag-model, but that one was not accurate because this is more than a model, so I did not use that one. The other option was family-ai-project, and that undersold the project, so choosing local-research-assistant was an easy decision to make. 

I chose the MIT license because I want people to be able to learn from this, and for it to be easily accessible for others. 

I have committed to not identifying anyone on this to make sure they have their own privacy. I will keep everything in abstract descriptions.


## 2026-05-30 — Requirements interview with end users

I started with a draft based on my knowledge of both users' jobs, then I interviewed them. 

I was correct about the use cases and what a successful system would look like. I was unaware about the complexity and details of an appraisal report before this interview. I learned about the sales comparison approach and the cost approach narrative. I learned that the most important thing that this project will do to this end user is search for market 
data. 
Right now, this end user struggles with all the steps they have to go through for market data, so this project should help them do this in a shorter amount of time. Another reason this end user finds this project helpful is confidentiality. USPAP's confidentiality rule states that one appraiser cannot share conclusions of an appraisal report to anyone other than the client. Since using an online source like Gemini is monitored by people, that could be considered a violation of the confidentiality rule. A local LLM would let this end user to avoid breaking the rule while getting QC assistance in writing the appraisal report. 

Interview took place in person on 05/27/26. Audio recorded with consent, transcribed using Google Gemini, then verified for accuracy with the participant. 
Examples of questions and answers were:
Can you walk me through what an appraisal report actually looks like?
"As a residential appraiser, I use a specific form because most of my work is for lending purposes. The process is broken down into different parts. First, I fill out part of the form and take photos on-site at the house. When I return to the office, I complete the narrative sections of the report. This includes the neighborhood description, market conditions, descriptions of the improvements, and any appraisal issues that arise.
Next, I develop a sales comparison approach and write a narrative for that, followed by a cost approach. Finally, I write a reconciliation that balances the different approaches to value, perform a quality control review of the entire appraisal, and deliver it to the client."
What's annoying or time-consuming in your current work?
The limitations I face when searching for market data. Everything is done through the MLS website, and some of the search functions are limited. Often, I have to open each individual listing just to find the specific data I need. It would be incredibly helpful to have direct API access to the data—the MLS does offer one called MLS Grid—so I could query the market sales directly and conduct my research more efficiently.
I've also been using AI, specifically Gemini, to run regression analyses on market data and draw conclusions about neighborhood market conditions. I would love to streamline that process. Additionally, I’d like to apply AI to the sales comparison approach. When I'm searching for the best comparable sales, having AI assistance would ensure I'm not missing any good options.
"It could also help me identify reliable "paired sales," which is a method appraisers use to determine the contributory value of a specific property feature. For example, by analyzing a large dataset of 500 sales, AI could help me figure out exactly how much a pool contributes to a home's value compared to a home without one. This would allow me to reflect that value more accurately in the sales comparison approach, and ultimately, the appraisal itself."

For end user 2, I was correct about a lot of the actual things that this end user does for work—interviewing, researching, writing, etc. I learned a lot about what this end user finds annoying, and that is Nvivo having annoying security issues. The last two things I learned are that Google Scholar and the university library are the two places they find papers to cite, and the main need from this project is a secure way to analyze transcript data.
Interview took place in person on 05/29/26. Audio recorded with consent, then verified for accuracy with the participant. 
Examples of questions and answers were:
How do you write a literature review? 
"I review high-quality peer reviewed literature about the related topic and synthesize it."
Where do you find the papers you cite?
"Google Scholar or through my university library."
What do you do with interview recordings after the interview is over?
"Recordings are destroyed, but the interviews are transcribed and the transcriptions are what I use for analysis."
What software do you use right now and what's annoying about it?
"Nvivo and Atlas.ti are annoying because their security issues."
What do you want from this project?
"Secure way to analyze transcript data"

## 2026—06—04 — Hardware spec reasoning

I learned today that the model size will drive the compute of the hardware, not the data size. Being in the stats world for a while, I thought that the price of the hardware would be based on the data size, but it is actually based on the model size. In the LLM world, data is small (prompt only a few KB) and the model is huge. The parameter count (7B vs 30B vs 70B) determines cost. 

I selected the 30B-class model because of these factors: Reliability, sufficient quality, and processing speed not being critical. The most important thing is that the QC review needs to be extremely accurate, so it can't be a small model. The writing quality does not need to be perfect since this end user will revise and edit the drafts. The speed is not critical; this end user has expressed that processing can take a few seconds versus instantaneously. The reason why going all the way to 70B is a mistake is because of the sharp cost cliff between 24GB and 48GB of VRAM. A single consumer GPU tops at ~24GB, going higher means either dual GPUs (extremley complex and expensive) or workstation-class cards. The end user does not want to pay more than $4,000 for this whole project. This means the 24GB VRAM is sufficient for this system. 

I chose the RTX 3090 over RTX 4090 because they both have 24GB VRAM. For our use case, the 4090 isn't a better LLM card in any meaningful way other than the fact that it runs models faster, which is not worth $1000 to the end user. We can save on this  aspect of the project while getting sufficient capability from the 3090. This unlocks a higher budget for the rest of the project, thus making the rest higher quality. 

Both systems were spec'd using the same right-sizing reasoning, but different platform constraints lead to different correct answers. There are two different platforms for this system: end user 1's PC (30B/24GB consumer GPU) and end user 2's MacBook (70B/128GB unified memory). End user 2's 128GB memory fits a 70B model. This model will produce higher-quality writing which benefits their academic work. End user 1 will have a single-consumer-GPU build PC, so 24GB and 30B are the correct targets.  

