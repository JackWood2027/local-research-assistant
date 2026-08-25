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

## 2026-06-29 — Final Order and decision making

The $3,375 spec was right at approval time, but markets and availability moved between approval and order, so the actual build required substitutions. This entry covers every change and the reasoning. 

The motherboard at approval time was the ASUS ProArt X670E ($300), but it was not available anywhere on the market for a reasonable price because it was an old chipset. There were three new options evaluated: ASRock X870E Taichi Lite, MSI MPG X670E Carbon WiFi, and ASUS ProArt B850-Creator Neo. The B850-Creator Neo was not verified to have x8/x8 PCIe splitting, and it was a lower chipset tier, so it was immediately ruled out. The MSI MPG recreated the unavailability of the X670E generation chipset issue, so it was ruled out. That left the Taichi Lite, which ended up being the decision. It is x8/x8 verified and is current gen and available. 

There was a pre-built that was available for sale that included many parts that were included in the original spec (Ryzen 9 9950X, MSI MAG X870E Tomahawk WiFi, 64GB DDR5, 850W PSU, NZXT H7 Flow, Arctic LF III). There were two issues though: The 850W PSU was insufficient for dual-GPUs, and the motherboard failed x8/x8 PCIe splitting requirement. This option was immediately shut down because of those two issues. 

The RAM, storage, and PSU were changed because of availability. The G.Skill Trident Z5 Neo CL30 ($900) was not available, so the G.Skill Ripjaws S5 CL36 ($1,107) was chosen and ordered. The Samsung 990 Pro PCIe 4.0 ($400) was the original choice, but the Samsung 9100 Pro PCIe 5.0 ($425) is the current gen so it was available and it was ordered. The difference between the models is largely invisible for this workload. The PSU was supposed to be the SeaSonic PRIME PX-1300 ($350), but the Corsair HX1500i (2025) 1500W Platinum ($400) was available and ordered. There was a slight capability upgrade — 1500W vs 1300W gives more headroom for GPU spikes. 

There were seven cases that were considered. The Corsair 5000D Airflow ($160) was the original approved spec; unavailable at order time. The Corsair FRAME 5000D RS ARGB ($200) was rejected because of the modest thermal gain. The Antec C8 ($120-$140) was a cheaper option, but it would be a harder first build and it would have poor thermals, so it was ruled out. The Fractal Design Torrent ($220) was rejected because it would be a harder first build, and it would be beyond bursty LLM workload needs.The LANCOOL III was selected and ordered for its balance of value, size, build ease, and thermals. When it arrived, physical inspection revealed an issue not visible from spec sheets — in a mid-tower the second GPU sits too close to the case floor, with restricted airflow underneath that creates a thermal bottleneck under sustained dual-GPU load. The case was returned. The Phanteks Enthoo Pro 2 Server Edition ($220) and the Fractal Design Meshify 2 XL Black (Tinted Glass) ($220) would both be viable options, but the Fractal Design Meshify was found quicker and so it was the final decision and purchase. The characteristics of this case that were required were: A strong mesh front, top, and side for a strong airflow throughout, full-tower form for proper dual-GPU airflow, E-ATX support for the Taichi Lite motherboard, at least 8 PCIe expansion slots, and conventional internal layout. Three Arctic P14 PWM 140mm top exhaust fans ($36) were also purchased.

The CPU cooler was switched from the Thermalright Peerless Assassin 120 SE ($35) to the Phantom Spirit 120 SE EVO ($50). This was because the Spirit has one extra heatpipe, making the TDP capacity slightly higher, and a few degrees better under sustained load. Initial reasoning for the swap was framed as 'to avoid having to undervolt,' but reviewing the technical details revealed that undervolting is an independent optimization unrelated to cooler choice — a better cooler shifts temps ~1-2 degrees celsius, while undervolting shifts them ~10-15 degrees celsius. The Phantom Spirit is a small thermal margin upgrade, nothing more.

The final build as ordered is: one NVIDIA GeForce RTX 3090 GPU, AMD Ryzen 9 9950X CPU, Thermalright Phantom Spirit CPU cooler, ASRock X870E Taichi Lite motherboard, G.Skill Ripjaws S5 64GB memory, Samsung 9100 PRO 2TB storage, Fractal Design Meshify 2 XL Black case, Corsair HX1500i 1500W 80+ Platinum PSU, three Arctic P14 PWM 140mm top exhaust fans, and the Ubuntu Linux OS. All supporting components are sized for a future dual-GPU expansion; adding a second 3090 later would bring the total to approximately $4,900.

The three biggest takeaways and lessons from picking the parts were markets move between spec and order (as seen with the X670E disappearance) the case was trickier than expected because it had to fit the needs of all the other specs, and verifying the reasoning behind decisions is needed, not just the decisions themselves. 

## 2026-07-13 — GPU acquisition pivot: dual 3090 plan to single 3090 Ti FE

The original plan was to get dual RTX 3090s on the used market to enable the 70B-class model support. Unfortunately, finding two RTX 3090s in good condition in this market for a good price is almost impossible. Instead, a RTX 3090 Ti Founders Edition was purchased, which has worse thermals since it has an extra 100W, but it is slightly faster. A Gigabyte Gaming OC 3090 was evaluated as a candidate for the second card. Its used-market status (potentially mining-run, potentially with original thin thermal pads) drove the design of a stress-testing protocol using gpu-burn, nvtop, and nvidia-smi, intended to validate any used GPU before committing it to the build. The protocol was never used because acquisition pivoted to the 3090 Ti FE before that step, but it's documented and reusable for future GPU purchases.

The VRAM ceiling(24GB) is unchanged with only one 3090. A dual-GPU expansion path is still in the future if one is available on the market for a good price, but the current plan is a single card. The 3090 Ti FE does have 450W versus the expected 350W of a standard 3090. The Corsair HX1500i handles this easily, and the Meshify 2 XL case has plenty of fans to handle the thermal load. The FE pushes air through the card rather than exhausting it into the case. This is worth watching to make sure the thermals are still okay. The $640 option premium documented in the earlier dev log — spent on components sized for future dual-GPU — remains a bet on future flexibility rather than a used capability. The pivot to a single 3090 Ti FE doesn't invalidate the over-build; it just means the flexibility is dormant.

Here are the lessons from this entry: Used-market GPU plan was contingent on finding two good GPUs for a fair price which sounded good on paper, but in practice was very challenging. The pivot to one GPU is appropriate though. The over-build of the rest of the PC still holds value, even without the dual-GPU. It allows for flexibility within the budget for future GPU purchases. Having a stress-testing protocol for the possible GPU options was a good lesson for future GPU acquisitions. 

## 2026-07-14 — PC assembly and Ubuntu installation

The PC assembly is completed. All the parts from the ordered build (Ryzen 9 9950X, Phantom Spirit cooler, Taichi Lite motherboard, Ripjaws S5 RAM, 9100 Pro NVMe, Meshify 2 XL case, HX1500i PSU, three Arctic P14 top exhaust fans) came together well, and the 3090 Ti FE was installed as the single GPU. 

The BIOS was updated to the current version. Unlike the earlier ProArt X670E-Creator plan, which would have required a BIOS update for CPU compatibility with the 9950X, the X870E chipset ships with 9950X support from the factory — so this update was for standard stability and security improvements, not a compatibility fix.

All four subsystems were detected during the BIOS-level hardware verification: CPU (9950X, 16 cores), RAM (64GB, verified at 6000 MT/s after enabling EXPO), storage (Samsung 9100 Pro on the Gen 5 M.2 slot), GPU (display output confirmed during-boot from the 3090 Ti's HDMI).

EXPO enabled to bring RAM to rated 6000 MT/s — without this, the DDR5-6000 kit runs at 4800 MT/s default which would mean the additional speed that was purchased wouldn't be used. This is very important for the RAM speed for the vector database and RAG pipeline. 

Ubuntu 24.04 LTS installed from USB, with the “Try Ubuntu” hardware validation step first. This confirmed CPU, RAM, storage, GPU, and network via the different commands. 

Settings: Interactive installation, Default selection (not Extended — clean install, no bloat), proprietary drivers enabled (NVIDIA driver + media codecs), disk fully erased and installed, no encryption (dedicated stationary machine, not a laptop that travels — encryption adds boot-time password overhead and recovery complexity for minimal real threat model), ext4 file system (installer default), no Active Directory (standalone workstation), location services off (not needed, aligns with privacy-first project ethos), Central time zone, local user account created.

Lessons from this entry: Running the “Try Ubuntu” from USB before committing to the installation was a low-cost check to validate that everything was working correctly. Hypothetically if something was not detected in Linux and this precaution did not take place, there would be serious issues. This habit of validating before committing generalizes beyond PC work. Defaults are usually right for a reason is the next lesson that stands out. Every option came back to picking the simpler options unless there was a specific reason not to, which was rare. The last lesson is that every setting has a workload connection. Enabling EXPO wasn’t just nice to have, it was the difference between the RAM’s full capability versus a cheaper RAM. There would be no point in paying the extra money for the better RAM if this setting was picked. Skipping proprietary drivers would have meant no CUDA. Taking every choice seriously is engineering, not paranoia.

## 2026-08-04 — Ubuntu system update

Today the system was updated with the commands apt update, full-upgrade, autoremove, and clean, then the machine rebooted, NVIDIA driver verified functional with nvidia-smi and that was the end. 

The lesson today was that Ubuntu install's proprietary-driver checkbox correctly installed the NVIDIA 595 open driver, thus the apt full=upgrade bumped the driver to 595.84 and installed matching kernel modules. The driver hardware worked on the first reboot. 

The GPU was detected, the VRAM had the correct amount available, idle temp was healthy, power draw at idle 15W, and the driver-level CUDA runtime available at version 13.2

## 2026-08-05 — [Phase 5] Post-install setup: updates, CUDA, dev tooling, Python 3.12, first venv

The first thing done today was system updates. In the terminal sudo apt update was run and 204 packages were upgradeable. The command sudo apt full-upgrade was completed with 187 upgrades, 9 new installs, 1 removal (an old NVIDIA firmware that was replaced). 17 packages were initially held back but that was resolved with full-upgrade. Autoremove and clean were run shortly after, which was followed by a reboot to apply the kernel/driver updates. This all was verified with the nvidia-smi command and it showed the correct information: Driver 595.84, CUDA runtime 13.2, RTX 3090 Ti detected, 24GB VRAM, healthy idle temps (33 degrees Celsius at 15W, P8 state). 

The installed OS turned out to be Ubuntu 26.04 and not the 24.04 LTS I'd planned. I discovered when the pending updates all came from the resolute-updates repo instead of noble-updates. I found out through lsb_release -a that confirmed 26.04. I chose to keep 26.04 because it works fine and CUDA 12.6 installed from the 24.04 repo is functional even on 26.04. There were 50 packages that wouldn’t upgrade and initially I was confused, but it turned out to be a diagnostic signal that was just phased updates. 


The NVIDIA driver runtime supports up to CUDA 13.2; Pytorch's supported CUDA versions on their download page includes 12.6, 13, 13.2, ROCm 7.2, CPU. CUDA toolkit 12.6 was chosen over other 13.X options because it is the mature, well-supported version. Unless there is a specific reason to choose the newest thing out, avoid it. LLM inference doesn't benefit from CUDA 13 improvements, the ecosystem of tutorials, libraries, and troubleshooting is built around 12.X and especially for a first-timer like me. This was installed through NVIDIA's official CUDA repository, rather than Ubuntu's apt package because the latter is often older and not officially supported by NVIDIA for CUDA development work. I ran the install with the -y flag (auto-confirm), which the coaching flagged as a discipline slip — the pause-before-confirming step exists for a reason and I skipped it here. Worth naming so I don't repeat.

CUDA installs to /usr/local/cuda-12.6/ but that is not in the shell's default search path. Appended two export lines to ~/.bashrc. One was for PATH and one was to find CUDA libraries at runtime. Re-sourced .bashrc and verified with nvcc --version which confirmed CUDA 12.6.3 compiler on PATH. 

The dev tooling install was next. There were 17 newly installed packages, which were verified and returned as Python 3.14. 

Python 3.12 was installed alongside Python 3.14 because 3.14 could be too new for the AI/ML ecosystem — some libraries (PyTorch, WhisperX) don't have wheels for 3.14 yet. Discovered during dev tooling install that Ubuntu 26.04 ships Python 3.14 as system Python — surprising, since 3.14 is very new (released October 2025). Deadsnakes PPA was added because Ubuntu did not have python3.12 in the terminal. Deadsnakes is the standard third-party source for alternative Python versions on Ubuntu. It was then installed and verified and the venv module is functional. 

The next step was configuring the git identity, so I linked my profile through user.name and user.email commands and cloned the repo. 32 objects were received which was the expected amount. The first venv was created (python3.12 -m venv .venv). It was activated and the prompt showed (.venv) which confirmed it. The python was then verified with python –version. The .gitignore didn’t show up with the command ls, but I learned that files starting with . are hidden from ls by default. Ls -la shows everything. 

The last step of the day was the first package installed through pip install pandas numpy jupyter and 60+ packages were installed. This was verified via Python REPL, pandas was 3.0.5 and numpy 2.5.1, Upgraded pip from 25.0.1 to 26.2.1.

There were three big lessons/takeaways from today, and the first was picking the slightly older but more reliable choice over the brand new one is actually the right move because there is not an environment for the brand new ones yet. This discipline was shown by selecting CUDA 12.6 over 13.x, and Python 3.12 over 3.14. The next lesson was: verify before committing, you have to double check what is getting deleted or not installing before you press yes. The last lesson was terminal silence isn’t failure. Multiple commands returned no output on success (apt clean, the two export lines). The Linux convention is quiet-means-good, which is different than what I’m used to.

## 2026-08-08 —[Phase 3] First pandas session: Jupyter, real data, exploratory analysis

Today was the first time I launched the Jupyter Notebook inside the venv. Pandas and numpy were imported as pd and np respectively. 

My first attempt loading a housing dataset resulted in a 404 error(URL was stale), so I switched to a different well-known dataset mirror (selva86/datasets) and that worked. 

I explored an old Boston Housing dataset: Shape was 506 homes and 14 attributes. I ran .describe() for descriptive statistics on all variables. The mean home value (medv) = 22.53, max = 50 (top-coded). There was a moderate to strong positive correlation between rooms and value at .695. I then filtered the subset: 333 homes with rm > 6, mean medv of subset = 25.16. Next it was grouped by riverfront(chas): 471 non-riverfront (mean 22.09), 35 riverfront (mean 28.44). I found out there was a non-linear pattern by dividing it into 4 groups with pd.cut(), the means were 15,18,24,45. 

The top-coded aspect of this almost certainly means any home above $50,000 was recorded as 50, which will show up in the MLS data later as well. Small subgroups producing noisier estimates was another thing that I noticed today which came straight from AP Stats. Smaller n means wider sampling distribution and more chance for skewed results. 

One causal claim that I made from observational data was "the riverfront is very pricy", when the data showed an association. This is a classic association versus causation distinction from AP stats — you can't conclude causation from observational studies. 

Two lessons today were: AP Stats and R background transferred directly to pandas and reading output is the analysis. Running the command is one thing, but noticing what those numbers mean is the work. This came up repeatedly with subset size, means, and correlation numbers. 

## 2026-08-09 — [Phase 3] Messy data cleaning: Titanic dataset

This was the second Jupyter session on the PC. I loaded the Titanic dataset from a public GitHub mirror which is a realistically messy dataset with 891 rows and 12 columns. Commands were run to diagnose the data before cleaning it. 

I learned the language of "dtypes" or data types. int64 is whole numbers, float64 is decimals or NaN values, and object is text or anything pandas cannot categorize. Object usually means it is bad data. This was a preview of MLS data. 

There was missing data in three columns. 77% of the cabin data was missing, 20% of the Age data was missing, and 2 rows had missing Embarked values. I removed the cabin since there was too much missing to be useful, I made the missing age numbers the median age of the data (28 years) and then I filled the embarked data with the mode: Southampton. All of these steps were verified before moving on. 

I had to convert the Embarked cleaning code from markdown to code since it didn't execute the first time which was a good thing to know how to do for the future. I also left the computer for a while and df was no longer defined, so I simply ran all cells to fix that issue. 

I ran survival statistics on the cleaned data and found: 38% overall survival. 74% female survived and 19% males survived. By class the survival rates: 63% for 1st, 47% for 2nd, and 24% for third. Grouping by sex and class revealed that class effect was bigger for women than for men. A third-class woman had a higher percentage of survival than a first class man. This is an interaction effect, the variables’ effects depend on each other. 

The three lessons today were: Cleaning is more about judgment then syntax, notebooks are memory-transient, and two-way groupby reveals information that one-way groupby hides. The three cleaning decisions made today each depended on severity and context of the missingness, not on a rule. For the notebook lesson, the file saves the code but the kernel resets wipe the data. Cell numbers tell you what's actually been run. Lastly, additive summaries would have missed the pattern that class matters more for women than for men on the Titanic. 

## 2026-08-11 — [Phase 4] AI fundamentals: how LLMs work

Today was about learning concepts. First off, an LLM is not a system that accesses sources which lots of people think. It is a machine that produces a probability distribution over its vocabulary, samples a token, adds it to the response, and repeats. 

Tokens are chunks of texts (about 3-4 characters) and not full words. Vocabulary size is 50,000-100,000 possible tokens which is the sample space for the probability distribution. 

Temperature plus the sharpness of the probability distribution controls response variation. Identical responses to a prompt happens when the temperature is 0 or when the distribution has only one token with meaningful probability.

Embeddings are vectors of numbers that represent how related words, phrases, and paragraphs are through comparison. Words with similar meanings get similar vectors which are learned from context and not programming. For example, cat and kitten have similar embeddings because they appear in a similar context in training text. 

Attention is the weighted combination of previous token embeddings that are computed on the fly. It determines what tokens or words are the most important or relevant to respond to. This solves the "fading memory" problem of older models, now all previous tokens are equally accessible. 

Training is the process that gives parameters their values. It shows the model text with a word covered up, and it predicts that covered word. Then it measures the error, adjusts the parameters slightly to reduce the error, then that is repeated billions of times. Inference produces output using frozen parameters, unlike training which is the adjustment of the parameters. 

## 2026-08-14 — [Phase 4.5] The Quantization Lab: defending the hardware from first principles

Today was a math session, unlike the previous concept session. The goal was to figure out what is the correct size of memory for this project through reasoning and not just trusting someone else's decision. 

The formula used was pretty simple: Memory required to hold a model equals parameter count times bytes per parameter. There are different precisions that use different amounts of memory per parameter. FP16 uses 2 bytes, INT8 uses 1, and INT4 uses .5. This is why it is called the "quantization lab" — the different numerical precisions trade accuracy for memory savings.

Here are the results. For a 7B model, FP16 uses 14 GB (or 14 billion bytes), INT8 uses 7 GBB, and INT4 uses 3.5 GB. 13B uses 26 GB for FP16, 13 GB for INT8, and 6.5 GB for INT4. 30B uses 60 GB for FP16, 30 GB for INT8, and 15 GB for INT 4. Lastly, 70B uses 140 GB for FP16, 70 GB for INT8, and 35 GB for INT4.

There is more memory than just the raw weight however. Actual VRAM usage includes KV cache (attention), activations (intermediate computation), and system overhead. The rough estimate is 1.5 times the raw weight size. 

The real estate appraiser (end user 1), will be using the RTX 3090 Ti with 24 GB of VRAM. Looking at the results above, the largest total is the 30B at INT4. Bigger models would require more VRAm than the card provides; smaller models would waste its capacity. Therefore 24 GB VRAM is the correct amount for a 30B ceiling. 

The researcher (end user 2) has a M-series Mac with 128 GB of memory. This allows a larger budget, with the 70B at INT8 being the one that fits comfortably (105 GB with overhead). The Mac’s architecture is specifically well-suited to LLM inference because it doesn’t force a separate VRAM constraint. This is why the researcher's use case (long-context writing assistance) maps to the Mac target rather than to a PC target. 

The memory ratio between a 30B model at FP16 versus INT4 is 4x. Without quantization, a 30B model wouldn't fit on any consumer GPU. With INT4 quantization, it runs on a 700 dollar used RTX 3090. Quantization isn't an optimization, it's the reason local LLm inference on consumer hardware is possible. 

This session gave me the ability to make a defensible argument on why I chose 24 GB VRAM over other choices from this math today. I also now have a framework that generalizes what a larger chip would enable through the equation today. I also gained some real intuition for the local LLM ecosystem. Understanding quantization explains why Ollama's file format (GGUF) is built around it, why model download options are labeled with quantization tiers (q4_K_M, q8_0), and why the Mac's unified memory is a different hardware architecture — not just more memory, but memory the GPU can actually use.

The big lesson I had today was simple math can make architectural decisions defensible. Hearing the word "quantization" might make it seem like the math was high level, but it was just simple multiplication and division that gave me defense for my decisions. 

Today was about the transition from understanding embeddings and cosine concepts to using them in code. I proved the concepts through a small RAG retrieval system. 

A vector database is a specialized data store designed to store embeddings and find the most similar ones to a query, quickly, at scale. Regular SQL databases can't do this efficiently because their indexes are built for exact matches and range queries on scalar values and not high-dimensional similarity search. Vector databases trade some accuracy for massive speed gains, using index structures like HNSW to find approximate nearest neighbors in logarithmic time instead of linear time. Qdrant was selected for this project because it is open source, fast, mature ecosystem, and local-friendly. 

I installed Qdrant-client and sentence-transformers in the project venv. Loaded the all-MiniLM-L6-v2 embedding model (384 dimensions, small and fast). Three samples were encoded into vectors, created a Qdrant collection configured for 384-dim vectors with cosine distance, inserted the embeddings as points, then queried with a new question and retrieved the most similar stored sentences. 

The query was “Where does the cat rest?”, a sentence not stored in the database. The three stored sentences were: “The cat sat on a mat,” “A feline rested on the rug,” and “The stock market crashed yesterday.” The similarity scores came back as .5431, .5062, .0730 respectively. Even with no words in common, the first two sentences had a very similar score. Keyword search would have missed this, but embedding search caught that these words are synonyms, and the training data showed this pattern. This is how the RAG will work for end user 1’s use case.

The first debugging moment was a typo which was an easy fix, and the second one was also a typo but the type was lien instead of len which are both real words. Fixing both of these was simple and I know that the last line of Python error tells me what's wrong. 

I now have a RAG mental model. All RAG systems on earth is a scaled-up version from what I did today. Documents turn into chunks, then embeddings, then it is stored in a vector database. Query, then embedding with the same model, then searched against the stored vectors. Top-K results returned into their payloads. Payloads inserted into an LLM prompt as retrieved context. The only difference is scale. 

There were two lessons that landed today. The feline/cat result proved this on real numbers. Keyword search operates on exact spelling; embedding search operates on meaning learned from training data patterns. This is why RAG is worth building over a simple SQL search of documents.The Phase 4 concepts weren't abstract — they showed up as running code. Every piece of today's session mapped directly to something from Phase 4: cosine similarity, high-dimensional vectors, the distributional hypothesis (why similar words end up with similar embeddings). The math and the code are the same thing, just at different levels of abstraction.
 
