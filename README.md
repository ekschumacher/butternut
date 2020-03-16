# butternut
Collection of all code for butternut
library(adegenet)
library(adegenet)
library(stringr)
library(tidyr)
library(hierfstat)
library(poppr)
library()
##load butternut document

##setwd("C:/Users/eschumacher/Documents/MortonArboretum/")
##personal computer
setwd("C:/Users/eksch/Documents/MortonArboretum")

butternutgen <- read.genepop("butternut_USCAN.gen", ncode = 3)

butternutpop <- genind2genpop(butternutgen)

##first identify if there were scoring discrepencies between Sean and Jeanne

loci <- c("B114","B159","WGA","A5_2","B157","B212_2","B121",	"B147",	"B249",	"B262","B264")

pdf(file=paste0("locibarplot.pdf"),width=40,height=9)

for(a in loci){
  
  new <- butternutpop[,which(grepl(a,colnames(butternutpop@tab)))]@tab
  
  barplot(new, las = 2, beside = TRUE, col = c("red", "blue"), legend.text =  c("Canada", "United States"))
  
}
dev.off()

##now remove individuals with greater than 25% missing data

butternutgen_nomd <- missingno(butternutgen, type = "geno", cutoff = 0.25, quiet = FALSE, freq = FALSE)

##then use this to create a dataframe

butternutgen_nomd_df <- genind2df(butternutgen_nomd, oneColPerAll = TRUE)

##which can be used in demerelate -- calculate relatedness of all individuals
butternutrel <- Demerelate(butternutrel_df, object = T, value = "loiselle")

##now identify how many individuals have greater than 25% relatedness = half siblings
butternut_halfsib_names <- names(which(unlist(butternutrel$Empirical_Relatedness) > 0.25))

##then use this to create a document which has all the unique individual numbers for every highly related individuals
butternut_halfsib_names_cleanfront <- gsub("^.*\\.","", butternut_halfsib_names)

butternut_halfsib_names_cleanback <- gsub("^.*\\_","", butternut_halfsib_names_cleanfront)

relate_ind_remove <- unique(butternut_halfsib_names_cleanback)

