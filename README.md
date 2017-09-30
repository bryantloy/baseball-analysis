# baseball-analysis
Some baseball analysis I used while learning R
install.packages("dplyr")
install.packages("sqldf")
install.packages("readxl")
install.packages("xlsx")
library(ggplot2)
library(googleVis)
library(sqldf)
players = read.csv("C://Users/Bryant/Desktop/R/Master.csv")
batting = read.csv("C://Users/Bryant/Desktop/R/batting.csv")
salaries = read.csv("C://Users/Bryant/Desktop/R/Salaries.csv")
teams = read.csv("C://Users/Bryant/Desktop/R/Teams.csv")
batting_extra = merge(batting, players[,c("playerID","birthYear")])
x = batting_extra[which(batting_extra$AB > 200 & batting_extra$yearID >=1980), ]
x$age = x$yearID - x$birthYear
salaries_teams = merge(salaries, teams[,c("yearID", "teamID")])
HR_Leader = sqldf("SELECT yearID, playerID, MAX(CASE WHEN lgID = 'AL' THEN HR END) as AL_HRLeader, MAX(CASE WHEN lgID = 'NL' THEN HR END) as NL_HRLeader FROM batting_extra WHERE yearID >=1965 GROUP BY 1")
g = gvisLineChart(HR_Leader, xvar = "yearID", yvar = c("AL_HRLeader", "NL_HRLeader"), options = list(title = "HR Leader by Year by League"))
plot(g)
hist(HR_Leader$AL_HRLeader)
hist(HR_Leader$NL_HRLeader)
