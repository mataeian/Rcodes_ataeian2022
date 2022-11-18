# Stacked Bar Plot of the heterotrophic community structure 
SSU rRNA extracted from metagenomes using phylofalsh is used for this figure

Saved the excel file as .csv with taxonomies as columns and conditions as rows and called it PF_SSU_Combined_utf8.csv

### Set working directory and loaded libraries 

```
setwd("Maryam/papers/R_scripts_for_Figures")
install.packages("ggplot2")
install.packages("reshape2")
library(ggplot2)
library(reshape2)

```
### Uploaded my data to R
```

pc = read.csv("PF_SSU_Combined_utf8.csv", header = TRUE)

```

### Convert data frame from a "wide" format to a "long" format
```
pcm = melt(pc, id = c("Sample"))
```


### To keep the order of my samples
```
pcm$Sample <- factor(pcm$Sample,levels=unique(pcm$Sample))
```
### Plot the data

```
plot = ggplot(pcm, aes(x = Sample, fill = variable, y = value)) + 
  geom_bar(stat = "identity", colour = "black") + 
  theme(axis.text.x = element_text(angle = 90, size = 14, colour = "black", vjust = 0.5, hjust = 1, face= "bold"), 
        axis.title.y = element_text(size = 16, face = "bold"), legend.title = element_text(size = 16, face = "bold"), 
        legend.text = element_text(size = 12, face = "bold", colour = "black"), 
        axis.text.y = element_text(colour = "black", size = 12, face = "bold")) + 
  scale_y_continuous(expand = c(0,0)) + 
  labs(x = "", y = "Relative Abundance (%)", fill = "OTU") 
  #scale_fill_manual(values = colours)

plot

```
Colors and additional features are edited in Inkscape