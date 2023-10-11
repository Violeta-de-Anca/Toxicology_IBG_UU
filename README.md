# Toxicology_IBG_UU
Welcome to the github page for the joined course Toxicology from Uppsala University, in here you will have a guide for the third lab: absorption rate as a function of site of injection.
### Introduction
The aim of this lab is to study how the way of administration influences the absorption rate of ethanol. We have data from a group of rats which where divided and study either intravenous (i.v.), intraperitoneal (i.p.) or subcutaneous (s.c.) administration. Some rats were given 4-MP (an inhibitor of ADH) and ethanol and others ethanol only.
![image](https://github.com/Violeta-de-Anca/Toxicology_IBG_UU/assets/101873673/a82412e1-589f-4bab-ae59-5d46809f56e9)

#### The specific aims of this lab are
1.	For the students to know how to plot the plasma ethanol concentration as a function of time for each of the methods.
2.	For the students to know how to calculate the distribution volume of ethanol in the rats.
3.	For the students to know how to calculate and plot the body burden as a function of time.
4.	For the students to know how to calculate the initial absorption rate of ethanol in rats.
5.	For the students to know how to calculate the elimination rate.
   
You have a quizz with all the questions you need to answer by the end of the lab in Studium!
#### Wet lab theorical part
In order to analyze data we need to know how was the experimental setup.
After injecting and waiting the specific time points for all the individuals the blood samples are prepared. 
Several steps involved diluting the samples, here there is a figure that sumarizes all the different steps:
![image](https://github.com/Violeta-de-Anca/Toxicology_IBG_UU/assets/101873673/6630a560-e978-4c59-83ff-9d0f8d725009)

##### Ethanol solution sample
- The wet lab scientist made pure ethanol samples, to enable measurement of the actual ethanol concentration in the injection solution they dilute the stock ethanol solution 100 times, otherwise they treated the ethanol samples the same way as the samples containing blood.
##### Sample treatment
All samples (including the ethanol sample) are centrifuged at 2 500 rpm in the centrifuge (1.5 ml tubes) for 5 minutes. Transfer 0.9 ml supernatant to new tubes.
##### Determination of ethanol concentration
Depending on way of administration they mixed the samples differently in the cuvettes:
![image](https://github.com/Violeta-de-Anca/Toxicology_IBG_UU/assets/101873673/628c4dac-f171-46b6-a755-05126aa22a9e)

The scientist mixed the sample and the different master mix (not relevant for the data analysis) and measured the absorbency of the samples, at 340 nm in an spectrophotometer. So the raw data we are going to get is absorvance values.

#### Dry lab, data analysis
Now that we have an idea of what was the experimental setup let's start analyzing the data.

##### Loading data
Download the data from studium, then load the following modules:

**To install for the first time R package the command is: install.packages("name_of_the_package")**

**To load an R package the command is: library()**

- data.table

- tidyr

- tidyselect

- dplyr

Now that we have all the packages installed and loaded, let's load our dataset which is in Studium but also in this GitHub:

   `dataset = fread(input = "<your_path>/dataset_lab_3.csv",header=T)`


##### Plasma ethanol concentration as a function of time

Let’s start with plotting the plasma ethanol concentration as a function of time. We will need to use the Beer’s law:

$$
ΔA = \text{C} \times \text{l} \times \text{ε}
$$

$$  
l = 1 cm
$$

$$
ε = 6.22 mM/cm
$$
   
   
Where c is concentration, l is the cuvette length and ε is the absorptivity constant.

**Think about the dilution factor**

*(final volume/aliquote volume) * (final volume/aliquote volume)…**

Once you have the dilution factors we can apply beer's law to get the concentration:

`dataset$concentration=(dataset$delta_A*<dilution_factor>)/(ε*l)`

In order to have the two different dilution factors we can subset by row to overwrite:

`dataset$concentration[<rows>]=(dataset$delta_A[<rows>]*<dilution_factor>)/(ε*l)`

When we finally have the correct concentrations we can plot againts time to see the behaviour.

```diff
+ This is to subset each group
dataset.<subgroup>=dataset[dataset$type_intake=="<option between iv/ip/sc>"&dataset$`4-MP`=="<option between yes/no>",]
```

```diff
+ This is to plot a graph concentration vs time
y=plot(dataset.<subgroup>$concentration~dataset.<subgroup>$time_elapsed,xlab="Time (min)",ylab="Concentration mM)",main="Plasma ethanol concentration in <subgroup>",ylim=c(<this change between methods of injection>))
```

In order to be able to compare here are the limits you need to put for each of the injection method:
- For IV is c(0,10)
- For IP is c(0,25)
- For SC is c(0,25)

##### Volume of distribution ethanol

Volume of distribution is the theoretical value to represent the total volume where the total amount of an administered compound can be found. Now that we have the concentration plots let’s focus on the second objective, calculating the distribution volume of ethanol in the rats.

$$
Vd = \frac{\text{Concentration} \times \text{Volume}}{C_{\text{ss}}}
$$

Concentration is the pure ethanol concentration, the Volume is of ethanol injected in the protocol (FYI it varies with the method) and Css is the concentration at steady state.

One extra piece of information you need to have is the measurement in the spectrophotometer of pure ethanol:

$$
ΔA=0,481
$$

Careful, check again that the dilution factors you calculated apply for this one…

```diff
- First let's calculate the ethanol concentration that was administered with beer's law
EtOH=(0.481*<dilution factor>)/(6.22*1)
+ Then we are going to calculate the volume that was administered in L
vol_EtOH= (<volume of ethanol administered>*<transformation to L>)
@@ Lastly let's calculate the volume of distribution @@
volume_of_distribution.<subgroup>=((EtOH*vol_EtOH)/dataset.<group where we can find the Css>$concentration[<timepoint you think is correct>])/<mass of the rats>
```

##### Body burden as a function of time

The body burden of a substance is said to be the amount of the substance in the human body. With the volume of distribution we can calculate the body burden as a function of time.
The body burden is calculated in mmol/kg, the formula is:

$$
Db = \text{Vd} \times \text{C}
$$

Where Db is body burden, Vd is the distribution of volume and the C is the concentration of ethanol each time we measured it.

```diff
! Let's create a new column with the body burden
dataset.<subgroup>$body_burden=volume_of_distribution.<subgroup>*dataset.<subgroup>$concentration
+ Now we can make a plot of body burden vs time
plot(dataset.<subgroup>$body_burden~dataset.<subgroup>$time_elapsed,xlab="Time (min)",ylab="Body burden (mmol/kg*bw)",main="Body burden as function of\ntime in rats with <method you chose>")
```

##### Initial absorption rate (AR) of ethanol

With the slope from the body burden now we can calculate the rate of absorption. By mathematical properties we can know the speed by the plot just like this:

![image](https://github.com/Violeta-de-Anca/Toxicology_IBG_UU/assets/101873673/62b2a6b9-ca1b-4d7b-9e92-10b337abe8a2)

With the formula being:

$$
Ak = \frac{Db_{\text{2}} - Db_{\text{1}}}{Time_{\text{2}} - Time_{\text{1}}}
$$


