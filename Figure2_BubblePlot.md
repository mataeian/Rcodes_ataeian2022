# Bubble Plot for relative abundance and protein expression

## Prepared .csv file 
![Abundance file](C:\Users\Maryam\Pictures\Genome and Proteome Abundance.png)

## Set working directory

```
setwd("/Users/Maryam/papers/R_scripts_for_Figures")
```




setwd("/Users/Maryam/OneDrive/PhD 2017/papers/Bioreactor_Genome_paper/Second_Metagenomics/Writing_Paper/Figures/R_scripts_for_Figures")

library(dplyr)
library(tidyr)
library(ggplot2)


#saved the excel file as csv called Abundance_GP

Abundance_GP <- read.table("Abundance_GP.csv", header = TRUE, sep = ",")
# R does not like numbers in the headers so it puts a X next to them. I changed the names so there is not header starting with a number.

#make a long table
Abundance_GP_long <- gather(Abundance_GP , Condition , Abundance, 7:14)


#remove zero
Abundance_GP_long_nozero <- filter(Abundance_GP_long, Abundance > 0)

# To split the condition column into two columns 
Abundance_GP_long_nozero_split <- separate(Abundance_GP_long_nozero, Condition, c("treatments","Bin_Protein"), sep = "_")

#combine two columns 

Abundance_GP_long_nozero_split$names <- paste(Abundance_GP_long_nozero_split$Bin, "_", Abundance_GP_long_nozero_split$Phyloflash_Taxon)
#test_long$names <- as.factor(test_long$names)


#This is just something I tried, but it is not right and I don't need it
#if(Abundance_GP_long_nozero_split$Taxon=="Cyanobacteria"){
#  Abundance_GP_long_nozero_split$Abundance_cyano=Abundance_GP_long_nozero_split$Abundance/10
#} else {Abundance_GP_long_nozero_split$Abundance_cyano=Abundance_GP_long_nozero_split$Abundance}


#set minimum vaules at 0.001, anything below that will be plotted at 0.001
#set cutoff value
cutoff = 0.001
Abundance_GP_long_nozero_split_below001 <- filter(Abundance_GP_long_nozero_split, Abundance < cutoff)
Abundance_GP_long_nozero_split_below001$Abundance_ADJ = cutoff
Abundance_GP_long_nozero_split_abv001 <- filter(Abundance_GP_long_nozero_split, Abundance > cutoff)
Abundance_GP_long_nozero_split_abv001$Abundance_ADJ = Abundance_GP_long_nozero_split_abv001$Abundance
Abundance_GP_long_nozero_split_adj2 <- bind_rows(Abundance_GP_long_nozero_split_abv001, Abundance_GP_long_nozero_split_below001)



##plot
#name_order <- c("24", "42", "3A", "39", "5", "1", "20", "6A", "53", "4A", "10A", "29", "17", "41", "34", "57", "12", "58", "50", "8A", "5A", "46", "40", "35", "7", "52", "23", "7A", "56")
#name_order <- c("56", "7A", "23", "52", "7", "35", "40", "46", "5A", "8A", "50", "58", "12", "57", "34", "41", "17", "29", "10A", "4A", "53", "6A", "20", "1", "5", "39", "3A", "42", "24")
name_order <- c("56", "7A", "41", "17", "57", "23", "5A", "50", "7", "35", "46", "40", "8A", "58", "52", "12", "34", "29", "42", "20", "6A", "4A", "3A", "53", "10A", "5", "1", "39", "24")

Treatment_order <- c("C", "T1", "T2", "T10")


plot<- ggplot(Abundance_GP_long_nozero_split_adj2, aes(x=treatments , y=Bin))

plot + geom_point(aes(size= Abundance_ADJ, colour= treatments)) +
  scale_y_discrete(limits = name_order) +
  scale_x_discrete(limits = Treatment_order) +
  scale_size(range = c(3,14), breaks = c(0.001, 0.01, 0.1, 1, 10), labels = c("0.001", "0.01", "0.1", "1", "10")) + 
  facet_grid(.~Bin_Protein)+
  theme(text = element_text(size = 14), panel.background = element_rect(fill = "white"), panel.grid.major = element_line(colour = "grey90")) 




  