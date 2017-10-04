# MappingKenya
AfriKenya
setwd("F:/Project")
library(maps)  
library(mapdata)
library(ggplot2)
library(RColorBrewer)
#Plotting the World Map
map('worldHires')
#Plotting African Map
map('worldHires', 
    xlim=c(-20,60),  # longitude
    ylim=c(-35,40),
    fill = TRUE,
    col = c("red", "blue", "green", "orange", "pink"))
title(main = "Simple Map of Africa")
#Plotting Kenyan Map
map('worldHires', "kenya")
#Population Plot in Kenya
keny<- read.csv("kenya.csv", header=T)
head(keny)
#Extracting Longitudes and Latitude.
kenya_map <- fortify(map("worldHires","kenya", fill=FALSE, plot=F))
attach(kenya_map)
#ggplot of Kenya map
ggplot() + geom_polygon(data = kenya_map, aes(x=long, y = lat, group = group)) + 
  coord_fixed(1.3)
#When fill is FALSE 
ggplot() + 
  geom_polygon(data = kenya_map, aes(x=long, y = lat, group = group), fill = NA, color = "red") + 
  coord_fixed(1.3)
#Selecting your color of choice
ggKen <- ggplot() + 
  geom_polygon(data = kenya_map, aes(x=long, y = lat, group = group), fill = "blue", color = "blue") + 
  coord_fixed(1.3)
ggKen
#Including the population points in the map
ggKen + geom_point(data = keny, aes(x = keny$lng, y = keny$lat), colour = "red") + 
  labs(title = "Population of Certain Towns in Kenya")
#Labelling the population points. 
dimkeny= data.frame(keny$population_proper, keny$lng, keny$lat)
colnames(dimkeny) = c('Pop Size','lon','lat')
ggKen + geom_point(aes(x=keny$lng, y=keny$lat, label=keny$city), data=keny,col="gold2",hjust=0.5, vjust=-0.5, size=4) + 
  scale_size_continuous(range=range(dimkeny$Pop size))
kenheat<- gg1 + geom_point( data= keny, aes(x=keny$lng, y=keny$lat, size = keny$population_proper), color="coral1") +
  scale_size(name="Population Map")+ scale_color_brewer(type = 'div', palette = 4, direction = 1)
kenheat<- kenheat + geom_text( data=keny, hjust=0.5, vjust=-0.5, aes(x=keny$lng, y=keny$lat, label=keny$city), colour="gold2", size=4 )
kenheat
#Other ways of ploting the kenya maps 
require(sp)
#Mapping Kenya 
ken0 <- readRDS("/Project/KEN_adm0.rds")
plot(ken0)
#Mapping the 47 counties 
ken1 <- readRDS("/Project/KEN_adm1.rds")
plot(ken1)
#Mapping districts
ken2 <- readRDS("/Project/KEN_adm2.rds")
plot(ken2)
#Mapping wards 
ken3 <- readRDS("/Project/KEN_adm3.rds")
plot(ken3)
#Extracting the names of the 47 counties
ken1$NAME_1

col <- rep(c("green","red","black", "orange"), 47)
col[36] <- "purple"
plot(ken1, col = col, border = 'blue')
title(main = "Simple Map of Africa")
