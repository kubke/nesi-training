# nesi-training

## Chunk setupenv

Need to modify setwd to local directory, as it assumes R script and all files are contained in the same directory.


## Chunk workforce

reads 4 csv files that are put into 4 dataframes:
reshed: 
* institution: name of higher ed institution
* inst.category: separates institutions into universities ('heduni') or other ('hedother')
* student.fte: FTE of students for each isntitution
* student.number: headcount of students for each institution
* funded.ep: number of PBRF funded EPs
* nesi.learners.YEAR: number of nesi learners per institution for each year

rescri:
* institution: name of cri - also a line with 'other' which captures data from other institutions (eg, govt)
* inst.category: separates intistutions into 'cri' and 'other'
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
*** same as above replacing .number. with .fte. for FTEs per sector/year

## Chunk researchers

Calculates the total number and fte of researchers in different roles (researcher, technician)
creates a reshedall file which contains the individual data for each university and pools the total of all other HED institutions in the last line, creating a new level, 'allother' that contains the totals of non University HED institutions. 

Replaces NAs with '0' - because I dont know how to deal with NAs. Hence this means I cannot distinguish when I know there are 0 people vs where the data is not available. 

Calculates the number and fte of researchers and techs from the percentage of funded eps and puts the values in reshedall. 

Identifies the difference between the number of research students in the two datasets: reshed and rdsurvey2016

Calculates total number of research workforce and adds it to reshedall.

## Chunk learners

calculates total number of learners per institution from the per year breakdown and adds it to a new column (into reshedall and rescri). 

## Chunk plot learners
Plots the total number of learners per institution as total and per year. 

## Chunk learner proportion
Plots the proportion of the workforce that is a 'learner' for different institutions | years.
For cris, it removes the line 'other' which includes learners from isntitutions that do not fall into either HED or CRI.

## Chunk user proportion


training data analysis

Licence:

Contributors:

