# [Rank-Serie-A-2021-Football-teams-in-R-with-the-ELO-RATING-SYSTEM](https://rpubs.com/FVR92/809530)
ELO RATING SYSTEM

I have now been with my girlfriend for nearly 4 or 5 years…time flies wright? Well lately i have begun to notice some little hints regarding a possible wedding…so one evening, at dinner, i have decided to talk her about it, and yes, she wouldn’t mind getting married. Long story short, i opted for a litle bet: “I’ll marry you if my football(soccer) team will win this year italian league!!” I am a big AS Roma supporter, and my team has not won anything in decades…still, she agreed. So this is where this project started; i love my girlfriend, and I also love data science, so why should i not use a litle help? Here comes the ELO RATING SYSTEM, i’ll use it to rank the Serie A teams, and maybe calculate some probabilities for a couple of matches.


asy enough, let's see what we'll need:

* Data, of course!
* Some tidying procedures
* A couple of R packages


To build the ELO Ranks, i have search the web for some historical results of the Serie A league, and put my hands on several dataframe starting from back in the 2000's.
I have allready cleaned and tidyed a bit the different dataframe with SQL, so we'll jump straight into R.

Load tidiverse, and get the data into R

```{r}
library("tidyverse")
serie_a_2020 <- read.csv("serie_a_2019_2020.csv")
serie_a_2019 <- read.csv("serie_a_2018_2019.csv")
serie_a_2018 <- read.csv("serie_a_2017_2018.csv")
serie_a_2017 <- read.csv("serie_a_2016_2017.csv")
serie_a_2016 <- read.csv("serie_a_2015_2016.csv")
serie_a_2015 <- read.csv("serie_a_2014_2015.csv")
serie_a_2014 <- read.csv("serie_a_2013_2014.csv")
serie_a_2013 <- read.csv("serie_a_2012_2013.csv")
serie_a_2012 <- read.csv("serie_a_2011_2012.csv")
serie_a_2011 <- read.csv("serie_a_2010_2011.csv")
serie_a_2010 <- read.csv("serie_a_2009_2010.csv")
serie_a_2009 <- read.csv("serie_a_2008_2009.csv")
serie_a_2008 <- read.csv("serie_a_2007_2008.csv")
serie_a_2007 <- read.csv("serie_a_2006_2007.csv")
serie_a_2006 <- read.csv("serie_a_2005_2006.csv")
serie_a_2005 <- read.csv("serie_a_2004_2005.csv")
serie_a_2004 <- read.csv("serie_a_2003_2004.csv")
serie_a_2003 <- read.csv("serie_a_2002_2003.csv")
serie_a_2002 <- read.csv("serie_a_2001_2002.csv")
serie_a_2001 <- read.csv("serie_a_2000_2001.csv")
serie_a_2000 <- read.csv("serie_a_1999_2000.csv")
```

Now select some variables of interest from the database, we'll need them later
```{r}
serie_a_2000<- serie_a_2000 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2001<- serie_a_2001 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2002<- serie_a_2002 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2003<- serie_a_2003 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2004<- serie_a_2004 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2005<- serie_a_2005 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2006<- serie_a_2006 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2007<- serie_a_2007 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2008<- serie_a_2008 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2009<- serie_a_2009 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2010<- serie_a_2010 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2011<- serie_a_2011 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2012<- serie_a_2012 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2013<- serie_a_2013 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2014<- serie_a_2014 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2015<- serie_a_2015 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2016<- serie_a_2016 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2017<- serie_a_2017 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
 serie_a_2018<- serie_a_2018 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2019<- serie_a_2019 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
serie_a_2020<- serie_a_2020 %>%
  select(Date, HomeTeam, AwayTeam, FTHG, FTAG, FTR, HTHG, HTAG, HTR)
```
Now we can merge the database into one (we can use rbind, because all dataframe has the same columns names and number)
```{r}
serie_a_total <- rbind(serie_a_2000, serie_a_2001, serie_a_2002,
                       serie_a_2003,serie_a_2004, serie_a_2005, serie_a_2006,
                       serie_a_2007, serie_a_2008, serie_a_2009, serie_a_2010,
                       serie_a_2011, serie_a_2012, serie_a_2013, serie_a_2014,
                       serie_a_2015, serie_a_2016, serie_a_2017, serie_a_2018,
                       serie_a_2019, serie_a_2020)
serie_a_total<-  serie_a_total %>%

arrange(desc(dmy(serie_a_total$Date)))
```  

Let's change some variable name as I still have trouble interpreting some of them
```{r}
serie_a_total <- serie_a_total %>%
  rename(FullTimeHomeTeamGoals= FTHG,
         FullTimeAwayTeamGoals= FTAG,
         FullTimeResult= FTR,
         HalfTimeHomeTeamGoals= HTHG,
         HalfTimeAwayTeamGoals= HTAG,
         HalfTimeResult= HTR)
```
Now the first thing to do, we have to create another dataframe to store each teams ELO RATING, and obviously update it after avery single match.
```{r}
serie_a_teams <- data.frame(team = unique(c(serie_a_total$HomeTeam, serie_a_total$AwayTeam)))
```

Ok, ready to move on! Before we begin playing with Elo ratings, we need to assign an initial Elo value to all of the Serie A Teams we have. We can set this value to 1200.
```{r}
serie_a_teams<- serie_a_teams %>%
   mutate(elo = 1200)
```
For each football game played, we’ll create a variable showing who won.
We'll set the variable values to:
#### 1 if the home team won
#### 0 if the away team won
#### 0.5 for a draw
```{r}
serie_a_total<- serie_a_total %>%
  mutate(GameResult = if_else(FullTimeHomeTeamGoals>FullTimeAwayTeamGoals,
  1,
  if_else(FullTimeHomeTeamGoals == FullTimeAwayTeamGoals, 0.5, 0)))
```
Now we install and load the most important package for our ELO RATING SYSTEM
```{r}
install.packages("elo")
library(elo)
```
The difficult part, we have to write our program. It won't be to difficult, and thanks to Edouard Mathiueu and elo CRAN package instruction everyone can easily understand a bit more.
We'll loop over every single game, get pre-match ratings and update them accordingly to our historical saved results.
```{r}
for (i in seq_len(nrow(serie_a_total))) {

  match <- serie_a_total[i, ]

  teamA_elo <- subset(serie_a_teams, team == match$HomeTeam)$elo

  teamB_elo <- subset(serie_a_teams, team == match$AwayTeam)$elo

  new_elo <- elo.calc(wins.A = match$GameResult,
                      elo.A = teamA_elo,
                      elo.B = teamB_elo,
                      k = 32)

  teamA_new_elo <- new_elo[1, 1]

  teamB_new_elo <- new_elo[1, 2]

  serie_a_teams <- serie_a_teams %>%

    mutate(elo = if_else(team == match$HomeTeam, teamA_new_elo,

     if_else(team == match$AwayTeam, teamB_new_elo, elo)))

 }

 Let's wait for R to run the code and then check it out!

serie_a_teams %>%
arrange(-elo) %>%
glimpse()
```
Now we can select only those team playing in the current 2021 Serie A league season
```{r}
serie_a_teams_2021 <- serie_a_teams %>%
  filter(team %in% c("Roma", "Milan","Napoli", "Inter", "Udinese","Bologna",
                     "Lazio", "Fiorentina", "Sassuolo", "Atalanta", "Torino",
                     "Empoli", "Genoa", "Venezia", "Sampdoria", "Juventus",
                     "Cagliari", "Spezia","Verona", "Salernitana")) %>%

arrange(-elo)

print.data.frame(serie_a_teams_2021)
```
![](https://github.com/FVR92/Rank-Serie-A-2021-Football-teams-in-R-with-the-ELO-RATING-SYSTEM/blob/main/immages/elorank_pic.JPG)


## Roma is only in the 6 raw.....well i guess I won't get married that soon will I?

Let's calculate probabilities for an individual game, like the first one of the season: AS ROMA VS Fiorentina. We'll use the elo.prob function
```{r}
Roma <- subset(serie_a_teams_2021, team == "Roma")$elo

Fiorentina <- subset(serie_a_teams_2021, team == "Fiorentina")$elo

elo.prob(Roma, Fiorentina)
```

![](https://github.com/FVR92/Rank-Serie-A-2021-Football-teams-in-R-with-the-ELO-RATING-SYSTEM/blob/main/immages/romafiorentina.JPG)

### Only 56% chance of winning...not bad
#### (AS ROMA actually won that match 3-1, in a very balanced match with one red card for each team)

# Latest Update: from when i decided to create this bet, AS ROMA has, in order: Signed Jose Mourinho as our coach + spent nearly 50.0000 pounds for a single player (most expensive agreement in our history) + won the first four games....should i look for a suite allready?
