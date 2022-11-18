# Heatmap for gene content of the bins

Saved the excel file as .csv with bins as columns and genes as rows and called it 

### Set working directory and loaded libraries
```
setwd("/Maryam/papers/R_scripts_for_Figures")
library(ggplot2)
library(reshape2)
```

### Uploaded my data to R
```
Bins_function <- read.csv("bins_function.csv", header = TRUE)
```

### Convert data frame from a "wide" format to a "long" format

```
xx = melt(Bins_function, id = c("Gene", "Category"))
```
### Plot the data

```
plot = ggplot(xx, aes(x = Gene, y = variable), shape ="carb") + geom_tile( aes(fill = Category, alpha = value), color = "black") + facet_grid( .~Category,scales = "free", space= "free", labeller = label_wrap_gen(width=12)) +
  theme(aspect.ratio = 1)+
  scale_fill_manual(values = c("dodgerblue4", "firebrick4", "aquamarine4", "darkgoldenrod4", "darkorchid4", "deeppink4")) +
  theme(axis.text.x = element_text(angle = 90, face = "bold", size =10, vjust = 0, hjust =0), axis.text.y = element_text(size = 12, face= "bold"), legend.text = element_text(size = 12, face = 'bold'), legend.title = element_text(size = 14, face= "bold"), legend.position = "right", panel.border = element_rect(fill = NA, colour = "black"), panel.background = element_blank()) +
  scale_x_discrete(position="top")+
  theme(strip.background = element_blank(), strip.text = element_blank()) +
  scale_alpha_continuous(range = c(0,1)) +
  labs(x = "", y ="")

plot
```

Colors and additional features were edited in Inkscape. One column was selected for each category and shapes inside the boxes was used for different genes