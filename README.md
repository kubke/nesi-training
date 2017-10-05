# nesi-training

## libraries

* ggplot2
* plyr
* reshape2


## GLOBAL

### Dataframes

* reshed: funded eps by institution and nesi-learners by institution, by year
* reshedall: Same as reshed, but clumps all non-university into a single 'all other' line
* resdiscipline: funded eps by discipline and all nesi users by discipline.
* rescri: cris (+ 'other') contains staff number and nesi learners by year
* rdsurvey2106: workforce as per 2016 R&D survey
* nesi.training.all: contains the data on NESI training activities


### Variables
hed.researcher.number: researcher number in HED as per R&D 2016 survey
hed.researcher.fte: researcher fte in HED as per R&D 2016 survey
gov.res.number: researcher number in government as per R&D 2016 survey
gov.res.fte: researcher fte in government as per R&D 2016 survey
hed.tech.number: technician number in HED as per R&D 2016 survey
hed.tech.fte: technician fte in EHEDas per R&D 2016 survey
gov.tech.number: technician number in government as per R&D 2016 survey
gov.tech.fte: technician fte in government as per R&D 2016 survey
hed.student.number: student number in HED as per R&D 2016 survey
hed.student.fte: student fte in HED as per R&D 2016 survey
hed.students.enrolled: sums all Ms and PhD students enrolled in HED
discipline.students.enrolled: sums all students enrolled in all disciplines
workforce.gov.2016: researchers and techs in gov R&D 2016 survey
workforce.hed.2016:researchers, students and techs in gov R&D 2016 survey
workforce.2016: sum of hed and gov workforce from R&D 2016 survey

hed.workforce: sum of all normalized students + researchers + technicians (from reshedall)
cri.workforce: sum of cri workforce in rescrionly
hed.cri.workforce: CRI + HED workforce
discipline.workforce: sums of all researchers, normalized students and tech (from resdiscipline)

total.hed.learners : nesi learners in hed from reshedall
total.cri.learners: learners in cris from rescrionly
total.other.learners: learners that are neither cri nor HED (from rescri|other)
total.learners: All nesi learners
total.hed.cri.learners : learners in CRI + HED

percent.hed.learners: learners as percentage of hed workforce
percent.cri.learners: learners as percentage of cri workforce

nesi.users: sum of all nesi users from resdicipline (from annual report)
percent.nesi.users.gov.hed: Nesi users as % of total gov+HED workforce
percent.nesi.users: NeSI users as % of total HED workforce (from resdiscipline)




## Chunks
### Chunk setupenv

Need to modify setwd to local directory, as it assumes R script and all files are contained in the same directory.


### Chunk workforce

reads 4 csv files that are put into 4 dataframes:
reshed: 
* institution: name of higher ed institution
* inst.category: separates institutions into universities ('heduni') or other ('hedother')
* relationship: the relationship with NeSI
* student.fte: FTE of students for each isntitution
* student.number: headcount of students for each institution
* funded.ep: number of PBRF funded EPs
* nesi.learners.YEAR: number of nesi learners per institution for each year

rescri:
* institution: name of cri - also a line with 'other' which captures data from other institutions (eg, govt)
* inst.category: separates intistutions into 'cri' and 'other'
* relationship: the relationship with NeSI
* total.fte: Total CRI staff FTE (all categories)
* total.number: Total CRI headcount (all categories)
* res.staff.fte: 'Researchers' FTE
* res.staff.number: 'Researchers' headcount
* tech.fte: Technical staff FTE
* tech.number: Technical staff headcount
* other.fte: Support and other staff FTE
* other.number: Support and other staff headcount
* nesi.learners.YEAR: number of nesi learners per institution for each year

resdiscipline
* discipline: discipline categories
* student.fte: FTE of Masters and PhD students in each discipline
* student.number: headcount of Masters and PhD students in each discipline
* funded.ep: number of PBRF funded EPs for each category
* nesi.users: number of nesi users per discipline

rdsurvey2016
* role: research workforce categories
* bus.number.2014: headcount for business in 2014 for each role category
* bus.number.2016: headcount for business in 2016 for each role category
* gov.number.2014: headcount for government in 2014 for each role category
* gov.number.2016: headcount for government in 2016 for each role category
* hed.number.2014: headcount for higher education in 2014 for each role category
* hed.number.2016: headcount for higher education in 2016 for each role category

### Chunk create reshedall

takes reshed, and groups all non-university HED institutions into a single line called 'all other'. Puts all data into reshedall. 
Replaces all NA with '0'

### Chunk hedresearchers

Reads the values from rdsurvey2016 and puts them into variables. 

in dataframe: 'reshedall'

Calculates the percentage of EPS per HED institution and discipline (funded EPS*100/total EPs) and puts it into column named 'percent.number.ep'. 
Calculates the total number and fte of researchers in HED from the percentage of funded eps and puts the values in columns 'res.staff.number' and 'res.number.fte'. (%funded ep * # researchers in RD survey /100)  

### Chunk hedtechnicians

reads number/fte of technicians in rdsurvey2016 per sector and creates corresponding variables. 
Using reshedall, calculates tech number in HED institutions as proportion of funded EPs. (% funded eps * technicians number/100). Puts in column 'tech.number'. Same with FTE into 'tech.fte'. 

using resdicipline, does the same as above but for HED discipline. 


### Chunk hedstudents

reads student number and fte from rdsurvey2016 and creates corresponding variables. 

Using reshedall, calculates total number of students enrolled from 'student.number' column and puts it into a variable. 
Calculates the percent student per institution and puts in 'percent.students' column (student number * 100 / total enrolled students). 
Normalizes the number of students to the total number of students in HED from R&D 2016 survey (percent students * number of r&D students in hed /100) and puts it into column 'normalized.students'.

Same for resdiscipline. 

Identifies the difference between the number of research students in the two datasets: reshed and rdsurvey2016.

### Chunk hedworkforce

Calculates total HED research workforce by adding the calculated numbers of researchers, technicians and students (normalized) per institution and per discipline and puts in column 'total.number'.

### Chunk criworkforce

reads government research workforce numbers and ftes from rdsurvey2016 (researchers and technicians) and puts them into variables

changes NAs to "0" in rescri

The dataframe rescri contains a line with 'other' that includes other government agencies and other institutions/groups. 

Calculates total number of researchers (researchers + tech) from total number and percentage of research staff and puts into "workforce"column. 



### Chunk learners

calculates total number of learners per institution from the per year breakdown and adds it to a new column 'nesi.learners.total'(into reshedall and rescri). 

Separates cris into rescrionly to estimate the total cri workforce

Calculates the proportion of NeSI learners against total research workforce per institution for HED and CRIs and puts it column 'nesi.learner.proportion'.

Counts total learners in hed, CRIs and 'other' and puts them into variables
Sums up to get total number of learners
adds hed and CRI nesi.learners

calculates the total workforce for HED and CRI, and HED + CRI

estimates the percentage of workforce that are learners for HED and CRI and puts them into variables

Reports back on total number and proportions of learners. 

### Chunk plot learners
Plots the total number of learners per institution as total and per year. The total graphs show the relationship of the institution with NeSI.


### Chunk plot learner proportion

Plots the proportion of the workforce that is a 'learner' for different institutions | years.
For cris, it removes the line 'other' which includes learners from isntitutions that do not fall into either HED or CRI.

### Chunk training sessions 
reads nesi training data file and creates a dataframe, then subsets into two dataframes - one with outreach activites, the other with all training activities

Calculates and reports on the total number of events, and how many are training or outreach. 

Plots training participation by institution

### Chunk user proportion

sums all nesi users in all disciplines and creates a variable
Calculates workforce in HED  by adding researchers, students and techs from resdisicpline and puts them into a variable

Calculates the research workforce in gov and HED, and gov + HED from R&D2016 and puts into variables
Estimates the percentage of nesi users by size of workforce when considering the R&D 2016 numbers, or just using the HED numbers

Reports on these differences and plots against the percentage when considering all users t be in HED.




training data analysis

Licence:

Contributors:

