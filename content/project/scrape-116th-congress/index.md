---
title: Code
summary: Using R to scrape tweets from members of the 116th Congress
tags:
  - R
  - Twitter
  - Web scraping
  
external_link: ""
date: "2020-01-03"


image: ""

links: ""

url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""

---

## Scraping Tweets from members of the 116th Congress. 

Putting together this code definitely took awhile, but hopefully someone will find it helpful.

## Code
```{r}
#---------------
#  MC Timelines and Stream
#--------------
library(twitteR)
library(remotes)
library(rtweet)
library(httpuv)
library(tm)
library(tidytext)
library(tidyverse)
library(ggplot2)
library(dplyr)
#Authentication
api_key <- ""
api_secret <- ""
access_token <- ""
access_token_secret <- ""
token <- create_token(
  app = "Congress_Gender",
  consumer_key = api_key,
  consumer_secret = api_secret,
  access_token = access_token,
  access_secret = access_token_secret
)
#Collect Timelines - RUN ON DESKTOP. computationally intensive... save in rproject to only need to do it once
  #md_timelines_senate
    #get data
dougjones <- get_timeline('@SenDougJones', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bennet <- get_timeline('@SenatorBennet', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
blumenthal <- get_timeline('@SenBlumenthal', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
murphey <- get_timeline('@SenMurphyOffice', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chriscoons <- get_timeline('@ChrisCoons', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cooper <- get_timeline('@SenatorCarper', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
brianschatz <- get_timeline('@SenBrianSchatz', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
durbin <- get_timeline('@SenatorDurbin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
markey <- get_timeline('@SenMarkey', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cardin <- get_timeline('@SenatorCardin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chrisvanhollen <- get_timeline('@ChrisVanHollen', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
garypeters <- get_timeline('@SenGaryPeters', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
booker <- get_timeline('@SenBooker', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
menedez <- get_timeline('@SenatorMenendez', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
martinheinrich <- get_timeline('@MartinHeinrich', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tomudall <- get_timeline('@SenatorTomUdall', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
schumer <- get_timeline('@SenSchumer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
sherrodbrown <- get_timeline('@SenSherrodBrown', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jeffmerkley <- get_timeline('@SenJeffMerkley', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ronwyden <- get_timeline('@RonWyden', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bobcasey <- get_timeline('@SenBobCasey', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
toomey <- get_timeline('@SenToomey', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jackreed <- get_timeline('@SenJackReed', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
whitehouse <- get_timeline('@SenWhitehouse', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
leahy <- get_timeline('@SenatorLeahy', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
sanders <- get_timeline('@SenSanders', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
warner <- get_timeline('@MarkWarner', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joemanchin <- get_timeline('@Sen_JoeManchin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
      #flatten data from json format to dataframe
df.dougjones <- data.frame(dougjones)
df.bennet <- data.frame(bennet)
df.blumenthal <- data.frame(blumenthal)
df.murphey <- data.frame(murphey)
df.chriscoons <- data.frame(chriscoons)
df.cooper <- data.frame(cooper)
df.brianschatz <- data.frame(brianschatz)
df.durbin <- data.frame(durbin)
df.markey <- data.frame(markey)
df.cardin <- data.frame(cardin)
df.chrisvanhollen <- data.frame(chrisvanhollen)
df.garypeters <- data.frame(garypeters)
df.booker <- data.frame(booker)
df.menedez <- data.frame(menedez)
df.martinheinrich <- data.frame(martinheinrich)
df.tomudall <- data.frame(tomudall)
df.schumer <- data.frame(schumer)
df.sherrodbrown <- data.frame(sherrodbrown)
df.jeffmerkley <- data.frame(jeffmerkley)
df.ronwyden <- data.frame(ronwyden)
df.bobcasey <- data.frame(bobcasey)
df.toomey <- data.frame(toomey)
df.jackreed <- data.frame(jackreed)
df.whitehouse <- data.frame(whitehouse)
df.leahy <- data.frame(leahy)
df.sanders <- data.frame(sanders)
df.warner <- data.frame(warner)
df.joemanchin <- data.frame(joemanchin)
    #combine the dataframes into one table for Male Democrats in the Senate
df.senatemd <- bind_rows(
  df.dougjones %>%
    select(
      text, screen_name, created_at),
  df.bennet %>%
    select(
      text,screen_name, created_at),
  df.blumenthal %>%
    select(
      text,screen_name,created_at),
  df.murphey %>%
    select(text,screen_name,created_at),
  df.chriscoons %>%
    select(text,screen_name,created_at),
  df.cooper %>%
    select(text,screen_name,created_at),
  df.brianschatz %>%
    select(text,screen_name, created_at),
  df.durbin %>%
    select(text,screen_name, created_at),
  df.markey %>%
    select(text,screen_name, created_at),
  df.cardin %>%
    select(text,screen_name, created_at),
  df.chrisvanhollen %>%
    select(text,screen_name, created_at),
  df.garypeters %>%
    select(text,screen_name, created_at),
  df.booker %>%
    select(text,screen_name, created_at),
  df.menedez %>%
    select(text,screen_name, created_at),
  df.martinheinrich %>%
    select(text,screen_name, created_at),
  df.tomudall %>%
    select(text, screen_name,created_at),
  df.schumer %>%
    select(text,screen_name,created_at),
  df.sherrodbrown %>%
    select(text,screen_name,created_at),
  df.jeffmerkley %>%
    select(text,screen_name,created_at),
  df.ronwyden %>%
    select(text,screen_name,created_at),
  df.bobcasey %>%
    select(text,screen_name,created_at),
  df.toomey %>%
    select(text,screen_name,created_at),
  df.jackreed %>%
    select(text,screen_name,created_at),
  df.whitehouse %>%
    select(text,screen_name, created_at),
  df.leahy %>%
    select(text,screen_name, created_at),
  df.sanders %>%
    select(text,screen_name,created_at),
  df.warner %>%
    select(text,screen_name,created_at),
  df.joemanchin %>%
    select(text,screen_name,created_at)
)
    #clean dataframe to be one word per row
replace_reg <- "http[s]?://[A-Za-z\\d/\\.]+|&amp;|&lt;|&gt;"
unnest_reg  <- "([^A-Za-z_\\d#@']|'(?![A-Za-z_\\d#@]))"
df.senatemd.tidy <- df.senatemd %>%
  filter(!str_detect(text, "^RT")) %>%
  mutate(text = str_replace_all(text,replace_reg, "")) %>%
  mutate(id = row_number()) %>%
  unnest_tokens(
    word, text, token = "regex", pattern = unnest_reg) %>%
  filter(!word %in% stop_words$word, str_detect(word, "[a-z]"))




#Female Democrats in the Senate
sinema <- get_timeline('@SenatorSinema', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
feinstein <- get_timeline('@SenFeinstein', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kamalaharris <- get_timeline('@SenKamalaHarris', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
maziehirono <- get_timeline('@maziehirono', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
duckworth <- get_timeline('@SenDuckworth', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
warren <- get_timeline('@SenWarren', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stabenow <- get_timeline('@SenStabenow', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
amyklobuchar <- get_timeline('@SenAmyKlobuchar', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tinasmith <- get_timeline('@SenTinaSmith', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hassan <- get_timeline('@SenatorHassan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
shaheen <- get_timeline('@SenatorShaheen', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cortezmasto <- get_timeline('@SenCortezMasto', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jackyrosen <- get_timeline(' @SenJackyRosen', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gillibrand <- get_timeline('@gillibrandny', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cantwell <- get_timeline('@SenatorCantwell', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
pattymurray <- get_timeline('@PattyMurray', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
baldwin <- get_timeline('@SenatorBaldwin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
  #convert to dataframe
df.feinstein <- data.frame(feinstein)
df.kamalaharris <- data.frame(kamalaharris)
df.maziehirono <- data.frame(maziehirono)
df.duckworth <- data.frame(duckworth)
df.warren <- data.frame(warren)
df.stabenow <- data.frame(stabenow)
df.amyklobuchar <- data.frame(amyklobuchar)
df.tinasmith <- data.frame(tinasmith)
df.hassan <- data.frame(hassan)
df.shaheen <- data.frame(shaheen)
df.cortezmasto <- data.frame(cortezmasto)
df.jackyrosen <- data.frame(jackyrosen)
df.gillibrand <- data.frame(gillibrand)
df.cantwell <- data.frame(cantwell)
df.pattymurray <- data.frame(pattymurray)
df.baldwin <- data.frame(baldwin)
  #combine the dataframes
df.senatefd <- bind_rows(
  df.feinstein %>%
    select(
      text, screen_name, created_at),
  df.kamalaharris %>%
    select(text,screen_name, created_at),
  df.maziehirono %>%
    select(text,screen_name,created_at),
  df.duckworth %>%
    select(text,screen_name, created_at),
  df.warren %>%
    select(text,screen_name,created_at),
  df.stabenow %>%
    select(text,screen_name,created_at),
  df.amyklobuchar %>%
    select(text,screen_name, created_at),
  df.tinasmith %>%
    select(text,screen_name,created_at),
  df.hassan %>%
    select(text,screen_name,created_at),
  df.shaheen %>%
    select(text,screen_name, created_at),
  df.cortezmasto %>%
    select(text,screen_name,created_at),
  df.jackyrosen %>%
    select(text,screen_name,created_at),
  df.gillibrand %>%
    select(text, screen_name, created_at),
  df.cantwell %>%
    select(text,screen_name,created_at),
  df.pattymurray %>%
    select(text,screen_name, created_at),
  df.baldwin %>%
    select(text,screen_name,created_at))
  #clean dataframe to be one word per row
replace_reg <- "http[s]?://[A-Za-z\\d/\\.]+|&amp;|&lt;|&gt;"
unnest_reg  <- "([^A-Za-z_\\d#@']|'(?![A-Za-z_\\d#@]))"
df.senatefd.tidy <- df.senatefd %>%
  filter(!str_detect(text, "^RT")) %>%
  mutate(text = str_replace_all(text,replace_reg, "")) %>%
  mutate(id = row_number()) %>%
  unnest_tokens(
    word, text, token = "regex", pattern = unnest_reg) %>%
  filter(!word %in% stop_words$word, str_detect(word, "[a-z]"))



#Female Republican Senate Tweets
lisamurkowski<- get_timeline('@lisamurkowski', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mcsally <- get_timeline('@SenMcSallyAZ', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joniernst <- get_timeline('@SenJoniErnst', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
collins <- get_timeline('@SenatorCollins', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hydesmith <- get_timeline('@SenHydeSmith', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
fischer <- get_timeline('@SenatorFischer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
marshablackburn <- get_timeline('@MarshaBlackburn', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
capito <- get_timeline('@SenCapito', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)

df.lisamurkowski <- data.frame(lisamurkowski)
df.mcsally <- data.frame(mcsally)
df.joniernst <- data.frame(joniernst)
df.collins <- data.frame(collins)
df.hydesmith <- data.frame(hydesmith)
df.fischer <- data.frame(fischer)
df.marshablackburn <- data.frame(marshablackburn)
df.capito <- data.frame(capito)
  #turn it all into one dataframe
df.senatefr <- bind_rows(
  df.lisamurkowski %>%
    select(
      text, screen_name, created_at),
  df.mcsally %>%
    select(text,screen_name,created_at),
  df.joniernst %>%
    select(text,screen_name,created_at),
  df.collins %>%
    select(text,screen_name,created_at),
  df.hydesmith %>%
    select(text,screen_name,created_at),
  df.fischer %>%
    select(text,screen_name,created_at),
  df.marshablackburn %>%
    select(text,screen_name,created_at),
  df.capito %>%
    select(text,screen_name,created_at))
  #turn dataframe into one word per row
replace_reg <- "http[s]?://[A-Za-z\\d/\\.]+|&amp;|&lt;|&gt;"
unnest_reg  <- "([^A-Za-z_\\d#@']|'(?![A-Za-z_\\d#@]))"
df.senatefr.tidy <- df.senatefr %>%
  filter(!str_detect(text, "^RT")) %>%
  mutate(text = str_replace_all(text,replace_reg, "")) %>%
  mutate(id = row_number()) %>%
  unnest_tokens(
    word, text, token = "regex", pattern = unnest_reg) %>%
  filter(!word %in% stop_words$word, str_detect(word, "[a-z]"))




#Male Republicans in the Senate Tweets
donsullivan <- get_timeline('@SenDanSullivan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
shelby <- get_timeline('@SenShelby', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnboozman <- get_timeline('@JohnBoozman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tomcotton <- get_timeline('@SenTomCotton', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
corygardner <- get_timeline('@SenCoryGardner', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rubio <- get_timeline('@SenRubioPress', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rickscott<- get_timeline('@SenRickScott', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
isakson <- get_timeline('@SenatorIsakson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davidperdue <- get_timeline('@sendavidperdue', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chuckgrassley <- get_timeline('@ChuckGrassley', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikecrapo <- get_timeline('@MikeCrapo', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
risch <- get_timeline('@SenatorRisch', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
braun<- get_timeline('@SenatorBraun', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
toddyoung<- get_timeline('@SenToddYoung', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jerrymoran<- get_timeline('@JerryMoran', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
patroberts<- get_timeline('@SenPatRoberts', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mitchmconnell<- get_timeline('@SenateMajLdr', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
randpaul<- get_timeline('@RandPaul', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
billcassidy<- get_timeline('@SenBillCassidy', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnkennedy<- get_timeline('@SenJohnKennedy', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
royblunt<- get_timeline('@RoyBlunt', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hawley<- get_timeline('@SenHawleyPress', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
wicker<- get_timeline('@SenatorWicker', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stevedaines<- get_timeline('@SteveDaines', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
burr<- get_timeline('@SenatorBurr', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
thomtillis<- get_timeline('@SenThomTillis', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kevincramer<- get_timeline('@@SenKevinCramer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnhoeven<- get_timeline('@SenJohnHoeven', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
sasse<- get_timeline('@SenSasse', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
robportman<- get_timeline('@senrobportman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jiminhofe<- get_timeline('@JimInhofe', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lankford<- get_timeline('@SenatorLankford', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lindseygraham<- get_timeline('@LindseyGrahamSC', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
timscott<- get_timeline('@SenatorTimScott', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rounds<- get_timeline('@SenatorRounds', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnthune<- get_timeline('@SenJohnThune', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
alexander<- get_timeline('@SenAlexander', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johncornyn<- get_timeline('@JohnCornyn', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tedcruz<- get_timeline('@SenTedCruz', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikelee<- get_timeline('@SenMikeLee', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
romney<- get_timeline('@SenatorRomney', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ronjohnson<- get_timeline('@SenRonJohnson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnbarrasso<- get_timeline('@SenJohnBarrasso', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
enzi<- get_timeline('@SenatorEnzi', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)

df.donsullivan <- data.frame(donsullivan)
df.shelby <- data.frame(shelby)
df.johnboozman <- data.frame(johnboozman)
df.tomcotton <- data.frame(tomcotton)
df.corygarnder <- data.frame(corygardner)
df.rubio <- data.frame(rubio)
df.rickscott <- data.frame(rickscott)
df.isakson <- data.frame(isakson)
df.davidperdue <- data.frame(davidperdue)
df.chuckgrassley <- data.frame(chuckgrassley)
df.mikecrapo <- data.frame(mikecrapo)
df.risch <- data.frame(risch)
df.braun <- data.frame(braun)
df.toddyoung <- data.frame(toddyoung)
df.jerrymoran <- data.frame(jerrymoran)
df.patroberts <- data.frame(patroberts)
df.mitchmcconnell <- data.frame(mitchmconnell)
df.randpaul <- data.frame(randpaul)
df.billcassidy <- data.frame(billcassidy)
df.johnkennedy <- data.frame(johnkennedy)
df.royblunt <- data.frame(royblunt)
df.hawley <- data.frame(hawley)
df.wicker <- data.frame(wicker)
df.stevedaines <- data.frame(stevedaines)
df.burr <- data.frame(burr)
df.thomtillis <- data.frame(thomtillis)
df.kevincramer <- data.frame(kevincramer)
df.johnhoeven <- data.frame(johnhoeven)
df.sasse <- data.frame(sasse)
df.robportman <- data.frame(robportman)
df.jiminhofe <- data.frame(jiminhofe)
df.lankford <- data.frame(lankford)
df.lindseygraham <- data.frame(lindseygraham)
df.timscott <- data.frame(timscott)
df.rounds <- data.frame(rounds)
df.johnthune <- data.frame(johnthune)
df.alexander <- data.frame(alexander)
df.johncornyn <- data.frame(johncornyn)
df.tedcruz <- data.frame(tedcruz)
df.mikelee <- data.frame(mikelee)
df.romney <- data.frame(romney)
df.ronjohnson <- data.frame(ronjohnson)
df.johnbarrasso <- data.frame(johnbarrasso)
df.enzi <- data.frame(enzi)

df.senatemr <- bind_rows(
  df.donsullivan %>%
    select(
      text, screen_name, created_at),
  df.shelby %>%
    select(text,screen_name,created_at),
  df.johnboozman %>%
    select(text,screen_name, created_at),
  df.tomcotton %>%
    select(text,screen_name,created_at),
  df.corygarnder %>%
    select(text,screen_name,created_at),
  df.rubio%>%
    select(text,screen_name,created_at),
  df.rickscott %>%
    select(text,screen_name,created_at),
  df.isakson %>%
    select(text,screen_name,created_at),
  df.davidperdue %>%
    select(text,screen_name,created_at),
  df.chuckgrassley %>%
    select(text,screen_name,created_at),
  df.mikecrapo %>%
    select(text,screen_name,created_at),
  df.risch %>%
    select(text,screen_name,created_at),
  df.braun %>%
    select(text,screen_name,created_at),
  df.toddyoung %>%
    select(text,screen_name,created_at),
  df.jerrymoran %>%
    select(text,screen_name,created_at),
  df.patroberts %>%
    select(text,screen_name,created_at),
  df.mitchmcconnell %>%
    select(text,screen_name,created_at),
  df.randpaul %>%
    select(text,screen_name,created_at),
  df.billcassidy %>%
    select(text,screen_name,created_at),
  df.johnkennedy %>%
    select(text,screen_name,created_at),
  df.royblunt %>%
    select(text,screen_name,created_at),
  df.hawley %>%
    select(text,screen_name,created_at),
  df.wicker %>%
    select(text,screen_name,created_at),
  df.stevedaines %>%
    select(text,screen_name,created_at),
  df.burr %>%
    select(text,screen_name,created_at),
  df.thomtillis %>%
    select(text,screen_name,created_at),
  df.kevincramer %>%
    select(text,screen_name,created_at),
  df.johnhoeven %>%
    select(text,screen_name,created_at),
  df.sasse %>%
    select(text,screen_name,created_at),
  df.robportman %>%
    select(text,screen_name,created_at),
  df.jiminhofe %>%
    select(text,screen_name,created_at),
  df.lankford %>%
    select(text,screen_name,created_at),
  df.lindseygraham %>%
    select(text,screen_name,created_at),
  df.timscott %>%
    select(text,screen_name,created_at),
  df.rounds %>%
    select(text,screen_name,created_at),
  df.johnthune %>%
    select(text,screen_name,created_at),
  df.alexander %>%
    select(text,screen_name,created_at),
  df.johncornyn %>%
    select(text,screen_name,created_at),
  df.tedcruz %>%
    select(text,screen_name,created_at),
  df.mikelee %>%
    select(text,screen_name,created_at),
  df.romney %>%
    select(text,screen_name,created_at),
  df.ronjohnson %>%
    select(text,screen_name,created_at),
  df.johnbarrasso %>%
    select(text,screen_name,created_at),
  df.enzi %>%
    select(text,screen_name,created_at))
  
replace_reg <- "http[s]?://[A-Za-z\\d/\\.]+|&amp;|&lt;|&gt;"
unnest_reg  <- "([^A-Za-z_\\d#@']|'(?![A-Za-z_\\d#@]))"
df.senatemr.tidy <- df.senatemr %>%
  filter(!str_detect(text, "^RT")) %>%
  mutate(text = str_replace_all(text,replace_reg, "")) %>%
  mutate(id = row_number()) %>%
  unnest_tokens(
    word, text, token = "regex", pattern = unnest_reg) %>%
  filter(!word %in% stop_words$word, str_detect(word, "[a-z]"))



######################################################
# HOUSE OF REPS TIMELINES
####################################################

#female republican HOR

martharoby <- get_timeline('@RepMarthaRoby', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
walorski <- get_timeline('@RepWalorski', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
susanbrooks <- get_timeline('@SusanWBrooks', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
annwagner <- get_timeline('@RepAnnWagner', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hartzler <- get_timeline('@RepHartzler', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
virginiafoxx <- get_timeline('@virginiafoxx', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
armstrong <- get_timeline('@RepArmstrongND', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stefanik <- get_timeline('@RepStefanik', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cathymorris <- get_timeline('@cathymcmorris', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lizcheney<- get_timeline('@RepLizCheney', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)


df.martharoby <- data.frame(martharoby)
df.walorski <- data.frame(walorski)
df.susanbrooks <- data.frame(susanbrooks)
df.annwagner <- data.frame(annwagner)
df.hartzler <- data.frame(hartzler)
df.virginiafoxx <- data.frame(virginiafoxx)
df.armstrong <- data.frame(armstrong)
df.stefanik <- data.frame(stefanik)
df.cathymorris <- data.frame(cathymorris)
df.lizcheney <- data.frame(lizcheney)

df.horfr <- bind_rows(
  df.martharoby %>%
    select(
      text, screen_name, created_at),
  df.walorski %>%
    select(text,screen_name,created_at),
  df.susanbrooks %>%
    select(text,screen_name,created_at),
  df.annwagner %>%
    select(text,screen_name,created_at),
  df.hartzler %>%
    select(text,screen_name,created_at),
  df.virginiafoxx %>%
    select(text,screen_name,created_at),
  df.armstrong %>%
    select(text,screen_name,created_at),
  df.stefanik %>%
    select(text,screen_name,created_at),
  df.cathymorris %>%
    select(text,screen_name,created_at),
  df.lizcheney %>%
    select(text,screen_name,created_at))

replace_reg <- "http[s]?://[A-Za-z\\d/\\.]+|&amp;|&lt;|&gt;"
unnest_reg  <- "([^A-Za-z_\\d#@']|'(?![A-Za-z_\\d#@]))"
df.horfr.tidy<- df.horfr %>%
  filter(!str_detect(text, "^RT")) %>%
  mutate(text = str_replace_all(text,replace_reg, "")) %>%
  mutate(id = row_number()) %>%
  unnest_tokens(
    word, text, token = "regex", pattern = unnest_reg) %>%
  filter(!word %in% stop_words$word, str_detect(word, "[a-z]"))




#Female Democrats HOR
terrisewell <- get_timeline('@RepTerriSewell', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kirkpatrick<- get_timeline('@RepKirkpatrick', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dorismatsui<- get_timeline('@DorisMatsui', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bera<- get_timeline('@RepBera', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
pelosi <- get_timeline('@SpeakerPelosi', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
barbaralee <- get_timeline('@RepBarbaraLee', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
speier <- get_timeline('@RepSpeier', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
annaeshoo <- get_timeline('@RepAnnaEshoo', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
zoelofgren <- get_timeline('@RepZoeLofgren', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
katiehill <- get_timeline('@RepKatieHill', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
brownley <- get_timeline('@RepBrownley', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
judychu <- get_timeline('@RepJudyChu', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gracenapolitano <- get_timeline('@gracenapolitano', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
normatorres <- get_timeline('@NormaJTorres', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
karenbass <- get_timeline('@RepKarenBass', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lindasanchez <- get_timeline('@RepLindaSanchez', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
roybalallard<- get_timeline('@RepRoybalAllard', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
maxinewaters<- get_timeline('@RepMaxineWaters', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
barragan<- get_timeline('@RepBarragan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
katieporter <- get_timeline('@RepKatiePorter', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
susandavis <- get_timeline('@RepSusanDavis', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dianadegette <- get_timeline('@RepDianaDeGette', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rosadelauro<- get_timeline('@rosadelauro', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jahanahayes<- get_timeline('@RepJahanaHayes', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lbr<- get_timeline('@RepLBR', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
eleanornorton<- get_timeline('@EleanorNorton', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stephmurphy<- get_timeline('@RepStephMurphy', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kcastor<- get_timeline('@USRepKCastor', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hastings <- get_timeline('@RepHastingsFL', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
loisfrankel <- get_timeline('@RepLoisFrankel', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dwstweets <- get_timeline('@RepDWStweets', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
wilson <- get_timeline('@RepWilson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dmp <- get_timeline('@RepDMP', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
shalala <- get_timeline('@RepShalala', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tulsi <- get_timeline('@TulsiPress', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
finkenauer <- get_timeline('@RepFinkenauer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cindyaxne <- get_timeline('@RepCindyAxne', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
robinkelly <- get_timeline('@RepRobinKelly', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
janschakowsky <- get_timeline('@janschakowsky', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
underwood <- get_timeline('@RepUnderwood', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cheri <- get_timeline('@RepCheri', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davids <- get_timeline('@RepDavids', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
loritrahan <- get_timeline('@RepLoriTrahan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kclark <- get_timeline('@RepKClark', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
pressley <- get_timeline('@RepPressley', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
haleystevens <- get_timeline('@RepHaleyStevens', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
debdingell <- get_timeline('@RepDebDingell', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rashida <- get_timeline('@RepRashida', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lawrence <- get_timeline('@RepLawrence', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
angiecraig <- get_timeline('@RepAngieCraig', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
deanphillips <- get_timeline('@RepDeanPhillips', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bettymccollum <- get_timeline('@BettyMcCollum04', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ilhan<- get_timeline('@Ilhan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lacyclay <- get_timeline('@LacyClayMO1', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
adams <- get_timeline('@RepAdams', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
anniekuster <- get_timeline('@RepAnnieKuster', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bonnie<- get_timeline('@RepBonnie', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
debhaaland <- get_timeline('@RepDebHaaland', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
torressmall <- get_timeline('@RepTorresSmall', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dinatitus<- get_timeline('@repdinatitus', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
susielee <- get_timeline('@RepSusieLee', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kathleenrice <- get_timeline('@RepKathleenRice', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
nydiavelazquez<- get_timeline('@NydiaVelazquez', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
yvetteclarke<- get_timeline('@RepYvetteClarke', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
maloney<- get_timeline('@RepMaloney', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
aoc <- get_timeline('@RepAOC', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
nitalowey <- get_timeline('@NitaLowey', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
beatty <- get_timeline('@RepBeatty', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
marcykaptur <- get_timeline('@RepMarcyKaptur', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
marciafudge<- get_timeline('@RepMarciaFudge', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kendrahorn<- get_timeline('@RepKendraHorn', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bonamici <- get_timeline('@RepBonamici', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mgs <- get_timeline('@RepMGS', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dean <- get_timeline('@RepDean', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
houlahan <- get_timeline('@RepHoulahan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
susanwild <- get_timeline('@RepSusanWild', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
fletcher <- get_timeline('@RepFletcher', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
escobar <- get_timeline('@RepEscobar', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jacksonlee <- get_timeline('@JacksonLeeTX18', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
sylviagarcia <- get_timeline('@RepSylviaGarcia', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
elaineluria<- get_timeline('@RepElaineLuria', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
spanberger<- get_timeline('@RepSpanberger', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
wexton <- get_timeline('@RepWexton', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
delbene <- get_timeline('@RepDelBene', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
herrerabeutler <- get_timeline('@HerreraBeutler', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jayapal<- get_timeline('@RepJayapal', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kimschrier <- get_timeline('@RepKimSchrier', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gwenmoore <- get_timeline('@RepGwenMoore', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)


df.terrisewell <- data.frame(terrisewell)
df.kirkpatrick <- data.frame(kirkpatrick)
df.dorismatsui <- data.frame(dorismatsui)
df.bera <- data.frame(bera)
df.pelosi <- data.frame(pelosi)
df.barbaralee <- data.frame(barbaralee)
df.speier <- data.frame(speier)
df.annaeshoo <- data.frame(annaeshoo)
df.zoelofgren <- data.frame(zoelofgren)
df.katiehill <- data.frame(katiehill)
df.brownley <- data.frame(brownley)
df.judychu <- data.frame(judychu)
df.gracenapolitano <- data.frame(gracenapolitano)
df.normatorres <- data.frame(normatorres)
df.karenbass <- data.frame(karenbass)
df.lindasanchez <- data.frame(lindasanchez)
df.roybalallard <- data.frame(roybalallard)
df.maxinewaters <- data.frame(maxinewaters)
df.barragan <- data.frame(barragan)
df.katieporter <- data.frame(katieporter)
df.susandavis <- data.frame(susandavis)
df.dianadegette <- data.frame(dianadegette)
df.rosadelauro <- data.frame(rosadelauro)
df.jahanahayes <- data.frame(jahanahayes)
df.lbr <- data.frame(lbr)
df.eleanornorton <- data.frame(eleanornorton)
df.stephmurphy <- data.frame(stephmurphy)
df.kcastor <- data.frame(kcastor)
df.hastings <- data.frame(hastings)
df.loisfrankel <- data.frame(loisfrankel)
df.dwstweets <- data.frame(dwstweets)
df.wilson <- data.frame(wilson)
df.dmp <- data.frame(dmp)
df.shalala <- data.frame(shalala)
df.tulsi <- data.frame(tulsi)
df.finkenauer <- data.frame(finkenauer)
df.cindyaxne <- data.frame(cindyaxne)
df.robinkelly <- data.frame(robinkelly)
df.janschakowsky <- data.frame(janschakowsky)
df.underwood <- data.frame(underwood)
df.cheri <- data.frame(cheri)
df.davids <- data.frame(davids)
df.loritrahan <- data.frame(loritrahan)
df.kclark <- data.frame(kclark)
df.pressley <- data.frame(pressley)
df.haleystevens <- data.frame(haleystevens)
df.debdingell <- data.frame(debdingell)
df.rashida <- data.frame(rashida)
df.lawrence <- data.frame(lawrence)
df.angiecraig <- data.frame(angiecraig)
df.deanphillips <- data.frame(deanphillips)
df.bettymccollum <- data.frame(bettymccollum)
df.ilhan <- data.frame(ilhan)
df.lacyclay <- data.frame(lacyclay)
df.adams <- data.frame(adams)
df.anniekuster <- data.frame(anniekuster)
df.bonnie <- data.frame(bonnie)
df.debhaaland <- data.frame(debhaaland)
df.torressmall <- data.frame(torressmall)
df.dinatitus <- data.frame(dinatitus)
df.susielee <- data.frame(susielee)
df.kathleenrice <- data.frame(kathleenrice)
df.nydiavelazquez <- data.frame(nydiavelazquez)
df.yvetteclarke <- data.frame(yvetteclarke)
df.maloney <- data.frame(maloney)
df.aoc <- data.frame(aoc)
df.nitalowey <- data.frame(nitalowey)
df.beatty <- data.frame(beatty)
df.marcykaptur <- data.frame(marcykaptur)
df.marciafudge <- data.frame(marciafudge)
df.kendrahorn <- data.frame(kendrahorn)
df.bonamici <- data.frame(bonamici)
df.mgs <- data.frame(mgs)
df.dean <- data.frame(dean)
df.houlahan <- data.frame(houlahan)
df.susanwild <- data.frame(susanwild)
df.fletcher <- data.frame(fletcher)
df.escobar <- data.frame(escobar)
df.jacksonlee <- data.frame(jacksonlee)
df.sylviagarcia <- data.frame(sylviagarcia)
df.elaineluria <- data.frame(elaineluria)
df.spanberger <- data.frame(spanberger)
df.wexton <- data.frame(wexton)
df.delbene <- data.frame(delbene)
df.herrerabeutler <- data.frame(herrerabeutler)
df.jayapal <- data.frame(jayapal)
df.kimschrier <- data.frame(kimschrier)
df.gwenmoore <- data.frame(gwenmoore)

df.horfd <- bind_rows(
  df.terrisewell %>%
    select(text,screen_name,created_at),
  df.kirkpatrick %>%
    select(text,screen_name,created_at),
  df.dorismatsui %>%
    select(text,screen_name,created_at),
  df.bera %>%
    select(text,screen_name,created_at),
  df.pelosi %>%
    select(text,screen_name,created_at),
  df.barbaralee %>%
    select(text,screen_name,created_at),
  df.speier %>%
    select(text,screen_name,created_at),
  df.annaeshoo %>%
    select(text,screen_name,created_at),
  df.zoelofgren %>%
    select(text,screen_name,created_at),
  df.katiehill %>%
    select(text,screen_name,created_at),
  df.brownley %>%
    select(text,screen_name,created_at),
  df.judychu %>%
    select(text,screen_name,created_at),
  df.gracenapolitano %>%
    select(text,screen_name,created_at),
  df.normatorres %>%
    select(text,screen_name,created_at),
  df.karenbass %>%
    select(text,screen_name,created_at),
  df.lindasanchez %>%
    select(text,screen_name,created_at),
  df.roybalallard %>%
    select(text,screen_name,created_at),
  df.maxinewaters %>%
    select(text,screen_name,created_at),
  df.barragan %>%
    select(text,screen_name,created_at),
  df.katieporter %>%
    select(text,screen_name,created_at),
  df.susandavis %>%
    select(text,screen_name,created_at),
  df.dianadegette %>%
    select(text,screen_name,created_at),
  df.rosadelauro %>%
    select(text,screen_name,created_at),
  df.jahanahayes %>%
    select(text,screen_name,created_at),
  df.lbr %>%
    select(text,screen_name,created_at),
  df.eleanornorton %>%
    select(text,screen_name,created_at),
  df.stephmurphy %>%
    select(text,screen_name,created_at),
  df.kcastor %>%
    select(text,screen_name,created_at),
  df.hastings %>%
    select(text,screen_name,created_at),
  df.loisfrankel %>%
    select(text,screen_name,created_at),
  df.dwstweets %>%
    select(text,screen_name,created_at),
  df.wilson %>%
    select(text,screen_name,created_at),
  df.dmp %>%
    select(text,screen_name,created_at),
  df.shalala %>%
    select(text,screen_name,created_at),
  df.tulsi %>%
    select(text,screen_name,created_at),
  df.finkenauer %>%
    select(text,screen_name,created_at),
  df.cindyaxne %>%
    select(text,screen_name,created_at),
  df.robinkelly %>%
    select(text,screen_name,created_at),
  df.janschakowsky %>%
    select(text,screen_name,created_at),
  df.underwood %>%
    select(text,screen_name,created_at),
  df.cheri %>%
    select(text,screen_name,created_at),
  df.davids %>%
    select(text,screen_name,created_at),
  df.loritrahan %>%
    select(text,screen_name,created_at),
  df.kclark %>%
    select(text,screen_name,created_at),
  df.pressley %>%
    select(text,screen_name,created_at),
  df.haleystevens %>%
    select(text,screen_name,created_at),
  df.debdingell %>%
    select(text,screen_name,created_at),
  df.rashida %>%
    select(text,screen_name,created_at),
  df.lawrence %>%
    select(text,screen_name,created_at),
  df.angiecraig %>%
    select(text,screen_name,created_at),
  df.deanphillips %>%
    select(text,screen_name,created_at),
  df.bettymccollum %>%
    select(text,screen_name,created_at),
  df.ilhan %>%
    select(text,screen_name,created_at),
  df.lacyclay %>%
    select(text,screen_name,created_at),
  df.adams %>%
    select(text,screen_name,created_at),
  df.anniekuster %>%
    select(text,screen_name,created_at),
  df.bonnie %>%
    select(text,screen_name,created_at),
  df.debhaaland %>%
    select(text,screen_name,created_at),
  df.torressmall %>%
    select(text,screen_name,created_at),
  df.dinatitus %>%
    select(text,screen_name,created_at),
  df.susielee %>%
    select(text,screen_name,created_at),
  df.kathleenrice %>%
    select(text,screen_name,created_at),
  df.nydiavelazquez %>%
    select(text,screen_name,created_at),
  df.yvetteclarke %>%
    select(text,screen_name,created_at),
  df.maloney %>%
    select(text,screen_name,created_at),
  df.aoc %>%
    select(text,screen_name,created_at),
  df.nitalowey %>%
    select(text,screen_name,created_at),
  df.beatty %>%
    select(text,screen_name,created_at),
  df.marcykaptur %>%
    select(text,screen_name,created_at),
  df.marciafudge %>%
    select(text,screen_name,created_at),
  df.kendrahorn %>%
    select(text,screen_name,created_at),
  df.bonamici %>%
    select(text,screen_name,created_at),
  df.mgs %>%
    select(text,screen_name,created_at),
  df.dean %>%
    select(text,screen_name,created_at),
  df.houlahan %>%
    select(text,screen_name,created_at),
  df.susanwild %>%
    select(text,screen_name,created_at),
  df.fletcher %>%
    select(text,screen_name,created_at),
  df.escobar %>%
    select(text,screen_name,created_at),
  df.jacksonlee %>%
    select(text,screen_name,created_at),
  df.sylviagarcia %>%
    select(text,screen_name,created_at),
  df.elaineluria %>%
    select(text,screen_name,created_at),
  df.spanberger %>%
    select(text,screen_name,created_at),
  df.wexton %>%
    select(text,screen_name,created_at),
  df.delbene %>%
    select(text,screen_name,created_at),
  df.herrerabeutler %>%
    select(text,screen_name,created_at),
  df.jayapal %>%
    select(text,screen_name,created_at),
  df.kimschrier %>%
    select(text,screen_name,created_at),
  df.gwenmoore %>%
    select(text,screen_name,created_at))

#Tokenization of FD HOR
replace_reg <- "http[s]?://[A-Za-z\\d/\\.]+|&amp;|&lt;|&gt;"
unnest_reg  <- "([^A-Za-z_\\d#@']|'(?![A-Za-z_\\d#@]))"
df.horfd.tidy<- df.horfd %>%
  filter(!str_detect(text, "^RT")) %>%
  mutate(text = str_replace_all(text,replace_reg, "")) %>%
  mutate(id = row_number()) %>%
  unnest_tokens(
    word, text, token = "regex", pattern = unnest_reg) %>%
  filter(!word %in% stop_words$word, str_detect(word, "[a-z]"))





  #Male Republicans

donyoung <- get_timeline('@repdonyoung', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
byrne <- get_timeline('@RepByrne', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikerogers <- get_timeline('@RepMikeRogersAL', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
robertaderholt <- get_timeline('@Robert_Aderholt', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mobrooks <- get_timeline('@RepMoBrooks', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
garypalmer <- get_timeline('@USRepGaryPalmer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rickcrawford <- get_timeline('@RepRickCrawford', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
frenchhill<- get_timeline('@RepFrenchHill', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stevewomack <- get_timeline('@rep_stevewomack', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
westerman <- get_timeline('@RepWesterman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gosar <- get_timeline('@RepGosar', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
andybiggs <- get_timeline('@RepAndyBiggsAZ', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
david <- get_timeline('@RepDavid', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dlesko <- get_timeline('@RepDLesko', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lamalfa<- get_timeline('@RepLaMalfa', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mcclintock <- get_timeline('@RepMcClintock', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
paulcook <- get_timeline('@RepPaulCook', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
devinnunes <- get_timeline('@RepDevinNunes', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kevinmccarthy <- get_timeline('@GOPLeader', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kencalvert<- get_timeline('@KenCalvert', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tipton <- get_timeline('@RepTipton', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kenbuck <- get_timeline('@RepKenBuck', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dlamborn <- get_timeline('@RepDLamborn', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mattgaetz <- get_timeline('@RepMattGaetz', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
nealdunn <- get_timeline('@DrNealDunnFL2', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tedyoho <- get_timeline('@RepTedYoho', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rutherford <- get_timeline('@RepRutherfordFL', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
michaelwaltz <- get_timeline('@RepMichaelWaltz', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
billposey <- get_timeline('@congbillposey', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
webster <- get_timeline('@RepWebster', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gusbilirakis <- get_timeline('@RepGusBilirakis', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
vernbuchanan<- get_timeline('@VernBuchanan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gregsteube <- get_timeline('@RepGregSteube', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
brianmast <- get_timeline('@RepBrianMast', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rooney <- get_timeline('@RepRooney', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mariodb <- get_timeline('@MarioDB', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
buddycarter <- get_timeline('@RepBuddyCarter', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
drewferguson <- get_timeline('@RepDrewFerguson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
robwoodall<- get_timeline('@RepRobWoodall', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
austinscott <- get_timeline('@AustinScottGA08', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dougcollins <- get_timeline('@RepDougCollins', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hice <- get_timeline('@CongressmanHice', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
loudermilk <- get_timeline('@RepLoudermilk', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rickallen <- get_timeline('@RepRickAllen', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tomgraves <- get_timeline('@RepTomGraves', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
russfulcher <- get_timeline('@RepRussFulcher', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikesimpson <- get_timeline('@CongMikeSimpson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bost <- get_timeline('@RepBost', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rodneydavis <- get_timeline('@RodneyDavis', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
shimkus <- get_timeline('@RepShimkus', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kinzinger <- get_timeline('@RepKinzinger', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lahood <- get_timeline('@RepLaHood', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jimbanks <- get_timeline('@RepJimBanks', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jimbaird <- get_timeline('@RepJimBaird', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gregpence <- get_timeline('@RepGregPence', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
larrybucshon <- get_timeline('@RepLarryBucshon', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
trey<- get_timeline('@RepTrey', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ronestes <- get_timeline('@RepRonEstes', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
guthrie <- get_timeline('@RepGuthrie', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
thomasmassie <- get_timeline('@RepThomasMassie', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
halrogers<- get_timeline('@RepHalRogers', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
andybarr <- get_timeline('@RepAndyBarr', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stevescalise <- get_timeline('@SteveScalise', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
clayhiggins <- get_timeline('@RepClayHiggins', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikejohnson <- get_timeline('@RepMikeJohnson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
abraham <- get_timeline('@RepAbraham', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
garretgraves <- get_timeline('@RepGarretGraves', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
andyharris<- get_timeline('@RepAndyHarrisMD', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jackbergman <- get_timeline('@RepJackBergman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
huizenga <- get_timeline('@RepHuizenga', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
justinamash <- get_timeline('@justinamash', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
moolenaar<- get_timeline('@RepMoolenaar', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
fredupton <- get_timeline('@RepFredUpton', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
walberg <- get_timeline('@RepWalberg', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
slotkin <- get_timeline('@RepSlotkin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
paulmitchell <- get_timeline('@RepPaulMitchell', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hagedorn<- get_timeline('@RepHagedorn', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tomemmer <- get_timeline('@RepTomEmmer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
petestauber <- get_timeline('@RepPeteStauber', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
blaine <- get_timeline('@RepBlaine', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
samgraves<- get_timeline('@RepSamGraves', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
long <- get_timeline('@USRepLong', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jasonsmith <- get_timeline('@RepJasonSmith', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
trentkelly <- get_timeline('@RepTrentKelly', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
michaelguest <- get_timeline('@RepMichaelGuest', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
palazzo<- get_timeline('@CongPalazzo', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
holding <- get_timeline('@RepHolding', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
walterjones <- get_timeline('@RepWalterJones', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
markwalker <- get_timeline('@RepMarkWalker', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davidrouzer <- get_timeline('@RepDavidRouzer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
richhudson <- get_timeline('@RepRichHudson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
patrickmchenry <- get_timeline('@PatrickMcHenry', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
markmeadows <- get_timeline('@RepMarkMeadows', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tedbudd <- get_timeline('@RepTedBudd', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jefffortenberry <- get_timeline('@JeffFortenberry', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
donbacon<- get_timeline('@RepDonBacon', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
adriansmith <- get_timeline('@RepAdrianSmith', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jvd <- get_timeline('@CongressmanJVD', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chrissmith <- get_timeline('@RepChrisSmith', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
markamodei <- get_timeline('@MarkAmodeiNV2', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
leezeldin <- get_timeline('@RepLeeZeldin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
peterking <- get_timeline('@RepPeteKing', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tomreed <- get_timeline('@RepTomReed', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnkatko <- get_timeline('@RepJohnKatko', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chriscollins <- get_timeline('@RepChrisCollins', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stevechabot <- get_timeline('@RepSteveChabot', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bradwenstrup<- get_timeline('@RepBradWenstrup', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jimjordan<- get_timeline('@Jim_Jordan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
boblatta <- get_timeline('@boblatta', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
billjohnson <- get_timeline('@RepBillJohnson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bobgibbs <- get_timeline('@RepBobGibbs', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
warrendavidson <- get_timeline('@WarrenDavidson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
miketurner <- get_timeline('@RepMikeTurner', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
balderson <- get_timeline('@RepBalderson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davejoyce <- get_timeline('@RepDaveJoyce', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stevestivers <- get_timeline('@RepSteveStivers', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
agonzalez<- get_timeline('@RepAGonzalez', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kevinhern <- get_timeline('@repkevinhern', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mullin <- get_timeline('@RepMullin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
franklucas <- get_timeline('@RepFrankLucas', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tomcole <- get_timeline('@TomColeOK04', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
brianfitz <- get_timeline('@RepBrianFitz', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
meuser <- get_timeline('@RepMeuser', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
scottperry <- get_timeline('@RepScottPerry', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
smucker <- get_timeline('@RepSmucker', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnjoyce <- get_timeline('@RepJohnJoyce', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
greschenthaler <- get_timeline('@GReschenthaler', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gt <- get_timeline('@CongressmanGT', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikekelly <- get_timeline('@MikeKellyPA', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joewilson <- get_timeline('@RepJoeWilson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jeffduncan <- get_timeline('@RepJeffDuncan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
timmons <- get_timeline('@reptimmons', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ralphnorman <- get_timeline('@RepRalphNorman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tomrice <- get_timeline('@RepTomRice', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dustyjohnson <- get_timeline('@RepDustyJohnson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
philroe <- get_timeline('@DrPhilRoe', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
timburchett <- get_timeline('@RepTimBurchett', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chuck <- get_timeline('@RepChuck', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
desjarlais <- get_timeline('@DesJarlaisTN04l', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnrose <- get_timeline('@RepJohnRose', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
markgreen <- get_timeline('@RepMarkGreen', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davidkustoff <- get_timeline('@RepDavidKustoff', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
louiegohmert <- get_timeline('@replouiegohmert', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dancrenshaw<- get_timeline('@RepDanCrenshaw', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
vantaylor <- get_timeline('@RepVanTaylor', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ratcliffe <- get_timeline('@RepRatcliffe', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lancegooden <- get_timeline('@RepLanceGooden', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ronwright <- get_timeline('@RepRonWright', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kevinbrady <- get_timeline('@RepKevinBrady', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mccaul <- get_timeline('@RepMcCaul', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
conaway <- get_timeline('@ConawayTX11', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kaygranger <- get_timeline('@RepKayGranger', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mac <- get_timeline('@MacTXPress', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
randy <- get_timeline('@TXRandy14', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
billflores <- get_timeline('@RepBillFlores', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
arrington <- get_timeline('@RepArrington', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chiproy <- get_timeline('@RepChipRoy', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
peteolson <- get_timeline('@RepPeteOlson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hurd <- get_timeline('@HurdOnTheHill', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
kenmarchant <- get_timeline('@RepKenMarchant', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rwilliams <- get_timeline('@RepRWilliams', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
michaelburgess <- get_timeline('@michaelcburgess', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cloud<- get_timeline('@RepCloudTX', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
carter <- get_timeline('@JudgeCarter', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
brianbabin <- get_timeline('@RepBrianBabin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
robbishop <- get_timeline('@RepRobBishop', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chrisstewart <- get_timeline('@RepChrisStewart', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johncurtis <- get_timeline('@RepJohnCurtis', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
robwittman <- get_timeline('@RobWittman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
riggleman <- get_timeline('@RepRiggleman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bencline <- get_timeline('@RepBenCline', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mgriffith <- get_timeline('@RepMGriffith', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
newhouse <- get_timeline('@RepNewhouse', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bryansteil <- get_timeline('@RepBryanSteil', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jim <- get_timeline('@JimPressOffice', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
grothman <- get_timeline('@RepGrothman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
seanduffy <- get_timeline('@RepSeanDuffy', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gallagher <- get_timeline('@RepGallagher', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mckinley <- get_timeline('@RepMcKinley', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
alexmooney <- get_timeline('@RepAlexMooney', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)


df.donyoung <- data.frame(donyoung)
df.byrne <- data.frame(byrne)
df.mikerogers <- data.frame(mikerogers)
df.robertalderholt <- data.frame(robertaderholt)
df.mobrooks <- data.frame(mobrooks)
df.garypalmer <- data.frame(garypalmer)
df.rickcrawford <- data.frame(rickcrawford)
df.frenchhill <- data.frame(frenchhill)
df.stevewomack <- data.frame(stevewomack)
df.westerman <- data.frame(westerman)
df.gosar <- data.frame(gosar)
df.andybiggs <- data.frame(andybiggs)
df.david <- data.frame(david)
df.dlesko <- data.frame(dlesko)
df.lamalfa <- data.frame(lamalfa)
df.mcclintock <- data.frame(mcclintock)
df.paulcook <- data.frame(paulcook)
df.devinnunes <- data.frame(devinnunes)
df.kevinmccarthy <- data.frame(kevinmccarthy)
df.kencalvert <- data.frame(kencalvert)
df.tipton <- data.frame(tipton)
df.kenbuck <- data.frame(kenbuck)
df.dlamborn <- data.frame(dlamborn)
df.mattgaetz <- data.frame(mattgaetz)
df.nealdunn <- data.frame(nealdunn)
df.tedyoho <- data.frame(tedyoho)
df.rutherford <- data.frame(rutherford)
df.michaelwaltz <- data.frame(michaelwaltz)
df.billposey <- data.frame(billposey)
df.webster<- data.frame(webster)
df.gusbilirakis <- data.frame(gusbilirakis)
df.vernbuchanan <- data.frame(vernbuchanan)
df.gregsteube <- data.frame(gregsteube)
df.brianmast <- data.frame(brianmast)
df.rooney <- data.frame(rooney)
df.mariodb <- data.frame(mariodb)
df.buddycarter <- data.frame(buddycarter)
df.drewferguson <- data.frame(drewferguson)
df.robwoodall <- data.frame(robwoodall)
df.austinscott <- data.frame(austinscott)
df.dougcollins <- data.frame(dougcollins)
df.hice <- data.frame(hice)
df.loudermilk <- data.frame(loudermilk)
df.rickallen <- data.frame(rickallen)
df.tomgraves <- data.frame(tomgraves)
df.russfulcher <- data.frame(russfulcher)
df.mikesimpson <- data.frame(mikesimpson)
df.bost <- data.frame(bost)
df.rodneydavis <- data.frame(rodneydavis)
df.shimkus <- data.frame(shimkus)
df.kinzinger <- data.frame(kinzinger)
df.lahood <- data.frame(lahood)
df.jimbanks <- data.frame(jimbanks)
df.jimbaird <- data.frame(jimbaird)
df.gregpence <- data.frame(gregpence)
df.larrybucshon <- data.frame(larrybucshon)
df.trey <- data.frame(trey)
df.ronestes <- data.frame(ronestes)
df.guthrie <- data.frame(guthrie)
df.thomasmassie <- data.frame(thomasmassie)
df.halrogers <- data.frame(halrogers)
df.andybarr <- data.frame(andybarr)
df.stevecalise <- data.frame(stevescalise)
df.clayhiggins <- data.frame(clayhiggins)
df.mikejohnson <- data.frame(mikejohnson)
df.abraham <- data.frame(abraham)
df.garretgraves <- data.frame(garretgraves)
df.andyharris <- data.frame(andyharris)
df.jackbergman <- data.frame(jackbergman)
df.huizenga <- data.frame(huizenga)
df.justinamash <- data.frame(justinamash)
df.moolenaar <- data.frame(moolenaar)
df.fredupton <- data.frame(fredupton)
df.walberg <- data.frame(walberg)
df.slotkin <- data.frame(slotkin)
df.paulmitchell <- data.frame(paulmitchell)
df.hagedorn <- data.frame(hagedorn)
df.tomemmer <- data.frame(tomemmer)
df.petestauber <- data.frame(petestauber)
df.blaine <- data.frame(blaine)
df.samgraves <- data.frame(samgraves)
df.long <- data.frame(long)
df.jasonsmith <- data.frame(jasonsmith)
df.trentkelly <- data.frame(trentkelly)
df.michaelguest <- data.frame(michaelguest)
df.palazzo <- data.frame(palazzo)
df.holding <- data.frame(holding)
df.walterjones <- data.frame(walterjones)
df.markwalker <- data.frame(markwalker)
df.davidrouzer <- data.frame(davidrouzer)
df.richhudson <- data.frame(richhudson)
df.patrickmchenry <- data.frame(patrickmchenry)
df.markmeadows <- data.frame(markmeadows)
df.tedbudd <- data.frame(tedbudd)
df.jeffforetenberry <- data.frame(jefffortenberry)
df.donbacon <- data.frame(donbacon)
df.adriansmith <- data.frame(adriansmith)
df.jvd <- data.frame(jvd)
df.chrissmith <- data.frame(chrissmith)
df.markamodei <- data.frame(markamodei)
df.leezeldin <- data.frame(leezeldin)
df.peterking <- data.frame(peterking)
df.tomreed <- data.frame(tomreed)
df.johnkatko <- data.frame(johnkatko)
df.chriscollins <- data.frame(chriscollins)
df.stevechabot <- data.frame(stevechabot)
df.bradwenstrup <- data.frame(bradwenstrup)
df.jimjordan <- data.frame(jimjordan)
df.boblatta <- data.frame(boblatta)
df.billjohnson <- data.frame(billjohnson)
df.bobgibbs <- data.frame(bobgibbs)
df.warrendavidson <- data.frame(warrendavidson)
df.miketurner <- data.frame(miketurner)
df.balderson <- data.frame(balderson)
df.davejoyce <- data.frame(davejoyce)
df.stevestivers <- data.frame(stevestivers)
df.agonzalez <- data.frame(agonzalez)
df.kevinhern <- data.frame(kevinhern)
df.mullin <- data.frame(mullin)
df.franklucas <- data.frame(franklucas)
df.tomcole <- data.frame(tomcole)
df.brianfitz <- data.frame(brianfitz)
df.meuser <- data.frame(meuser)
df.scottperry <- data.frame(scottperry)
df.smucker <- data.frame(smucker)
df.johnjoyce <- data.frame(johnjoyce)
df.greschenthaler <- data.frame(greschenthaler)
df.gt <- data.frame(gt)
df.mikekelly <- data.frame(mikekelly)
df.joewilson <- data.frame(joewilson)
df.jeffduncan <- data.frame(jeffduncan)
df.timmons <- data.frame(timmons)
df.ralphnorman <- data.frame(ralphnorman)
df.tomrice <- data.frame(tomrice)
df.dustyjohnson <- data.frame(dustyjohnson)
df.philroe <- data.frame(philroe)
df.timburchett <- data.frame(timburchett)
df.chuck <- data.frame(chuck)
df.desjarlais <- data.frame(desjarlais)
df.johnrose <- data.frame(johnrose)
df.markgreen <- data.frame(markgreen)
df.davidkustoff <- data.frame(davidkustoff)
df.louiegohmert <- data.frame(louiegohmert)
df.dancrenshaw <- data.frame(dancrenshaw)
df.vantaylor <- data.frame(vantaylor)
df.ratcliffe <- data.frame(ratcliffe)
df.lancegooden <- data.frame(lancegooden)
df.ronwright <- data.frame(ronwright)
df.kevinbrady <- data.frame(kevinbrady)
df.mccaul <- data.frame(mccaul)
df.conaway <- data.frame(conaway)
df.kaygranger <- data.frame(kaygranger)
df.mac <- data.frame(mac)
df.randy <- data.frame(randy)
df.billflores <- data.frame(billflores)
df.arrington <- data.frame(arrington)
df.chiproy <- data.frame(chiproy)
df.peteolson <- data.frame(peteolson)
df.hurd <- data.frame(hurd)
df.kenmarchant <- data.frame(kenmarchant)
df.rwilliams <- data.frame(rwilliams)
df.michaelburgess <- data.frame(michaelburgess)
df.cloud <- data.frame(cloud)
df.carter <- data.frame(carter)
df.brianbabin <- data.frame(brianbabin)
df.robbishop <- data.frame(robbishop)
df.chrisstewart <- data.frame(chrisstewart)
df.johncurtis <- data.frame(johncurtis)
df.robwittman <- data.frame(robwittman)
df.riggleman <- data.frame(riggleman)
df.bencline <- data.frame(bencline)
df.mgriffith <- data.frame(mgriffith)
df.newhouse <- data.frame(newhouse)
df.bryansteil <- data.frame(bryansteil)
df.jim <- data.frame(jim)
df.grothman <- data.frame(grothman)
df.seanduffy <- data.frame(seanduffy)
df.gallagher <- data.frame(gallagher)
df.mckinley <- data.frame(mckinley)
df.alexmooney <- data.frame(alexmooney)

df.hormr <- bind_rows(
  df.donyoung %>%
    select(text, screen_name, created_at),
  df.byrne %>%
    select(text, screen_name, created_at),
  df.mikerogers %>%
    select(text, screen_name, created_at),
  df.robertalderholt %>%
    select(text, screen_name, created_at),
  df.mobrooks %>%
    select(text, screen_name, created_at),
  df.garypalmer %>%
    select(text, screen_name, created_at),
  df.rickcrawford %>%
    select(text, screen_name, created_at),
  df.frenchhill %>%
    select(text, screen_name, created_at),
  df.stevewomack %>%
    select(text, screen_name, created_at),
  df.westerman %>%
    select(text, screen_name, created_at),
  df.gosar %>%
    select(text, screen_name, created_at),
  df.andybiggs %>%
    select(text, screen_name, created_at),
  df.david %>%
    select(text, screen_name, created_at),
  df.dlesko %>%
    select(text, screen_name, created_at),
  df.lamalfa %>%
    select(text, screen_name, created_at),
  df.mcclintock%>%
    select(text, screen_name, created_at),
  df.paulcook%>%
    select(text, screen_name, created_at),
  df.devinnunes%>%
    select(text, screen_name, created_at),
  df.kevinmccarthy %>%
    select(text, screen_name, created_at),
  df.kencalvert %>%
    select(text, screen_name, created_at),
  df.tipton%>%
    select(text, screen_name, created_at),
  df.kenbuck%>%
    select(text, screen_name, created_at),
  df.dlamborn %>%
    select(text, screen_name, created_at),
  df.mattgaetz %>%
    select(text, screen_name, created_at),
  df.nealdunn%>%
    select(text, screen_name, created_at),
  df.tedyoho %>%
    select(text, screen_name, created_at),
  df.rutherford %>%
    select(text, screen_name, created_at),
  df.michaelwaltz %>%
    select(text, screen_name, created_at),
  df.billposey %>%
    select(text, screen_name, created_at),
  df.webster %>%
    select(text, screen_name, created_at),
  df.gusbilirakis %>%
    select(text, screen_name, created_at),
  df.vernbuchanan %>%
    select(text, screen_name, created_at),
  df.gregsteube %>%
    select(text, screen_name, created_at),
  df.brianmast %>%
    select(text, screen_name, created_at),
  df.rooney %>%
    select(text, screen_name, created_at),
  df.mariodb %>%
    select(text, screen_name, created_at),
  df.buddycarter %>%
    select(text, screen_name, created_at),
  df.drewferguson %>%
    select(text, screen_name, created_at),
  df.robwoodall %>%
    select(text, screen_name, created_at),
  df.austinscott %>%
    select(text, screen_name, created_at),
  df.dougcollins %>%
    select(text, screen_name, created_at),
  df.hice %>%
    select(text, screen_name, created_at),
  df.loudermilk %>%
    select(text, screen_name, created_at),
  df.rickallen %>%
    select(text, screen_name, created_at),
  df.tomgraves %>%
    select(text, screen_name, created_at),
  df.russfulcher %>%
    select(text, screen_name, created_at),
  df.mikesimpson %>%
    select(text, screen_name, created_at),
  df.bost %>%
    select(text, screen_name, created_at),
  df.rodneydavis %>%
    select(text, screen_name, created_at),
  df.shimkus %>%
    select(text, screen_name, created_at),
  df.kinzinger %>%
    select(text, screen_name, created_at),
  df.lahood %>%
    select(text, screen_name, created_at),
  df.jimbanks %>%
    select(text, screen_name, created_at),
  df.jimbaird %>%
    select(text, screen_name, created_at),
  df.gregpence %>%
    select(text, screen_name, created_at),
  df.larrybucshon %>%
    select(text, screen_name, created_at),
  df.trey %>%
    select(text, screen_name, created_at),
  df.ronestes %>%
    select(text, screen_name, created_at),
  df.guthrie %>%
    select(text, screen_name, created_at),
  df.thomasmassie %>%
    select(text, screen_name, created_at),
  df.halrogers %>%
    select(text, screen_name, created_at),
  df.andybarr %>%
    select(text, screen_name, created_at),
  df.stevecalise %>%
    select(text, screen_name, created_at),
  df.clayhiggins %>%
    select(text, screen_name, created_at),
  df.mikejohnson %>%
    select(text, screen_name, created_at),
  df.abraham %>%
    select(text, screen_name, created_at),
  df.garretgraves %>%
    select(text, screen_name, created_at),
  df.andyharris %>%
    select(text, screen_name, created_at),
  df.jackbergman %>%
    select(text, screen_name, created_at),
  df.huizenga %>%
    select(text, screen_name, created_at),
  df.justinamash %>%
    select(text, screen_name, created_at),
  df.moolenaar %>%
    select(text, screen_name, created_at),
  df.fredupton %>%
    select(text, screen_name, created_at),
  df.walberg %>%
    select(text, screen_name, created_at),
  df.slotkin %>%
    select(text, screen_name, created_at),
  df.paulmitchell %>%
    select(text, screen_name, created_at),
  df.hagedorn %>%
    select(text, screen_name, created_at),
  df.tomemmer %>%
    select(text, screen_name, created_at),
  df.petestauber %>%
    select(text, screen_name, created_at),
  df.blaine %>%
    select(text, screen_name, created_at),
  df.samgraves %>%
    select(text, screen_name, created_at),
  df.long %>%
    select(text, screen_name, created_at),
  df.jasonsmith %>%
    select(text, screen_name, created_at),
  df.trentkelly %>%
    select(text, screen_name, created_at),
  df.michaelguest %>%
    select(text, screen_name, created_at),
  df.palazzo %>%
    select(text, screen_name, created_at),
  df.holding %>%
    select(text, screen_name, created_at),
  df.walterjones %>%
    select(text, screen_name, created_at),
  df.markwalker %>%
    select(text, screen_name, created_at),
  df.davidrouzer %>%
    select(text, screen_name, created_at),
  df.richhudson%>%
    select(text, screen_name, created_at),
  df.patrickmchenry %>%
    select(text, screen_name, created_at),
  df.markmeadows %>%
    select(text, screen_name, created_at),
  df.tedbudd %>%
    select(text, screen_name, created_at),
  df.jeffforetenberry %>%
    select(text, screen_name, created_at),
  df.donbacon %>%
    select(text, screen_name, created_at),
  df.adriansmith %>%
    select(text, screen_name, created_at),
  df.jvd %>%
    select(text, screen_name, created_at),
  df.chrissmith %>%
    select(text, screen_name, created_at),
  df.markamodei %>%
    select(text, screen_name, created_at),
  df.leezeldin %>%
    select(text, screen_name, created_at),
  df.peterking %>%
    select(text, screen_name, created_at),
  df.tomreed %>%
    select(text, screen_name, created_at),
  df.johnkatko %>%
    select(text, screen_name, created_at),
  df.stevechabot %>%
    select(text, screen_name, created_at),
  df.bradwenstrup %>%
    select(text, screen_name, created_at),
  df.jimjordan %>%
    select(text, screen_name, created_at),
  df.boblatta %>%
    select(text, screen_name, created_at),
  df.billjohnson %>%
    select(text, screen_name, created_at),
  df.bobgibbs %>%
    select(text, screen_name, created_at),
  df.warrendavidson %>%
    select(text, screen_name, created_at),
  df.miketurner %>%
    select(text, screen_name, created_at),
  df.balderson %>%
    select(text, screen_name, created_at),
  df.davejoyce %>%
    select(text, screen_name, created_at),
  df.stevestivers %>%
    select(text, screen_name, created_at),
  df.agonzalez %>%
    select(text, screen_name, created_at),
  df.kevinhern %>%
    select(text, screen_name, created_at),
  df.mullin %>%
    select(text, screen_name, created_at),
  df.franklucas %>%
    select(text, screen_name, created_at),
  df.tomcole %>%
    select(text, screen_name, created_at),
  df.brianfitz %>%
    select(text, screen_name, created_at),
  df.meuser %>%
    select(text, screen_name, created_at),
  df.scottperry %>%
    select(text, screen_name, created_at),
  df.smucker %>%
    select(text, screen_name, created_at),
  df.johnjoyce %>%
    select(text, screen_name, created_at),
  df.greschenthaler %>%
    select(text, screen_name, created_at),
  df.gt %>%
    select(text, screen_name, created_at),
  df.mikekelly %>%
    select(text, screen_name, created_at),
  df.joewilson %>%
    select(text, screen_name, created_at),
  df.jeffduncan %>%
    select(text, screen_name, created_at),
  df.timmons %>%
    select(text, screen_name, created_at),
  df.ralphnorman %>%
    select(text, screen_name, created_at),
  df.tomrice %>%
    select(text, screen_name, created_at),
  df.dustyjohnson %>%
    select(text, screen_name, created_at),
  df.philroe %>%
    select(text, screen_name, created_at),
  df.timburchett %>%
    select(text, screen_name, created_at),
  df.chuck %>%
    select(text, screen_name, created_at),
  df.johnrose %>%
    select(text, screen_name, created_at),
  df.markgreen %>%
    select(text, screen_name, created_at),
  df.davidkustoff %>%
    select(text, screen_name, created_at),
  df.louiegohmert %>%
    select(text, screen_name, created_at),
  df.dancrenshaw %>%
    select(text, screen_name, created_at),
  df.vantaylor %>%
    select(text, screen_name, created_at),
  df.ratcliffe %>%
    select(text, screen_name, created_at),
  df.ronwright %>%
    select(text, screen_name, created_at),
  df.kevinbrady %>%
    select(text, screen_name, created_at),
  df.mccaul %>%
    select(text, screen_name, created_at),
  df.conaway %>%
    select(text, screen_name, created_at),
  df.kaygranger %>%
    select(text, screen_name, created_at),
  df.mac %>%
    select(text, screen_name, created_at),
  df.randy %>%
    select(text, screen_name, created_at),
  df.billflores %>%
    select(text, screen_name, created_at),
  df.arrington %>%
    select(text, screen_name, created_at),
  df.chiproy %>%
    select(text, screen_name, created_at),
  df.peteolson %>%
    select(text, screen_name, created_at),
  df.hurd %>%
    select(text, screen_name, created_at),
  df.kenmarchant %>%
    select(text, screen_name, created_at),
  df.rwilliams %>%
    select(text, screen_name, created_at),
  df.michaelburgess %>%
    select(text, screen_name, created_at),
  df.cloud %>%
    select(text, screen_name, created_at),
  df.carter %>%
    select(text, screen_name, created_at),
  df.brianbabin %>%
    select(text, screen_name, created_at),
  df.chrisstewart %>%
    select(text, screen_name, created_at),
  df.johncurtis %>%
    select(text, screen_name, created_at),
  df.robwittman %>%
    select(text, screen_name, created_at),
  df.riggleman %>%
    select(text, screen_name, created_at),
  df.bencline %>%
    select(text, screen_name, created_at),
  df.mgriffith %>%
    select(text, screen_name, created_at),
  df.newhouse %>%
    select(text, screen_name, created_at),
  df.bryansteil %>%
    select(text, screen_name, created_at),
  df.jim %>%
    select(text, screen_name, created_at),
  df.grothman %>%
    select(text, screen_name, created_at),
  df.gallagher %>%
    select(text, screen_name, created_at),
  df.mckinley %>%
    select(text, screen_name, created_at),
  df.alexmooney %>%
    select(text, screen_name, created_at))

replace_reg <- "http[s]?://[A-Za-z\\d/\\.]+|&amp;|&lt;|&gt;"
unnest_reg  <- "([^A-Za-z_\\d#@']|'(?![A-Za-z_\\d#@]))"
df.hormr.tidy<- df.hormr %>%
  filter(!str_detect(text, "^RT")) %>%
  mutate(text = str_replace_all(text,replace_reg, "")) %>%
  mutate(id = row_number()) %>%
  unnest_tokens(
    word, text, token = "regex", pattern = unnest_reg) %>%
  filter(!word %in% stop_words$word, str_detect(word, "[a-z]"))





#Male Democrats House of Representatives

ohalleran <- get_timeline('@RepOHalleran', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
raulgrijalva<- get_timeline('@RepRaulGrijalva', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rubengallego <- get_timeline('@RepRubenGallego', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gregstanton<- get_timeline('@RepGregStanton', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
garamendi<- get_timeline('@RepGaramendi', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
thompson<- get_timeline('@RepThompson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mcnerney <- get_timeline('@RepMcNerney', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joshharder <- get_timeline('@RepJoshHarder', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
desaulnier<- get_timeline('@RepDeSaulnier', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
swalwell<- get_timeline('@RepSwalwell', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jimcosta <- get_timeline('@RepJimCosta', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
rokhanna <- get_timeline('@RepRoKhanna', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jimmypanetta <- get_timeline('@RepJimmyPanetta', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tjcox<- get_timeline('@RepTjCox', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
carbajal <- get_timeline('@RepCarbajal', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
adamschiff <- get_timeline('@RepAdamSchiff', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cardenas<- get_timeline('@RepCardenas', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bradsherman <- get_timeline('@BradSherman', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
peteaguilar<- get_timeline('@RepPeteAguilar', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tedlieu<- get_timeline('@RepTedLieu', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jimmygomez <- get_timeline('@RepJimmyGomez', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ruiz<- get_timeline('@CongressmanRuiz', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gilcisneros <- get_timeline('@RepGilCisneros', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
marktakano<- get_timeline('@RepMarkTakano', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
loucorrea<- get_timeline('@RepLouCorrea', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lowenthal<- get_timeline('@RepLowenthal', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
harley<- get_timeline('@RepHarley', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikelevin <- get_timeline('@RepMikeLevin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hunter<- get_timeline('@Rep_Hunter', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
juanvargas <- get_timeline('@RepJuanVargas', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
scottpeters<- get_timeline('@RepScottPeters', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joeneguse<- get_timeline('@RepJoeNeguse', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jasoncrow<- get_timeline('@RepJasonCrow', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
perlmutter<- get_timeline('@RepPerlmutter', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnlarson<- get_timeline('@RepJohnLarson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joecourtney<- get_timeline('@RepJoeCourtney', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jahimes<- get_timeline('@jahimes', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
allawson <- get_timeline('@RepAlLawsonJr', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
darrensoto <- get_timeline('@RepDarrenSoto', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
valdemings<- get_timeline('@RepValDemings', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
charliecrist<- get_timeline('@RepCharlieCrist', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
teddeutch<- get_timeline('@RepTedDeutch', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
sanfordbishop<- get_timeline('@SanfordBishop', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hankjohnson<- get_timeline('@RepHankJohnson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnlewis<- get_timeline('@repjohnlewis', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lucymcbath<- get_timeline('@RepLucyMcBath', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davidscott<- get_timeline('@repdavidscott', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
edcase<- get_timeline('@RepEdCase', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
daveloebsack <- get_timeline('@daveloebsack', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bobbyrush<- get_timeline('@RepBobbyRush', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lipinski<- get_timeline('@RepLipinski', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chuygarcia <- get_timeline('@RepChuyGarcia', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikequigley<- get_timeline('@RepMikeQuigley', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
casten<- get_timeline('@RepCasten', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dannydavis <- get_timeline('@RepDannyDavis', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
raja<- get_timeline('@CongressmanRaja', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
schneider <- get_timeline('@RepSchneider', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
billfoster<- get_timeline('@RepBillFoster', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
visclosky<- get_timeline('@RepVisclosky', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
andrecarson<- get_timeline('@RepAndreCarson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
johnyarmuth<- get_timeline('@RepJohnYarmuth', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
richmond<- get_timeline('@RepRichmond', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chelliepingree <- get_timeline('@chelliepingree', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
golden<- get_timeline('@RepGolden', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dutch <- get_timeline('@Call_Me_Dutch', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
sarbanes <- get_timeline('@RepSarbanes', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
anthonybrown <- get_timeline('@RepAnthonyBrown', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
hoyer<- get_timeline('@LeaderHoyer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davidtrone<- get_timeline('@RepDavidTrone', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cummings<- get_timeline('@RepCummings', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
raskin <- get_timeline('@RepRaskin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
richardneal <- get_timeline('@RepRichardNeal', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mcgovern<- get_timeline('@RepMcGovern', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joekennedy <- get_timeline('@RepJoeKennedy', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
moulton<- get_timeline('@teammoulton', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
stephenlynch <- get_timeline('@RepStephenLynch', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
keating<- get_timeline('@USRepKeating', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dankildee <- get_timeline('@RepDanKildee', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
andylevin<- get_timeline('@RepAndyLevin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cleaver<- get_timeline('@repcleaver', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
benniethompson <- get_timeline('@BennieGThompson', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gkbutterfield<- get_timeline('@GKButterfield', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davidprice<- get_timeline('@RepDavidEPrice', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
chrispappas<- get_timeline('@RepChrisPappas', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
donaldnorcross<- get_timeline('@DonaldNorcross', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
andykim<- get_timeline('@RepAndyKimNJ', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joshg <- get_timeline('@RepJoshG', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
frankpallone <- get_timeline('@FrankPallone', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
malinowski<- get_timeline('@RepMalinowski', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
sires<- get_timeline('@RepSires', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
billpascrell <- get_timeline('@BillPascrell', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
donaldpayne<- get_timeline('@RepDonaldPayne', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
sherrill<- get_timeline('@RepSherrill', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
benraylujan <- get_timeline('@repbenraylujan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
horsford<- get_timeline('@RepHorsford', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
tomsuozzi <- get_timeline('@RepTomSuozzi', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gregorymeeks<- get_timeline('@RepGregoryMeeks', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gracemeng<- get_timeline('@RepGraceMeng', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jeffries<- get_timeline('@RepJeffries', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jerrynadler <- get_timeline('@RepJerryNadler', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
maxrose<- get_timeline('@RepMaxRose', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
espaillat <- get_timeline('@RepEspaillat', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joseserrano<- get_timeline('@RepJoseSerrano', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
eliotengel<- get_timeline('@RepEliotEngel', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
seanmaloney<- get_timeline('@RepSeanMaloney', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
delgado<- get_timeline('@repdelgado', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
paultonko <- get_timeline('@RepPaulTonko', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
brindisi<- get_timeline('@RepBrindisi', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joemorelle <- get_timeline('@RepJoeMorelle', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
brianhiggins<- get_timeline('@RepBrianHiggins', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
timryan<- get_timeline('@RepTimRyan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gregwalden <- get_timeline('@repgregwalden', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
blumenauer<- get_timeline('@repblumenauer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
peterdefazio<- get_timeline('@RepPeterDeFazio', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
schrader<- get_timeline('@RepSchrader', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
boyle <- get_timeline('@CongBoyle', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dwightevans <- get_timeline('@RepDwightEvans', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cartwright<- get_timeline('@RepCartwright', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
conorlamb<- get_timeline('@RepConorLamb', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mikedoyle<- get_timeline('@USRepMikeDoyle', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
davidcicilline<- get_timeline('@davidcicilline', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jimlangevin<- get_timeline('@JimLangevin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cunningham<- get_timeline('@RepCunningham', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
clyburn<- get_timeline('@WhipClyburn', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
jimcooper <- get_timeline('@repjimcooper', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cohen<- get_timeline('@RepCohen', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
algreen <- get_timeline('@RepAlGreen', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gonzalez <- get_timeline('@RepGonzalez', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
joaquincastro <- get_timeline('@JoaquinCastrotx', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
cuellar<- get_timeline('@RepCuellar', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ebj <- get_timeline('@RepEBJ', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
colinallred <- get_timeline('@RepColinAllred', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
veasey<- get_timeline('@RepVeasey', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
filemonvela <- get_timeline('@RepFilemonVela', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
lloyddoggett<- get_timeline('@RepLloydDoggett', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
benmcadams<- get_timeline('@RepBenMcAdams', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
bobbyscott<- get_timeline('@BobbyScott', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
mceachin<- get_timeline('@RepMcEachin', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
donbeyer <- get_timeline('@RepDonBeyer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
gerryconnolly <- get_timeline('@GerryConnolly', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
peterwelch<- get_timeline('@PeterWelch', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ricklarsen<- get_timeline('@RepRickLarsen', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
derekkilmer<- get_timeline('@RepDerekKilmer', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
adamsmith<- get_timeline('@RepAdamSmith', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
dennyheck<- get_timeline('@RepDennyHeck', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
markpocan<- get_timeline('@repmarkpocan', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
ronkind<- get_timeline('@RepRonKind', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)
carolmiller <- get_timeline('@RepCarolMiller', n = 50, max_id = NULL, home = FALSE, parse = TRUE, check = FALSE, token = token, include_rts = FALSE)

df.ohalleran <- data.frame(ohalleran)
df.raulgrijvala <- data.frame(raulgrijalva)
df.rubengallego <- data.frame(rubengallego)
df.gregstanton <- data.frame(gregstanton)
df.garamendi <- data.frame(garamendi)
df.thompson <- data.frame(thompson)
df.mcnerney <- data.frame(mcnerney)
df.joshharder <- data.frame(joshharder)
df.desaulnier <- data.frame(desaulnier)
df.swalwell <- data.frame(swalwell)
df.jimcosta <- data.frame(jimcosta)
df.rokhanna <- data.frame(rokhanna)
df.jimmypanetta <- data.frame(jimmypanetta)
df.tjcox <- data.frame(tjcox)
df.carbajal <- data.frame(carbajal)
df.adamschiff <- data.frame(adamschiff)
df.cardenas <- data.frame(cardenas)
df.bradsherman <- data.frame(bradsherman)
df.peteaguilar <- data.frame(peteaguilar)
df.tedlieu <- data.frame(tedlieu)
df.jimmygomez <- data.frame(jimmygomez)
df.ruiz <- data.frame(ruiz)
df.gilcisneros <- data.frame(gilcisneros)
df.marktakano <- data.frame(marktakano)
df.loucorrea <- data.frame(loucorrea)
df.lowenthal <- data.frame(lowenthal)
df.harley <- data.frame(harley)
df.mikelevin <- data.frame(mikelevin)
df.hunter <- data.frame(hunter)
df.juanvargas <- data.frame(juanvargas)
df.scottpeters <- data.frame(scottpeters)
df.joeneguse <- data.frame(joeneguse)
df.jasoncrow <- data.frame(jasoncrow)
df.perlmutter <- data.frame(perlmutter)
df.johnlarson <- data.frame(johnlarson)
df.joecourtney <- data.frame(joecourtney)
df.jahimes <- data.frame(jahimes)
df.allawson <- data.frame(allawson)
df.darrensoto <- data.frame(darrensoto)
df.valdemings <- data.frame(valdemings)
df.charliecrist <- data.frame(charliecrist)
df.teddeutch <- data.frame(teddeutch)
df.sanfordbishop <- data.frame(sanfordbishop)
df.hankjohnson <- data.frame(hankjohnson)
df.johnlewis <- data.frame(johnlewis)
df.lucymcbath <- data.frame(lucymcbath)
df.davidscott <- data.frame(davidscott)
df.edcase <- data.frame(edcase)
df.daveloebsack <- data.frame(daveloebsack)
df.bobbyrush <- data.frame(bobbyrush)
df.lipinski <- data.frame(lipinski)
df.chuygarcia <- data.frame(chuygarcia)
df.mikequigley <- data.frame(mikequigley)
df.casten <- data.frame(casten)
df.dannydavis <- data.frame(dannydavis)
df.raja <- data.frame(raja)
df.schneider <- data.frame(schneider)
df.billfoster <- data.frame(billfoster)
df.visclosky <- data.frame(visclosky)
df.andrecarson <- data.frame(andrecarson)
df.johnyarmuth <- data.frame(johnyarmuth)
df.richmond <- data.frame(richmond)
df.chelliepingree <- data.frame(chelliepingree)
df.golden <- data.frame(golden)
df.dutch <- data.frame(dutch)
df.sarbanes <- data.frame(sarbanes)
df.anthonybrown <- data.frame(anthonybrown)
df.hoyer <- data.frame(hoyer)
df.davidtrone <- data.frame(davidtrone)
df.cummings <- data.frame(cummings)
df.raskin <- data.frame(raskin)
df.richardneal <- data.frame(richardneal)
df.mcgovern <- data.frame(mcgovern)
df.joekennedy <- data.frame(joekennedy)
df.moulton <- data.frame(moulton)
df.stephenlynch <- data.frame(stephenlynch)
df.keating <- data.frame(keating)
df.dankildee <- data.frame(dankildee)
df.andylevin <- data.frame(andylevin)
df.cleaver <- data.frame(cleaver)
df.benniethompson <- data.frame(benniethompson)
df.gkbutterfield <- data.frame(gkbutterfield)
df.davidprice <- data.frame(davidprice)
df.chrispappas <- data.frame(chrispappas)
df.donaldnorcross <- data.frame(donaldnorcross)
df.andykim <- data.frame(andykim)
df.joshg <- data.frame(joshg)
df.frankpallone <- data.frame(frankpallone)
df.malinowski <- data.frame(malinowski)
df.sires <- data.frame(sires)
df.billpascrell <- data.frame(billpascrell)
df.donaldpayne <- data.frame(donaldpayne)
df.sherrill <- data.frame(sherrill)
df.benraylujan <- data.frame(benraylujan)
df.horsford <- data.frame(horsford)
df.tomsuozzi <- data.frame(tomsuozzi)
df.gregorymeeks <- data.frame(gregorymeeks)
df.gracemeng <- data.frame(gracemeng)
df.jeffries <- data.frame(jeffries)
df.jerrynadler <- data.frame(jerrynadler)
df.maxrose <- data.frame(maxrose)
df.espaillat <- data.frame(espaillat)
df.joseserrano <- data.frame(joseserrano)
df.eliotengel <- data.frame(eliotengel)
df.seanmaloney <- data.frame(seanmaloney)
df.delgado <- data.frame(delgado)
df.paultonko <- data.frame(paultonko)
df.brindisi <- data.frame(brindisi)
df.joemorelle <- data.frame(joemorelle)
df.brianhiggins <- data.frame(brianhiggins)
df.timryan <- data.frame(timryan)
df.gregwalden <- data.frame(gregwalden)
df.blumenauer <- data.frame(blumenauer)
df.peterdefazio <- data.frame(peterdefazio)
df.schrader <- data.frame(schrader)
df.boyle <- data.frame(boyle)
df.dwightevans <- data.frame(dwightevans)
df.cartwright <- data.frame(cartwright)
df.conorlamb <- data.frame(conorlamb)
df.mikedoyle <- data.frame(mikedoyle)
df.davidcicilline <- data.frame(davidcicilline)
df.jimlangevin <- data.frame(jimlangevin)
df.cunningham <- data.frame(cunningham)
df.clyburn <- data.frame(clyburn)
df.jimcooper <- data.frame(jimcooper)
df.cohen <- data.frame(cohen)
df.algreen <- data.frame(algreen)
df.gonzalez <- data.frame(gonzalez)
df.joaquincastro <- data.frame(joaquincastro)
df.cuellar <- data.frame(cuellar)
df.ebj <- data.frame(ebj)
df.colinallred <- data.frame(colinallred)
df.veasey <- data.frame(veasey)
df.filemonvela <- data.frame(filemonvela)
df.lloyddoggett <- data.frame(lloyddoggett)
df.benmcadams <- data.frame(benmcadams)
df.bobbyscott <- data.frame(bobbyscott)
df.mceachin <- data.frame(mceachin)
df.donbeyer <- data.frame(donbeyer)
df.gerryconnolly <- data.frame(gerryconnolly)
df.peterwelch <- data.frame(peterwelch)
df.ricklarson <- data.frame(ricklarsen)
df.derekkilmer <- data.frame(derekkilmer)
df.adamsmith <- data.frame(adamsmith)
df.dennyheck <- data.frame(dennyheck)
df.markpocan <- data.frame(markpocan)
df.ronkind <- data.frame(ronkind)
df.carolmiller <- data.frame(carolmiller)

df.hormd <- bind_rows(
  df.ohalleran %>%
    select(text, screen_name, created_at),
  df.raulgrijvala %>%
    select(text, screen_name, created_at),
  df.rubengallego %>%
    select(text, screen_name, created_at),
  df.gregstanton %>%
    select(text, screen_name, created_at),
  df.garamendi %>%
    select(text, screen_name, created_at),
  df.thompson %>%
    select(text, screen_name, created_at),
  df.mcnerney %>%
    select(text, screen_name, created_at),
  df.joshharder %>%
    select(text, screen_name, created_at),
  df.desaulnier %>%
    select(text, screen_name, created_at),
  df.swalwell %>%
    select(text, screen_name, created_at),
  df.jimcosta %>%
    select(text, screen_name, created_at),
  df.rokhanna %>%
    select(text, screen_name, created_at),
  df.jimmypanetta %>%
    select(text, screen_name, created_at),
  df.tjcox %>%
    select(text, screen_name, created_at),
  df.carbajal %>%
    select(text, screen_name, created_at),
  df.adamschiff %>%
    select(text, screen_name, created_at),
  df.cardenas %>%
    select(text, screen_name, created_at),
  df.bradsherman %>%
    select(text, screen_name, created_at),
  df.peteaguilar %>%
    select(text, screen_name, created_at),
  df.tedlieu %>%
    select(text, screen_name, created_at),
  df.jimmygomez %>%
    select(text, screen_name, created_at),
  df.ruiz %>%
    select(text, screen_name, created_at),
  df.gilcisneros %>%
    select(text, screen_name, created_at),
  df.marktakano %>%
    select(text, screen_name, created_at),
  df.loucorrea %>%
    select(text, screen_name, created_at),
  df.lowenthal %>%
    select(text, screen_name, created_at),
  df.harley %>%
    select(text, screen_name, created_at),
  df.mikelevin %>%
    select(text, screen_name, created_at),
  df.hunter %>%
    select(text, screen_name, created_at),
  df.juanvargas %>%
    select(text, screen_name, created_at),
  df.scottpeters %>%
    select(text, screen_name, created_at),
  df.joeneguse %>%
    select(text, screen_name, created_at),
  df.jasoncrow %>%
    select(text, screen_name, created_at),
  df.perlmutter %>%
    select(text, screen_name, created_at),
  df.johnlarson %>%
    select(text, screen_name, created_at),
  df.joecourtney %>%
    select(text, screen_name, created_at),
  df.jahimes %>%
    select(text, screen_name, created_at),
  df.allawson %>%
    select(text, screen_name, created_at),
  df.darrensoto %>%
    select(text, screen_name, created_at),
  df.valdemings %>%
    select(text, screen_name, created_at),
  df.charliecrist %>%
    select(text, screen_name, created_at),
  df.teddeutch %>%
    select(text, screen_name, created_at),
  df.sanfordbishop %>%
    select(text, screen_name, created_at),
  df.hankjohnson %>%
    select(text, screen_name, created_at),
  df.johnlewis %>%
    select(text, screen_name, created_at),
  df.lucymcbath %>%
    select(text, screen_name, created_at),
  df.davidscott %>%
    select(text, screen_name, created_at),
  df.edcase %>%
    select(text, screen_name, created_at),
  df.daveloebsack %>%
    select(text, screen_name, created_at),
  df.bobbyrush %>%
    select(text, screen_name, created_at),
  df.lipinski %>%
    select(text, screen_name, created_at),
  df.chuygarcia %>%
    select(text, screen_name, created_at),
  df.mikequigley %>%
    select(text, screen_name, created_at),
  df.casten %>%
    select(text, screen_name, created_at),
  df.dannydavis %>%
    select(text, screen_name, created_at),
  df.raja %>%
    select(text, screen_name, created_at),
  df.schneider %>%
    select(text, screen_name, created_at),
  df.billfoster %>%
    select(text, screen_name, created_at),
  df.visclosky %>%
    select(text, screen_name, created_at),
  df.andrecarson %>%
    select(text, screen_name, created_at),
  df.johnyarmuth %>%
    select(text, screen_name, created_at),
  df.richmond %>%
    select(text, screen_name, created_at),
  df.chelliepingree %>%
    select(text, screen_name, created_at),
  df.golden %>%
    select(text, screen_name, created_at),
  df.dutch %>%
    select(text, screen_name, created_at),
  df.sarbanes %>%
    select(text, screen_name, created_at),
  df.anthonybrown %>%
    select(text, screen_name, created_at),
  df.hoyer %>%
    select(text, screen_name, created_at),
  df.davidtrone %>%
    select(text, screen_name, created_at),
  df.cummings %>%
    select(text, screen_name, created_at),
  df.raskin %>%
    select(text, screen_name, created_at),
  df.richardneal %>%
    select(text, screen_name, created_at),
  df.mcgovern %>%
    select(text, screen_name, created_at),
  df.joekennedy %>%
    select(text, screen_name, created_at),
  df.moulton %>%
    select(text, screen_name, created_at),
  df.stephenlynch %>%
    select(text, screen_name, created_at),
  df.keating %>%
    select(text, screen_name, created_at),
  df.dankildee %>%
    select(text, screen_name, created_at),
  df.andylevin %>%
    select(text, screen_name, created_at),
  df.cleaver %>%
    select(text, screen_name, created_at),
  df.benniethompson %>%
    select(text, screen_name, created_at),
  df.gkbutterfield %>%
    select(text, screen_name, created_at),
  df.davidprice %>%
    select(text, screen_name, created_at),
  df.chrispappas %>%
    select(text, screen_name, created_at),
  df.donaldnorcross %>%
    select(text, screen_name, created_at),
  df.andykim %>%
    select(text, screen_name, created_at),
  df.joshg %>%
    select(text, screen_name, created_at),
  df.frankpallone %>%
    select(text, screen_name, created_at),
  df.malinowski %>%
    select(text, screen_name, created_at),
  df.sires %>%
    select(text, screen_name, created_at),
  df.billpascrell %>%
    select(text, screen_name, created_at),
  df.donaldpayne %>%
    select(text, screen_name, created_at),
  df.sherrill %>%
    select(text, screen_name, created_at),
  df.benraylujan %>%
    select(text, screen_name, created_at),
  df.horsford %>%
    select(text, screen_name, created_at),
  df.tomsuozzi %>%
    select(text, screen_name, created_at),
  df.gregorymeeks %>%
    select(text, screen_name, created_at),
  df.gracemeng %>%
    select(text, screen_name, created_at),
  df.jeffries %>%
    select(text, screen_name, created_at),
  df.jerrynadler %>%
    select(text, screen_name, created_at),
  df.maxrose %>%
    select(text, screen_name, created_at),
  df.espaillat %>%
    select(text, screen_name, created_at),
  df.joseserrano %>%
    select(text, screen_name, created_at),
  df.eliotengel %>%
    select(text, screen_name, created_at),
  df.seanmaloney %>%
    select(text, screen_name, created_at),
  df.delgado %>%
    select(text, screen_name, created_at),
  df.paultonko %>%
    select(text, screen_name, created_at),
  df.brindisi %>%
    select(text, screen_name, created_at),
  df.joemorelle %>%
    select(text, screen_name, created_at),
  df.brianhiggins %>%
    select(text, screen_name, created_at),
  df.timryan %>%
    select(text, screen_name, created_at),
  df.gregwalden %>%
    select(text, screen_name, created_at),
  df.blumenauer %>%
    select(text, screen_name, created_at),
  df.peterdefazio %>%
    select(text, screen_name, created_at),
  df.schrader %>%
    select(text, screen_name, created_at),
  df.boyle %>%
    select(text, screen_name, created_at),
  df.dwightevans %>%
    select(text, screen_name, created_at),
  df.cartwright %>%
    select(text, screen_name, created_at),
  df.conorlamb %>%
    select(text, screen_name, created_at),
  df.mikedoyle %>%
    select(text, screen_name, created_at),
  df.davidcicilline %>%
    select(text, screen_name, created_at),
  df.jimlangevin %>%
    select(text, screen_name, created_at),
  df.cunningham %>%
    select(text, screen_name, created_at),
  df.clyburn %>%
    select(text, screen_name, created_at),
  df.jimcooper %>%
    select(text, screen_name, created_at),
  df.cohen %>%
    select(text, screen_name, created_at),
  df.algreen %>%
    select(text, screen_name, created_at),
  df.gonzalez %>%
    select(text, screen_name, created_at),
  df.joaquincastro %>%
    select(text, screen_name, created_at),
  df.cuellar %>%
    select(text, screen_name, created_at),
  df.ebj %>%
    select(text, screen_name, created_at),
  df.colinallred %>%
    select(text, screen_name, created_at),
  df.veasey %>%
    select(text, screen_name, created_at),
  df.filemonvela %>%
    select(text, screen_name, created_at),
  df.lloyddoggett %>%
    select(text, screen_name, created_at),
  df.benmcadams %>%
    select(text, screen_name, created_at),
  df.bobbyscott %>%
    select(text, screen_name, created_at),
  df.mceachin %>%
    select(text, screen_name, created_at),
  df.donbeyer %>%
    select(text, screen_name, created_at),
  df.gerryconnolly %>%
    select(text, screen_name, created_at),
  df.peterwelch %>%
    select(text, screen_name, created_at),
  df.ricklarson %>%
    select(text, screen_name, created_at),
  df.derekkilmer %>%
    select(text, screen_name, created_at),
  df.adamsmith %>%
    select(text, screen_name, created_at),
  df.dennyheck %>%
    select(text, screen_name, created_at),
  df.markpocan %>%
    select(text, screen_name, created_at),
  df.ronkind %>%
    select(text, screen_name, created_at),
  df.carolmiller %>%
    select(text, screen_name, created_at)
)

replace_reg <- "http[s]?://[A-Za-z\\d/\\.]+|&amp;|&lt;|&gt;"
unnest_reg  <- "([^A-Za-z_\\d#@']|'(?![A-Za-z_\\d#@]))"
df.hormd.tidy<- df.hormd %>%
  filter(!str_detect(text, "^RT")) %>%
  mutate(text = str_replace_all(text,replace_reg, "")) %>%
  mutate(id = row_number()) %>%
  unnest_tokens(
    word, text, token = "regex", pattern = unnest_reg) %>%
  filter(!word %in% stop_words$word, str_detect(word, "[a-z]"))


require(foreign)
write.dta(df.horfd.tidy, "df_horfd_tidy.dta")
write.dta(df.horfr.tidy, "df_horfr_tidy.dta")
write.dta(df.hormd.tidy, "df_hormd_tidy.dta")
write.dta(df.hormr.tidy, "df_hormr_tidy.dta")
write.dta(df.senatefd.tidy, "df_senatefd_tidy.dta")
write.dta(df.senatefr.tidy, "df_senatefr_tidy.dta")
write.dta(df.senatemr.tidy, "df_senatemr_tidy.dta")
write.dta(df.senatemd.tidy, "df_senatemd_tidy.dta")

```
