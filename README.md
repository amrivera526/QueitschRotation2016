# QueitschRotation2016
Notes to share with myself across devices.
plink --file mydata --r2


module load modules modules-init modules-gs
module load plink/latest
plink --file Dummy1 --map3 --r2

#remove header

tail -n +2 Dummy1.map > Dummy2.map
tail -n +2 Dummy1.ped > Dummy2.ped
plink --file Dummy2 --map3 --r2

#Skips ld due to lack of founders.Try with nonfounders tag
plink --file Dummy2 --nonfounders --map3 --r2

plink --file OnlineDummy --r2
plink --file OnlineDummy --r #Produces nan

#remember --allow-no-sex flag

plink --bfile name --recode --tab --out newfile
plink --bfile plink --recode --tab --out ReDummy

plink --file OnlineDummy --recode vcf --out DummyVCF

module load vcftools/latest

#vcftools not in cluster? 
plink --file OnlineDummy --ld-cutoff-r2 0.2 --r2 --out LDTestX   ##Doesn't work

##Try with R. Must load some fucking prerequisites.

module load mpc/latest

module load mpfr/latest

module load gmp/latest

module load gcc/4.9.1

module load R/latest

module load VCFtools/latest

plink --file mydata --r2


module load modules modules-init modules-gs
module load plink/latest
plink --file Dummy1 --map3 --r2

#remove header

tail -n +2 Dummy1.map > Dummy2.map
tail -n +2 Dummy1.ped > Dummy2.ped
plink --file Dummy2 --map3 --r2

#Skips ld due to lack of founders.Try with nonfounders tag
plink --file Dummy2 --nonfounders --map3 --r2

plink --file OnlineDummy --r2

#remember --allow-no-sex flag

plink --bfile name --recode --tab --out newfile
plink --bfile plink --recode --tab --out ReDummy

plink --file OnlineDummy --recode vcf --out DummyVCF

module load VCFtools/latest
vcftools --vcf DummyVCF.vcf --hap-r2 --out LDTest_onVCF1
##Error: Insufficient sites remain after filtering. Is this an inherent problem of dummy data.


##Try --const-fid in plink


plink --file Dummy4 --r2 --const-fid --out LDTest_20161006_2 
##This works.

#############R Code
getwd()
setwd("C:/Users/alber/Dropbox/Queistsch Rotation 2016")
##library(xlsx)

Annots<-read.csv("str_annots.csv")
genots<-read.xlsx("problem_mip_genotypes_081016_reform.xlsx",1)

#make MAP file
cms<-rep(0,nrow(Annots))
mapfile<-cbind(Annots[,c(2,1)],cms,Annots[,3])
mapfile$ID<-paste("STR",mapfile$ID,sep="_")


genots<-genots[order(genots$STR_ID),]
genots$STR_ID<-paste("STR",genots$STR_ID,sep="_")

genots2<-t(genots)

genots2<-data.frame(genots2)

pedstart<-zeros(nrow(genots2),6)


