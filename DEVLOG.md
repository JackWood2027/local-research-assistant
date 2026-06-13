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

## 2026—06—10 — Component selection and reasoning

The discipline that framed my original component selection was simple: don't pay for capability the workload doesn't use. For the CPU, I chose a mid-tier model with the 9900X 12-core because it comfortably handles OS, LLM serving, vector DB queries, and Python data work in parallel; 8 cores would be tight, 16 cores is overkill. Choosing the 3090 over the 4090 has already been covered, but the philosophy of not choosing things that would be overkill and saving money is embodied by that decision. 

The remaining component decisions were thoroughly researched for compatibility, reliability, and performance for this project. The storage that was selected was the Samsung 990 Pro 2TB because of the DRAM cache and the reliability of the Samsung brand. The motherboard selected is the Gigabyte B650 EAGLE AX because it is AM5 compatible and matches with the CPU, and it is the right chipset tier. The PSU selected is the MSI MAG A850GL. I approached the PSU decision by realizing that size for peak system draw plus headroom for component aging, capacitor degradation over time, and transient spikes is needed. This PSU has 850W; the sum of the other parts is 550W. The case is the Corsair 4000D Frame because it fits the GPU easily (405mm case clearance and 315mm 3090 GPU = 90mm margin). Cooler is the Peerless Assassin 120 SE with 265W TDP margin over the 9900X's 170W. The Operating System is Ubuntu because it is free and is great for AI. It is an open source software, is Linux first, and has deep customizability for debugging. The first complete spec came out to be $2735, well under the $4,000 budget.

The RAM (G.Skill Trident Z5 Neo RGB 64 GB) was chosen because the apps, vector DB, and OS add up to around 15GB, and the 64GB gives 2-3x that amount in headroom which is enough to buffer memory spikes. Technically 32GB would work, but one large memory spike would cause the system to swap to disk, which is why 64GB is the choice. There was one surprise with the RAM though, and that was the price. At the beginning of the year similar RAMs were around $300, and now they are closer to $920. Even with this huge price gap, the stakeholder was asked and gave explicit approval to absorb the cost premium for the workflow headroom — multi-process operation without disk swap matters more to him than the savings. This significantly changes how close this project will get to the budget of $4000. AI-quoted prices reflect a knowledge cutoff and can be months stale, so verifying current pricing via PCPartPicker is necessary. 

There were two risks that were identified on PCPartPicker.com. There was a BIOS update that may be needed (Q-Flash Plus on the motherboard which allows you to flash BIOS from a USB stick without a CPU installed) and RAM clearance with the cooler (might need to raise the fan 4-6mm if needed). Catching these issues at spec time means planning around them. Catching them mid-build means a stalled assembly.

## 2026-06-12 — Scope change:over-build for dual-GPU future option

The stakeholder reviewed the original spec and requested an over-build to preserve future dual-GPU capacity. A 16-core/32-thread CPU, motherboard with x8/x8 PCIe lane splitting, larger PSU sized for two GPUs at peak, and a larger case were all requested. This is not a failure in design — it is normal engineering. The stakeholder wanted something different, and so the computer was re-specified.

There were two components that I made the mistake of overshooting when I selected them, that being the ASUS ROG Crosshair X870E Hero motherboard which was ~$650 and was misidentified originally for having a X670E chipset. The ASUS ProArt X670E-Creator (~$300) meets the requirement of x8/x8 PCIe splitting for future dual-GPUs, and it is not an overshoot. The PSU first pick was the be quiet! Dark Power Pro 13 with Titanium efficiency (~$450), but the SeaSonic PRIME PX-1300 is only ~$350 and is Platinum tier. There is a marginal difference between Platinum and Titanium, so saving $100 is worth it. I felt the pressure to max out every component because the best features are so intriguing, but as an engineer I need to have the discipline to meet the new requirement, not to overshoot it.

The original spec was ~$2,735, and the revised spec is ~$3,375. This increase is to preserve the dual-GPU option, and if another GPU is added then it will be around ~$4,100. That means there is a $640 option premium for future flexibility. If the second GPU ends up being added, then this will be a good investment. 

The stakeholder signed off on the final spec; the motherboard supports immediate single-GPU use and future dual-GPU expansion. The PSU and case are sized for the eventual second GPU as well.


