## Hello and welcome to a little introduction about R!

This is not meant to be a big introduction to R, but just one were you see commands you will use in the next session.
If you have never used R please find here a link to download it: 
https://www.dataquest.io/blog/installing-r-on-your-computer/ 

R as itself is a little bit ugly, so everyone uses Rstudio, here there is a link to download it: 
https://docs.oracle.com/en/database/oracle/machine-learning/oml4r/1.5.1/oread/installing-rstudio-desktop.html#GUID-CD347255-0A64-4A86-9710-0C6FE73D50DD

If you have already installed R, we need at least the version 4.3, here there is a link on how to update it: https://www.r-project.org/nosvn/pandoc/installr.html
- This has to be done in R and not in Rstudio!
  
In order to install properly packages, you will also need to install RTools: https://cran.r-project.org/bin/windows/Rtools/

Once we have everything downloaded, let's look at the terminal in RStudio!

Once you opened RStudio you should see a promp like this:
<img width="959" alt="image" src="https://github.com/Violeta-de-Anca/Toxicology_IBG_UU/assets/101873673/6be8c661-e90a-4944-8b86-6c4d0a0292cd">

Now we need to open a script so we can save all the commands in case we need to repeat them:
<img width="533" alt="image" src="https://github.com/Violeta-de-Anca/Toxicology_IBG_UU/assets/101873673/b568c1b7-316b-44fd-89c5-e8682af1ba71">

<img width="249" alt="image" src="https://github.com/Violeta-de-Anca/Toxicology_IBG_UU/assets/101873673/5ba90e9f-4c3f-4239-9589-8831a5e672b1">

After opening a script, we will need to set up the working directory:

![image](https://github.com/Violeta-de-Anca/Toxicology_IBG_UU/assets/101873673/20e6e789-6ead-4e34-a734-7e04908bcd49)

The working directory as to be where our dataset is.

## Let's install all the packages
Lot's of different files can be loaded into R, the most typical in Toxicology would be a .csv file, which is an excel file readable for R.

There are multiple commands to read data, one way which does not consume much RAM is `fread()`, from the package data.table. 
First thing you want to do is to install and load the necesary packages in R. In order to do that there are two functions: 

```diff
install.packages("name of the package")
library(name of the package)
#In out case it would be:
install.packages("data.table")
library(data.table)
```

Please remember to read the output when is installing the packages.
Now install these packages: tidyr, tidyselect, dplyr, ggplot2, viridis, gplots, qqman, data.table, diffdf, useful, stringr.

## Now we can start working on some dataset
Let's start by loading our example dataset to see what is it.

```diff
dataset = fread(input = "dataset_for_first_session.csv",header=T)
```

We see there are several columns, with different individuals and different treatments:
![image](https://github.com/user-attachments/assets/43650fb9-9ff1-41be-b909-f8e887c7f9de)

Let's subset the individual why their treatment:

```diff
subset=dataset[dataset$route_administered=="which ever type of treatment,]
```

You can play a little more with the data for the next session, please remember to do the pre-lab quizz, as it will give you the theoretical background necessary for understanding the data you are analyzing.










